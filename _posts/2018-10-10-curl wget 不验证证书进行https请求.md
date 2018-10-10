# curl wget 不验证证书进行https请求 #  

## curl https://pl.1mdata.com/#/loginByPwd -k ##  

```html
C:\Users\Mannix\Desktop
> curl https://pl.1mdata.com/#/loginByPwd -k
<!DOCTYPE html>
<html ng-app="app">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="icon" type="image/png" href="/favicon.ico" />
    <title>鍖婚福鏁版嵁骞冲彴</title>
    <link rel="stylesheet" href="styles/vendor-55195b673d.css">
    <link rel="stylesheet" href="styles/app-04de5fbf68.css">
```

## curl -I https://pl.1mdata.com/#/loginByPwd -k ##  

```html
C:\Users\Mannix\Desktop                         
> curl -I https://pl.1mdata.com/#/loginByPwd -k 
HTTP/1.1 200 OK                                 
Server: nginx                                   
Date: Wed, 10 Oct 2018 08:49:03 GMT             
Content-Type: text/html                         
Content-Length: 2989                            
Connection: keep-alive                          
Last-Modified: Mon, 03 Sep 2018 02:07:34 GMT    
ETag: "5b8c9766-bad"                            
Accept-Ranges: bytes                            
Cache-Control: no-store                         
```

## wget https://pl.1mdata.com/#/loginByPwd --no-check-certificate ##  

```html
C:\Users\Mannix\Desktop
> wget https://pl.1mdata.com/#/loginByPwd --no-check-certificate
--2018-10-10 16:50:12--  https://pl.1mdata.com/
Resolving pl.1mdata.com (pl.1mdata.com)... 114.118.7.117
Connecting to pl.1mdata.com (pl.1mdata.com)|114.118.7.117|:443... connected.
WARNING: cannot verify pl.1mdata.com's certificate, issued by 'CN=Go Daddy Secure Certificate Authority - G2,OU=http://certs.godaddy.com/repository/,O=GoDaddy.com\\, Inc.,L=Scottsdale,ST=Arizona,C=US':
  Unable to locally verify the issuer's authority.
HTTP request sent, awaiting response... 200 OK
Length: 2989 (2.9K) [text/html]
Saving to: 'index.html.2'

index.html.2                              100%[=======================================================================================>]   2.92K  --.-KB/s   in 0s

2018-10-10 16:50:13 (11.5 MB/s) - 'index.html.2' saved [2989/2989]
```

## wget --debug https://pl.1mdata.com/#/loginByPwd --no-check-certificate ##  

```html
C:\Users\Mannix\Desktop
> wget --debug https://pl.1mdata.com/#/loginByPwd --no-check-certificate
Setting --check-certificate (checkcertificate) to 0
DEBUG output created by Wget 1.16.3 on mingw32.

URI encoding = 'ASCII'
converted 'https://pl.1mdata.com/#/loginByPwd' (ASCII) -> 'https://pl.1mdata.com/#/loginByPwd' (UTF-8)
--2018-10-10 16:57:29--  https://pl.1mdata.com/
Resolving pl.1mdata.com (pl.1mdata.com)... seconds 0.00, 114.118.7.117
Caching pl.1mdata.com => 114.118.7.117
Connecting to pl.1mdata.com (pl.1mdata.com)|114.118.7.117|:443... seconds 0.00, connected.
Created socket 3.
Releasing 0x0000000003484e10 (new refcount 1).
Initiating SSL handshake.
seconds 900.00, Winsock error: 0
Handshake successful; connected socket 3 to SSL handle 0x00000000029bce20
certificate:
  subject: CN=*.1mdata.com,O=Beijing 1M Data Tech Co.\\, Ltd.,L=Beijing,ST=Beijing,C=CN
  issuer:  CN=Go Daddy Secure Certificate Authority - G2,OU=http://certs.godaddy.com/repository/,O=GoDaddy.com\\, Inc.,L=Scottsdale,ST=Arizona,C=US
WARNING: cannot verify pl.1mdata.com's certificate, issued by 'CN=Go Daddy Secure Certificate Authority - G2,OU=http://certs.godaddy.com/repository/,O=GoDaddy.com\\, Inc.,L=Scottsdale,ST=Arizona,C=US':
  Unable to locally verify the issuer's authority.

---request begin---
GET / HTTP/1.1
User-Agent: Wget/1.16.3 (mingw32)
Accept: */*
Accept-Encoding: identity
Host: pl.1mdata.com
Connection: Keep-Alive

---request end---
HTTP request sent, awaiting response... seconds 900.00, Winsock error: 0

---response begin---
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 10 Oct 2018 08:58:08 GMT
Content-Type: text/html
Content-Length: 2989
Connection: keep-alive
Last-Modified: Mon, 03 Sep 2018 02:07:34 GMT
ETag: "5b8c9766-bad"
Accept-Ranges: bytes
Cache-Control: no-store

---response end---
200 OK
Registered socket 3 for persistent reuse.
Length: 2989 (2.9K) [text/html]
Saving to: 'index.html.5'

index.html.5                                0%[                                                                                        ]       0  --.-KB/s             seconds 900.00, Winsock error: 0
index.html.5                              100%[=======================================================================================>]   2.92K  --.-KB/s   in 0.001s

2018-10-10 16:57:29 (3.31 MB/s) - 'index.html.5' saved [2989/2989]
```

## wget --save-headers https://pl.1mdata.com/#/loginByPwd --no-check-certificate ##  

```html
C:\Users\Mannix\Desktop
> wget --save-headers https://pl.1mdata.com/#/loginByPwd --no-check-certificate
--2018-10-10 16:58:17--  https://pl.1mdata.com/
Resolving pl.1mdata.com (pl.1mdata.com)... 114.118.7.117
Connecting to pl.1mdata.com (pl.1mdata.com)|114.118.7.117|:443... connected.
WARNING: cannot verify pl.1mdata.com's certificate, issued by 'CN=Go Daddy Secure Certificate Authority - G2,OU=http://certs.godaddy.com/repository/,O=GoDaddy.com\\, Inc.,L=Scottsdale,ST=Arizona,C=US':
  Unable to locally verify the issuer's authority.
HTTP request sent, awaiting response... 200 OK
Length: 2989 (2.9K) [text/html]
Saving to: 'index.html.6'

index.html.6                              100%[=======================================================================================>]   2.92K  --.-KB/s   in 0s

2018-10-10 16:58:17 (15.2 MB/s) - 'index.html.6' saved [2989/2989]
```