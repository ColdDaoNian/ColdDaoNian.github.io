# 点击劫持：X-Frame-Options头丢失漏洞

------

《Web安全测试学习手册》- 点击劫持：X-Frame-Options头丢失漏洞

------

## 0x00 点击劫持：X-Frame-Options头丢失漏洞 - 介绍

### 1）什么是点击劫持：X-Frame-Options头丢失漏洞

X-Frame-Options HTTP 响应头， 可以指示浏览器是否应该加载一个 iframe 中的页面。 网站可以通过设置 X-Frame-Options 阻止站点内的页面被其他页面嵌入从而防止点击劫持。

### 2）点击劫持：X-Frame-Options头丢失漏洞的特点

属于一种具有迷惑性高、利用难度中等、攻击方式单一的攻击手法。

---

## 0x01 点击劫持：X-Frame-Options头丢失漏洞 - 风险等级

中

---

## 0x02 点击劫持：X-Frame-Options头丢失漏洞 - 原理

当X-Frame-Options HTTP 响应头丢失的时候，攻击者可以伪造一个页面，该页面使用前端技术精心构造一些诱惑用户点击的按钮、图片，该元素下方就是一个iframe标签，当用户点击后上层的元素后，就相当于点击了iframe标签引入的网页页面。

---

## 0x03 点击劫持：X-Frame-Options头丢失漏洞 - 常见场景

* 默认配置的WebServer

* 单一交互的网站（按钮）

---

## 0x04 测试方案

使用CURL请求网站，查看响应头是否包含`X-Frame-Options`

```txt
curl -I http://target
```

### 未修复

```txt
payloads@Online$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.12.1 (Ubuntu)
Date: Mon, 16 Apr 2018 07:48:51 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Sun, 08 Apr 2018 02:41:44 GMT
Connection: keep-alive
ETag: "5ac98168-264"
Accept-Ranges: bytes
```

不存在`X-Frame-Options`响应头。

```txt
MannixdeMacBook-Air:~ mannixmannix$ curl -I mannix.top
HTTP/1.1 200 OK
Server: GitHub.com
Content-Type: text/html; charset=utf-8
Last-Modified: Mon, 22 Oct 2018 18:57:10 GMT
ETag: "5bce1d86-6411"
Access-Control-Allow-Origin: *
Expires: Wed, 24 Oct 2018 08:05:53 GMT
Cache-Control: max-age=600
X-GitHub-Request-Id: 4526:49BF:2B30B04:397243B:5BD02589
Content-Length: 25617
Accept-Ranges: bytes
Date: Wed, 24 Oct 2018 15:40:58 GMT
Via: 1.1 varnish
Age: 0
Connection: keep-alive
X-Served-By: cache-hnd18727-HND
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1540395659.802252,VS0,VE93
Vary: Accept-Encoding
X-Fastly-Request-ID: 4a550aebaf182b3570d382ccbed042d2d0dc3630
```

![Snipaste_2018-10-24_23-41-08.png](https://whitecell.io/upload/attach/201810/151_MB6SU28EDCNBNBY.png "Snipaste_2018-10-24_23-41-08.png")

```txt
MannixdeMacBook-Air:~ mannixmannix$ curl -I 10.211.55.3
HTTP/1.1 200 OK
Date: Wed, 24 Oct 2018 15:43:52 GMT
Server: Apache/2.4.23 (Win32) OpenSSL/1.0.2j mod_fcgid/2.3.9
X-Powered-By: PHP/7.2.1
Content-Type: text/html; charset=UTF-8
```

```txt
MannixdeMacBook-Air:~ mannixmannix$ curl -I 10.211.55.3:8080
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Content-Type: text/html;charset=ISO-8859-1
Transfer-Encoding: chunked
Date: Wed, 24 Oct 2018 15:44:30 GMT
```

### 修复后

```txt
payloads@Online$ curl -I http://localhost
HTTP/1.1 200 OK
Server: nginx/1.12.1 (Ubuntu)
Date: Mon, 16 Apr 2018 07:52:08 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Sun, 08 Apr 2018 02:41:44 GMT
Connection: keep-alive
ETag: "5ac98168-264"
X-Frame-Options: SAMEORIGIN
Accept-Ranges: bytes
```

---

## 0x05 修复方案

配置WebServer，更改配置文件，添加自定义响应头

使用 X-Frame-Options 有三个可选的值：

* DENY：浏览器拒绝当前页面加载任何Frame页面

* SAMEORIGIN：frame页面的地址只能为同源域名下的页面

* ALLOW-FROM：origin为允许frame加载的页面地址

若网站内有使用iframe标签链接同源资源的，需要设置为`SAMEORIGIN`。

### Apache

配置 `Apache` 在所有页面上发送 `X-Frame-Options` 响应头，需要把下面这行添加到 `site` 的配置中:

```txt
Header always append X-Frame-Options SAMEORIGIN
```

### Nginx

配置 `nginx` 发送 `X-Frame-Options` 响应头，把下面这行添加到 `http`, `server` 或者 `location` 的配置中:

```txt
add_header X-Frame-Options SAMEORIGIN;
```

### IIS

配置 `IIS` 发送 `X-Frame-Options` 响应头，添加下面的配置到 `Web.config` 文件中:

```txt
<system.webServer>
  ...
 
  <httpProtocol>
    <customHeaders>
      <add name="X-Frame-Options" value="SAMEORIGIN" />
    </customHeaders>
  </httpProtocol>
 
  ...
</system.webServer>
```

### Tomcat

在`conf/web.xml`中添加如下配置：

`C:\Program Files\Apache Software Foundation\Tomcat 7.0\conf\web.xml`

```txt
<filter>
        <filter-name>httpHeaderSecurity</filter-name>
        <filter-class>org.apache.catalina.filters.HttpHeaderSecurityFilter</filter-class>
        <init-param>
            <param-name>antiClickJackingOption</param-name>
            <param-value>SAMEORIGIN</param-value>
        </init-param>
        <async-supported>true</async-supported>
    </filter>
<filter-mapping>
        <filter-name>httpHeaderSecurity</filter-name>
        <url-pattern>/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

------

作者 [@Mannix][1]     
2018 年 10月 25日

参考文档作者 [@Rvn0xsy][2]     
2018 年 04月 16日

[1]: http://mannix.top/
[2]: https://payloads.online/archivers/2018-04-16/3




