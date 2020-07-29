---
title: Linux man page学习之open
date: 2020-07-28 14:23:00
categories: Linux知识
index_img: /img/articles/open.jpg
tags:
	- Linux
	- open
---

本文简介了类UNIX系统下（主要为Linux）的open相关函数的内容。

<!-- more -->

# 原型

对于open相关函数的原型为：

~~~c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);

int creat(const char *pathname, mode_t mode);

int openat(int dirfd, const char *pathname, int flags);
int openat(int dirfd, const char *pathname, int flags, mode_t mode);
~~~

# 简介

open()系统调用打开参数pathname指定的文件。如果指定的文件不存在，并且flag参数的O_CREAT选项被设置，则文件将会被open()系统调用创建（creat）。

open()系统调用的返回值是一个文件描述符。文件描述符是一个非负的整数，用来引用该打开的文件，一般返回的文件描述符是当前进程的未打开的文件描述符的最小整数值，即open()系统调用总是返回当前进程可用的文件描述符最小值。

打开的文件描述符记录了文件的偏移值，以及文件的状态标志（file status flags）。

如果获取了一个open的文件描述符，随后，pathname参数被修改为指向一个不同的文件，或者被删除，则原先获得的文件描述符将不受影响，依然引用原先打开的文件。

对于flags参数，除了必须设置的O_RDINLY、O_WRONLY与O_RDWR以外，还可以设置零个或者多个标志位，这些标志位可以分为两组：

- the file creation flags，影响open操作本身的语义，包括O_CLOEXEC、O_CREAT、O_DIRECTORY、O_EXCL、O_NOCTTY、O_NOFOLLOW、O_TMPFILE、O_TRUNC
- the file status flags，影响随后的IO操作的语义，包括O_APPEND、O_ASYNC、O_DSYNC、O_LARGEFILE、O_NOATIME、O_NONBLOCK or O_NDELAY、O_PATH、O_SYNC、O_TMPFILE

# file flags

## O_APPEND

文件以append模式被打开，每一次write之前，文件的偏移值都被设置为文件的末尾。

改变文件的偏移值至末尾，与写操作是单独的一个原子操作。

在NFS文件系统中，如果多个进程对同一个文件同时进行追加写操作，则会引起该文件的错误。

## O_ASYNC

在该文件描述符上，若可读或者可写，则产生一个信号，默认信号为SIGIO，可通过fcntl修改该默认信号。该特性只可以使用于终端、伪终端、sockets、pipes与FIFOs。

## O_CLOEXEC

对于O_CLOEXEC模式和FD_CLOEXEC选项的总结为：

- 调用open函数O_CLOEXEC模式打开的文件描述符在执行exec调用新程序中关闭，且为原子操作。
- 调用open函数不使用O_CLOEXEC模式打开的文件描述符，然后调用fcntl 函数设置FD_CLOEXEC选项，效果和使用O_CLOEXEC选项open函数相同，但分别调用open、fcnt两个函数，不是原子操作，多线程环境中存在竞态条件，故用open函数O_CLOEXEC选项代替之。
- 调用open函数O_CLOEXEC模式打开的文件描述符，或是使用fcntl设置FD_CLOEXEC选项，这二者得到（处理）的描述符在通过fork调用产生的子进程中均不被关闭。
- 调用dup族类函数得到的新文件描述符将清除O_CLOEXEC模式。

为了防止在多线程的程序中出现竞争导致的错误，一般需要设置该标志位，因为相比调用opne与fcntl的两步操作，设置该标志位为原子操作。

## O_CREAT

如果open()系统调用的参数pathname不存在，且指定该标志位，则将创建一个普通文件。

新文件的user id将被设置为进程的有效用户ID（the effective user id）。

新文件的group id在类UNIX系统中，可能会被设置为进程的有效组ID（the effective group id），也可能会被设置为父目录的group id。对于Linux系统，若文件的父目录mode的set-group-ID位被设置，则新文件的group id会被设置为父目录的group id，否则，新文件的group id会被设置为进程的有效组ID。

