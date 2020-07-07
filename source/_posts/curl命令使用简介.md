---
title: curl命令使用简介
date: 2020-07-07 20:38:06
categories: Linux知识
tags:
	- Linux
	- 命令工具
---

## cURL

`cURL`是一个开源项目，其主要的产品是命令行工具`curl`，以及`C`语言`API`库`libcurl`。这两者的功能是基于网络协议，对指定的`URL`进行网络传输。

`cURL`设计任何网络协议传输，不涉及对具体数据的具体处理。

`cURL`的官方网站为：https://curl.haxx.se/



## 查看帮助

通过`--help`或者`-h`选项可以查看`curl`命令的帮助信息，具体为:

~~~shell
[jamza@jamza-vm etc]$ curl -h
Usage: curl [options...] <url>
Options: (H) means HTTP/HTTPS only, (F) means FTP only
     --anyauth       Pick "any" authentication method (H)
 -a, --append        Append to target file when uploading (F/SFTP)
     --basic         Use HTTP Basic Authentication (H)
     --cacert FILE   CA certificate to verify peer against (SSL)
     --capath DIR    CA directory to verify peer against (SSL)
 -E, --cert CERT[:PASSWD] Client certificate file and password (SSL)
     --cert-type TYPE Certificate file type (DER/PEM/ENG) (SSL)
     --ciphers LIST  SSL ciphers to use (SSL)
     --compressed    Request compressed response (using deflate or gzip)
 -K, --config FILE   Specify which config file to read
     --connect-timeout SECONDS  Maximum time allowed for connection
 -C, --continue-at OFFSET  Resumed transfer offset
 -b, --cookie STRING/FILE  String or file to read cookies from (H)
 -c, --cookie-jar FILE  Write cookies to this file after operation (H)
     --create-dirs   Create necessary local directory hierarchy
     --crlf          Convert LF to CRLF in upload
     --crlfile FILE  Get a CRL list in PEM format from the given file
 -d, --data DATA     HTTP POST data (H)
     --data-ascii DATA  HTTP POST ASCII data (H)
     --data-binary DATA  HTTP POST binary data (H)
     --data-urlencode DATA  HTTP POST data url encoded (H)
     --delegation STRING GSS-API delegation permission
     --digest        Use HTTP Digest Authentication (H)
     --disable-eprt  Inhibit using EPRT or LPRT (F)
     --disable-epsv  Inhibit using EPSV (F)
 -D, --dump-header FILE  Write the headers to this file
     --egd-file FILE  EGD socket path for random data (SSL)
     --engine ENGINGE  Crypto engine (SSL). "--engine list" for list
 -f, --fail          Fail silently (no output at all) on HTTP errors (H)
 -F, --form CONTENT  Specify HTTP multipart POST data (H)
     --form-string STRING  Specify HTTP multipart POST data (H)
     --ftp-account DATA  Account data string (F)
     --ftp-alternative-to-user COMMAND  String to replace "USER [name]" (F)
     --ftp-create-dirs  Create the remote dirs if not present (F)
     --ftp-method [MULTICWD/NOCWD/SINGLECWD] Control CWD usage (F)
     --ftp-pasv      Use PASV/EPSV instead of PORT (F)
 -P, --ftp-port ADR  Use PORT with given address instead of PASV (F)
     --ftp-skip-pasv-ip Skip the IP address for PASV (F)
     --ftp-pret      Send PRET before PASV (for drftpd) (F)
     --ftp-ssl-ccc   Send CCC after authenticating (F)
     --ftp-ssl-ccc-mode ACTIVE/PASSIVE  Set CCC mode (F)
     --ftp-ssl-control Require SSL/TLS for ftp login, clear for transfer (F)
 -G, --get           Send the -d data with a HTTP GET (H)
 -g, --globoff       Disable URL sequences and ranges using {} and []
 -H, --header LINE   Custom header to pass to server (H)
 -I, --head          Show document info only
 -h, --help          This help text
     --hostpubmd5 MD5  Hex encoded MD5 string of the host public key. (SSH)
 -0, --http1.0       Use HTTP 1.0 (H)
     --ignore-content-length  Ignore the HTTP Content-Length header
 -i, --include       Include protocol headers in the output (H/F)
 -k, --insecure      Allow connections to SSL sites without certs (H)
     --interface INTERFACE  Specify network interface/address to use
 -4, --ipv4          Resolve name to IPv4 address
 -6, --ipv6          Resolve name to IPv6 address
 -j, --junk-session-cookies Ignore session cookies read from file (H)
     --keepalive-time SECONDS  Interval between keepalive probes
     --key KEY       Private key file name (SSL/SSH)
     --key-type TYPE Private key file type (DER/PEM/ENG) (SSL)
     --krb LEVEL     Enable Kerberos with specified security level (F)
     --libcurl FILE  Dump libcurl equivalent code of this command line
     --limit-rate RATE  Limit transfer speed to this rate
 -l, --list-only     List only names of an FTP directory (F)
     --local-port RANGE  Force use of these local port numbers
 -L, --location      Follow redirects (H)
     --location-trusted like --location and send auth to other hosts (H)
 -M, --manual        Display the full manual
     --mail-from FROM  Mail from this address
     --mail-rcpt TO  Mail to this receiver(s)
     --mail-auth AUTH  Originator address of the original email
     --max-filesize BYTES  Maximum file size to download (H/F)
     --max-redirs NUM  Maximum number of redirects allowed (H)
 -m, --max-time SECONDS  Maximum time allowed for the transfer
     --metalink      Process given URLs as metalink XML file
     --negotiate     Use HTTP Negotiate Authentication (H)
 -n, --netrc         Must read .netrc for user name and password
     --netrc-optional Use either .netrc or URL; overrides -n
     --netrc-file FILE  Set up the netrc filename to use
 -N, --no-buffer     Disable buffering of the output stream
     --no-keepalive  Disable keepalive use on the connection
     --no-sessionid  Disable SSL session-ID reusing (SSL)
     --noproxy       List of hosts which do not use proxy
     --ntlm          Use HTTP NTLM authentication (H)
 -o, --output FILE   Write output to <file> instead of stdout
     --pass PASS     Pass phrase for the private key (SSL/SSH)
     --post301       Do not switch to GET after following a 301 redirect (H)
     --post302       Do not switch to GET after following a 302 redirect (H)
     --post303       Do not switch to GET after following a 303 redirect (H)
 -#, --progress-bar  Display transfer progress as a progress bar
     --proto PROTOCOLS  Enable/disable specified protocols
     --proto-redir PROTOCOLS  Enable/disable specified protocols on redirect
 -x, --proxy [PROTOCOL://]HOST[:PORT] Use proxy on given port
     --proxy-anyauth Pick "any" proxy authentication method (H)
     --proxy-basic   Use Basic authentication on the proxy (H)
     --proxy-digest  Use Digest authentication on the proxy (H)
     --proxy-negotiate Use Negotiate authentication on the proxy (H)
     --proxy-ntlm    Use NTLM authentication on the proxy (H)
 -U, --proxy-user USER[:PASSWORD]  Proxy user and password
     --proxy1.0 HOST[:PORT]  Use HTTP/1.0 proxy on given port
 -p, --proxytunnel   Operate through a HTTP proxy tunnel (using CONNECT)
     --pubkey KEY    Public key file name (SSH)
 -Q, --quote CMD     Send command(s) to server before transfer (F/SFTP)
     --random-file FILE  File for reading random data from (SSL)
 -r, --range RANGE   Retrieve only the bytes within a range
     --raw           Do HTTP "raw", without any transfer decoding (H)
 -e, --referer       Referer URL (H)
 -J, --remote-header-name Use the header-provided filename (H)
 -O, --remote-name   Write output to a file named as the remote file
     --remote-name-all Use the remote file name for all URLs
 -R, --remote-time   Set the remote file's time on the local output
 -X, --request COMMAND  Specify request command to use
     --resolve HOST:PORT:ADDRESS  Force resolve of HOST:PORT to ADDRESS
     --retry NUM   Retry request NUM times if transient problems occur
     --retry-delay SECONDS When retrying, wait this many seconds between each
     --retry-max-time SECONDS  Retry only within this period
 -S, --show-error    Show error. With -s, make curl show errors when they occur
 -s, --silent        Silent mode. Don't output anything
     --socks4 HOST[:PORT]  SOCKS4 proxy on given host + port
     --socks4a HOST[:PORT]  SOCKS4a proxy on given host + port
     --socks5 HOST[:PORT]  SOCKS5 proxy on given host + port
     --socks5-basic  Enable username/password auth for SOCKS5 proxies
     --socks5-gssapi Enable GSS-API auth for SOCKS5 proxies
     --socks5-hostname HOST[:PORT] SOCKS5 proxy, pass host name to proxy
     --socks5-gssapi-service NAME  SOCKS5 proxy service name for gssapi
     --socks5-gssapi-nec  Compatibility with NEC SOCKS5 server
 -Y, --speed-limit RATE  Stop transfers below speed-limit for 'speed-time' secs
 -y, --speed-time SECONDS  Time for trig speed-limit abort. Defaults to 30
     --ssl           Try SSL/TLS (FTP, IMAP, POP3, SMTP)
     --ssl-reqd      Require SSL/TLS (FTP, IMAP, POP3, SMTP)
 -2, --sslv2         Use SSLv2 (SSL)
 -3, --sslv3         Use SSLv3 (SSL)
     --ssl-allow-beast Allow security flaw to improve interop (SSL)
     --stderr FILE   Where to redirect stderr. - means stdout
     --tcp-nodelay   Use the TCP_NODELAY option
 -t, --telnet-option OPT=VAL  Set telnet option
     --tftp-blksize VALUE  Set TFTP BLKSIZE option (must be >512)
 -z, --time-cond TIME  Transfer based on a time condition
 -1, --tlsv1         Use => TLSv1 (SSL)
     --tlsv1.0       Use TLSv1.0 (SSL)
     --tlsv1.1       Use TLSv1.1 (SSL)
     --tlsv1.2       Use TLSv1.2 (SSL)
     --tlsv1.3       Use TLSv1.3 (SSL)
     --tls-max VERSION  Use TLS up to VERSION (SSL)
     --trace FILE    Write a debug trace to the given file
     --trace-ascii FILE  Like --trace but without the hex output
     --trace-time    Add time stamps to trace/verbose output
     --tr-encoding   Request compressed transfer encoding (H)
 -T, --upload-file FILE  Transfer FILE to destination
     --url URL       URL to work with
 -B, --use-ascii     Use ASCII/text transfer
 -u, --user USER[:PASSWORD]  Server user and password
     --tlsuser USER  TLS username
     --tlspassword STRING TLS password
     --tlsauthtype STRING  TLS authentication type (default SRP)
     --unix-socket FILE    Connect through this UNIX domain socket
 -A, --user-agent STRING  User-Agent to send to server (H)
 -v, --verbose       Make the operation more talkative
 -V, --version       Show version number and quit
 -w, --write-out FORMAT  What to output after completion
     --xattr        Store metadata in extended file attributes
 -q                 If used as the first parameter disables .curlrc

~~~



可以看出，`curl`命令支持的选项非常的多。 需要关注常用的选项即可。一般常用的选项包括：

1. `-X, --request COMMAND  Specify request command to use`
2. `-i, --include       Include protocol headers in the output (H/F)`
3. `-I, --head          Show document info only`
4. `-o, --output FILE   Write output to <file> instead of stdout`
5. `-L, --location      Follow redirects (H)`


## 查看网页源码

直接在`curl`命令后面加上网址，即可获取网页源码。

~~~shell
[jamza@jamza-vm etc]$ curl http://www.zte.com.cn/
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html>
<head><title>307 Temporary Redirect</title></head>
<body bgcolor="white">
<h1>307 Temporary Redirect</h1>
<p>The requested resource resides temporarily under a different URI.</p>
<hr/>Powered by ZTE</body>
</html>

~~~

若需要将网页源码内容缓存下来，可以使用`-o`选项，具体为：

~~~shell
curl -o filename http://www.zte.com.cn/
~~~

## 显示头部信息

参数`-i`可以显示`http response`的头部信息，显示还包括网页代码内容。

~~~shell
[jamza@jamza-vm test]$ curl -i http://www.zte.com.cn/
HTTP/1.1 307 Temporary Redirect
Server: ZTE
Date: Fri, 19 Jun 2020 02:20:28 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 280
Connection: keep-alive
Location: https://www.zte.com.cn/
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html>
<head><title>307 Temporary Redirect</title></head>
<body bgcolor="white">
<h1>307 Temporary Redirect</h1>
<p>The requested resource resides temporarily under a different URI.</p>
<hr/>Powered by ZTE</body>
</html>

~~~

而参数`-I`只显示`http response`的头部信息，不显示网页代码内容。

~~~shell
[jamza@jamza-vm test]$ curl -I http://www.zte.com.cn/
HTTP/1.1 307 Temporary Redirect
Server: ZTE
Date: Fri, 19 Jun 2020 02:21:42 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 280
Connection: keep-alive
Location: https://www.zte.com.cn/
X-Content-Type-Options: nosniff
X-Xss-Protection: 1; mode=block


~~~



## 显示通信过程

参数`-v`可以显示一次`http`通信的整个过程，包括端口连接与头部信息。

~~~shell
[jamza@jamza-vm test]$ curl -v http://www.zte.com.cn/
* About to connect() to www.zte.com.cn port 80 (#0)
*   Trying 10.30.6.85...
* Connected to www.zte.com.cn (10.30.6.85) port 80 (#0)
> GET / HTTP/1.1
> User-Agent: curl/7.29.0
> Host: www.zte.com.cn
> Accept: */*
>
< HTTP/1.1 307 Temporary Redirect
< Server: ZTE
< Date: Fri, 19 Jun 2020 02:23:48 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 280
< Connection: keep-alive
< Location: https://www.zte.com.cn/
< X-Content-Type-Options: nosniff
< X-Xss-Protection: 1; mode=block
<
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html>
<head><title>307 Temporary Redirect</title></head>
<body bgcolor="white">
<h1>307 Temporary Redirect</h1>
<p>The requested resource resides temporarily under a different URI.</p>
<hr/>Powered by ZTE</body>
</html>
* Connection #0 to host www.zte.com.cn left intact

~~~



## HTTP动词

`curl`的默认`HTTP`动词为`GET`，使用参数`-X`可以支持其他的动词类型。

~~~shell
curl -X POST www.example.com

curl -X DELETE www.example.com
~~~



## 增加头部信息

有时需要在`http request`之中，自行增加一个头信息。`--header`参数就可以起到这个作用。

~~~shell
curl --header "Content-Type:application/json" http://example.com
~~~



## HTTP认证

有些网域需要`HTTP`认证，这时`curl`需要用到`--user`参数。

~~~shell
curl --user name:password example.com
~~~