open()系统调用的参数mode用来设定新建的文件的mode。当flags的O_CREAT或者O_TMPFILE被设置，则必须提供mode参数，否则mode参数将被忽略。

通常情况下，有效的mode将受到进程的umask影响，如果没有默认ACL，则新建文件的mode值为（mode & ~umask）。

另外，若open()系统调用通过mode创建的新文件为只读文件（read-only），但是返回的文件描述符也是可以为读写类型的文件描述符。

对于mode，可以取以下的值：

- S_IRWXU  00700 user (file owner) has read, write, and execute permission
- S_IRUSR  00400 user has read permission
- S_IWUSR  00200 user has write permission
- S_IXUSR  00100 user has execute permission
- S_IRWXG  00070 group has read, write, and execute permission
- S_IRGRP  00040 group has read permission
- S_IWGRP  00020 group has write permission
- S_IXGRP  00010 group has execute permission
- S_IRWXO  00007 others have read, write, and execute permission
- S_IROTH  00004 others have read permission
- S_IWOTH  00002 others have write permission
- S_IXOTH  00001 others have execute permission
- S_ISUID  0004000 set-user-ID bit
- S_ISGID  0002000 set-group-ID bit (see inode(7)).
- S_ISVTX  0001000 sticky bit (see inode(7)).

## O_DIRECT

本节对O_DIRECT与O_SYNC标志位进行解释，并进行比对。

### 直接IO

Linux系统运行应用程序在执行磁盘的IO操作时，绕过缓冲区高速缓存，从用户空间直接将数据传递到磁盘上的文件中，或者磁盘设备中，这样的操作被称为直接IO（direct IO），或者裸IO（raw IO）。

直接IO的应用场景可以是数据库系统，其高速缓存与IO优化机制自成一体，因此可以省去内核的高速缓存机制。

但是使用直接IO，可能会大大降低性能，一般不推荐使用。

使用直接IO需要遵守以下限制，若不遵守任一限制，将导致EINVAL错误：

- 用于传递数据的缓冲区，其内存边界必须对齐为块大小的整数倍；
- 数据传输的起点，即文件或者设备的偏移值，必须为块大小的整数倍；
- 待传递的数据的长度必须是块大小的整数倍。

**使用直接IO的方式，就是在open()系统调用中，设置flags的O_DIRECT标志位。**

注意：若某个进程以O_DIRECT标志打开某个文件，而另一个进程以普通方式打开同一个文件（即使用高速缓存），则由直接IO所读写的数据，与缓冲区高速缓存中的内容会存在不一致，应该避免该情况的发生。

### 同步方式写文件

在某些时候，为了数据的安全，需要强制刷新内核缓冲区的数据至文件或者设备。

在这里先介绍两个概念：

- synchronized IO data integrity completion：确保针对文件的一次更新传递了足够的信息（部分文件元数据）至磁盘，以便于后续对数据的读取；
- synchronized IO file integrity completion：确保针对文件的一次更新传递了所有的信息（所有文件元数据）至磁盘，即使有些在后续对文件数据的操作并不需要。

内核提供的用于控制文件IO内核缓存的系统调用包括：

- fsync系统调用，将使得缓冲数据与相关元数据都刷新到磁盘上，会强制文件处于synchronized IO file integrity completion状态；
- fdatasync系统调用，会强制文件处于synchronized IO data integrity completion状态。与fsync相比，fdatasync可能会减少磁盘操作的次数；
- sync系统调用，会使得包含更新文件信息的所有内核缓冲区数据（数据块、指针块、元数据等）刷新到磁盘上。若内容发生变化的内核缓冲区在30s内未经显式方式同步到磁盘上，则一条长期运行的内核线程会确保将其刷新到磁盘上。这一做法是为了规避缓冲区与相关磁盘文件内容长期处于不一致状态。

而调用open()系统调用时，如果设置O_SYNC标志位，会使得所有的后续输出同步，即每个write调用会自动将文件数据和元数据刷新到磁盘上，即按照Synchronized IO file integrity completion的要求执行写操作。

## O_DIRECTORY

如果参数pathname不是目录，则open()系统调用返回失败。

## O_DSYNC

设置该标志位，则write操作将按照synchronized IO data integrity completion的规范要求完成对文件的写入。

其效果类似于write操作后，紧跟着一个fdatasync系统调用。

## O_EXCL

如果该标志位与O_CREAT一起被设置，且pathname指向的文件已经存在，则open()系统调用返回失败，失败码EEXIST。否则，将创建文件。

如果该标志位与O_CREAT一起被设置，且pathname为一个符号链接，则符号链接不跟随。

一般情况下，如果没有设置O_CREAT标志的情况下，设置了O_EXCL标志，则其行为是未定义的。一个例外是：在内核2.6版本以后，如果pathname指向一个块设备（block device），则可以单独设置O_EXCL标志。

## O_LARGEFILE

设置该标志位，则允许大文件被打开。（大文件指的是文件偏移值不能通过off_t来表示，但是可以通过off64_t来表示的文件）

若使用该标志，则宏 _LARGEFILE64_SOURCE 需要被定义。

在32位系统上，相比该标志，通过设置宏 _FILE_OFFSET_BITS 为64，是更好的方法。

## O_NOATIME

设置该标志，则文件被读操作时，不更新文件的最后访问时间（last access time）。该标志在内核2.6.8版本以后才被支持。

该标志对应功能生效，需满足以下任一条件：

- 进程的有效用户ID必须等于文件的拥有者ID
- 调用的进程在其namespace具备CAP_FOWNER，且文件的拥有者ID在该namespace有映射

## O_NOCTTY

如果pathname指向的是一个终端设备，则设置该标志，该终端设备不会成为调用进程的控制终端。

## O_NOFOLLOW

如果pathname引用的是一个符号链接，则open()系统调用将失败，返回错误码ELOOP。

## O_NONBLOCK or O_NDELAY

如果设置该标志，则文件将以非阻塞模式打开。

注意：该标志对普通文件与块设备无效，即无论该标志是否设置，当设备需要时。IO操作还是会被阻塞。因此对于普通文件与块设备，应用程序不应该依赖于该标志来设置阻塞与非阻塞行为。

## O_PATH

使用该标志，则open()系统调用将不会真正的打开一个文件，而只是准备好该文件的文件描述符。

对于获取的文件描述符，不能用于read、write、fchmod、fchown、fgetxattr、ioctl、mmap等系统调用。

对于获取的文件描述符，可以用于：

- close
- fchdir（若文件描述符指向目录）
- fstat
- fstatfs
- 复制文件描述符（dup、fcntl F_DUPFD等）
- 获取与设置文件描述符标志位（fcntl F_SETFD F_GETFD）

当该标志位被设置，则 O_CLOEXEC、O_DIRECTORY、与O_NOFOLLOW标志位将被忽略。

## O_SYNC

见 O_DIRECT 节相关介绍。

## O_TMPFILE

创建一个未命名的临时普通文件。pathname参数为一个目录路径。

当文件描述符被关闭，任何写入文件的数据将丢失。

## O_TRUNC

如果文件已经存在，且为普通文件，写权限被打开，则open()系统调用后，文件将被截断为空。

如果文件为FIFO，或者终端设备，则该标志将被忽略。

其他类型的文件，设置该标志的结果未定义。

# openat

openat()系统调用具有与open()的大部分相同功能，除了以下的差异：

1. 如果参数pathname是相对路径，则pathname是相对于参数dirfd指向的目录，而对于open()系统调用，如果参数pathname是相对路径，则是相对于调用进程的当前工作目录。
2. 如果参数pathname是相对路径，且参数dirfd为AT_FDCWD，则pathname是相对于调用进程的当前工作目录，同open()系统调用相同。
3. 如果参数pathname是绝对路径，则参数dirfd将被忽略。

# creat
creat()系统调用，等同于open()系统调用，且设置标志位 O_CREAT|O_WRONLY|O_TRUNC

# 返回值

若成功，返回一个非负整数的文件描述符。

若失败，返回-1，且设置errno。