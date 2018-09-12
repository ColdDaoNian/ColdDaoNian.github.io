# MiscSecNotes #

今天给大家推荐一个渗透测试学习笔记合集，此系列文章是JnuSimba关于学习 Web/Cloud/Docker 安全、渗透测试、安全建设等时记录的一些笔记，部分原创，部分是对网上文章的理解整理。如果可以找到原始参考链接时则会在文末贴出（如 乌云很多链接已失效，或者记不起当时存档时的链接），或者在文章开头写上 by xx，如有侵权请联系我（dameng34 at 163.com）删除或加上reference，感谢在网上共享知识的师傅们，觉得内容不错的朋友请不要吝啬您的 **star**。 

* [资源来源](https://github.com/JnuSimba)

https://github.com/JnuSimba

* [资源来源](https://github.com/JnuSimba/MiscSecNotes)

https://github.com/JnuSimba/MiscSecNotes

* [下载地址](https://github.com/JnuSimba/MiscSecNotes/archive/master.zip)

https://github.com/JnuSimba/MiscSecNotes/archive/master.zip

### 捐赠链接 ###

如果觉得以下内容对您有一定帮助，不妨小额赞助我，以鼓励我更好地完善内容列表。    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/JnuSimba/MiscSecNotes/raw/master/pictures/weixinzhifu.jpg" width=200>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://github.com/JnuSimba/MiscSecNotes/raw/master/pictures/zhifubao.jpg" width=174>  


## 文章目录 ##

### Web 安全 ###

* Web服务基础
    * [HTTP协议](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/HTTP协议.md)
    * [同源策略](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/同源策略.md)
    * [前端基础](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/前端基础.md)
    * [JS 跨域](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/JS跨域.md)
    * [后端基础](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/后端基础.md)
    * [常见函数](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/常见函数.md)
    * [nginx安全配置](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/nginx安全配置.md)
    * [apache安全配置](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/apache安全配置.md)
    * [htaccess文件利用](https://github.com/JnuSimba/MiscSecNotes/blob/master/Web服务基础/htaccess文件利用.md)

* 跨站脚本
  * [解码顺序](https://github.com/JnuSimba/MiscSecNotes/blob/master/跨站脚本/解码顺序.md)
  * [反射XSS](https://github.com/JnuSimba/MiscSecNotes/blob/master/跨站脚本/反射XSS.md)
  * [DOMXSS](https://github.com/JnuSimba/MiscSecNotes/blob/master/跨站脚本/DOMXSS.md)
  * [存储XSS](https://github.com/JnuSimba/MiscSecNotes/blob/master/跨站脚本/存储XSS.md)

* 跨站请求伪造
  * [CSRF](https://github.com/JnuSimba/MiscSecNotes/blob/master/跨站请求伪造/CSRF.md)

* SQL 注入
  * [MYSQL注入](https://github.com/JnuSimba/MiscSecNotes/blob/master/SQL%20注入/MYSQL注入.md)
  * [sqlmap tips](https://github.com/JnuSimba/MiscSecNotes/blob/master/SQL%20注入/sqlmap%20tips.md)
  * [sqlmap 进阶](https://github.com/JnuSimba/MiscSecNotes/blob/master/SQL%20注入/sqlmap%20进阶.md)

* Flash安全
  * [Flash xss](https://github.com/JnuSimba/MiscSecNotes/blob/master/Flash安全/Flash%20XSS.md)
  * [Flash csrf](https://github.com/JnuSimba/MiscSecNotes/blob/master/Flash安全/Flash%20CSRF.md)

* PHP安全
  * [php filter](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20filter.md)
  * [php open_basedir](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20open_basedir.md)
  * [php 安全编码](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20安全编码.md)
  * [php 弱类型问题](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20弱类型问题.md)
  * [php 代码审计入门](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20代码审计入门.md)
  * [php 高级代码审计](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20高级代码审计.md)
  * [php 框架审计](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20框架审计.md)
  * [php 版本特点](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20版本特点.md)
  * [php 防getshell思路](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20防getshell思路.md)
  * [php 变形shell检测](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20变形shell检测.md)
  * [php rasp实现](https://github.com/JnuSimba/MiscSecNotes/blob/master/PHP安全/php%20rasp%20实现.md)  

* URL跳转
  * [url跳转](https://github.com/JnuSimba/MiscSecNotes/blob/master/URL跳转/url跳转.md)

* XML注入
  * [XXE漏洞](https://github.com/JnuSimba/MiscSecNotes/blob/master/XML注入/XXE漏洞.md)

* 点击劫持
  * [clickjacking](https://github.com/JnuSimba/MiscSecNotes/blob/master/点击劫持/clickjacking.md)

* 服务端请求伪造
  * [SSRF 基础](https://github.com/JnuSimba/MiscSecNotes/blob/master/服务端请求伪造/SSRF%20基础.md)
  * [SSRF 利用](https://github.com/JnuSimba/MiscSecNotes/blob/master/服务端请求伪造/SSRF%20利用.md)

* 逻辑漏洞
  * [业务安全](https://github.com/JnuSimba/MiscSecNotes/blob/master/逻辑漏洞/业务安全.md)
  * [支付安全](https://github.com/JnuSimba/MiscSecNotes/blob/master/逻辑漏洞/支付安全.md)


* 命令执行
  * [命令执行](https://github.com/JnuSimba/MiscSecNotes/blob/master/命令执行/命令执行.md)

* 文件包含
  * [文件包含](https://github.com/JnuSimba/MiscSecNotes/blob/master/文件包含/文件包含.md)

* 文件解析
  * [文件解析](https://github.com/JnuSimba/MiscSecNotes/blob/master/文件解析/文件解析.md)

* 文件上传
  * [文件上传](https://github.com/JnuSimba/MiscSecNotes/blob/master/文件上传/文件上传.md)

* 信息泄露
  * [信息泄露](https://github.com/JnuSimba/MiscSecNotes/blob/master/信息泄露/信息泄露.md)

* Bypass WAF
  * [bypass cdn find ip](https://github.com/JnuSimba/MiscSecNotes/blob/master/Bypass%20WAF/bypass%20cdn%20find%20ip.md)  
  * [bypass sqli](https://github.com/JnuSimba/MiscSecNotes/blob/master/Bypass%20WAF/bypass%20sqli.md)
  * [bypass xss](https://github.com/JnuSimba/MiscSecNotes/blob/master/Bypass%20WAF/XSS_Bypass_Cookbook_ver_3.0.pdf)
  * [bypass waf（四个层次）](https://github.com/JnuSimba/MiscSecNotes/blob/master/Bypass%20WAF/bypass%20waf（四个层次）.md)
  * [bypass waf Cookbook](https://github.com/JnuSimba/MiscSecNotes/blob/master/Bypass%20WAF/bypass%20waf%20Cookbook.md)
  * [waf 之SQL注入防御思路分享](https://github.com/JnuSimba/MiscSecNotes/blob/master/Bypass%20WAF/waf%20之SQL注入防御思路分享.md)

* 工具与思路
  * [漏洞检测思路](https://github.com/JnuSimba/MiscSecNotes/blob/master/工具与思路/漏洞检测思路.md)
  * [漏洞挖掘与工具](https://github.com/JnuSimba/MiscSecNotes/blob/master/工具与思路/漏洞挖掘与工具.md)
  * [子域名爆破](https://github.com/JnuSimba/MiscSecNotes/blob/master/工具与思路/子域名爆破.md)  
  * [暴力破解](https://github.com/JnuSimba/MiscSecNotes/blob/master/工具与思路/暴力破解.md)   

* 协议相关
  * [IPv6协议相关](https://github.com/JnuSimba/MiscSecNotes/blob/master/协议相关/IPv6协议相关.md)   
  * [IPv6协议安全](https://github.com/JnuSimba/MiscSecNotes/blob/master/协议相关/IPv6协议安全.md)  

* 漏洞修复
  * [漏洞修复指南](https://github.com/JnuSimba/MiscSecNotes/blob/master/漏洞修复/漏洞修复指南.md)

### 渗透测试 ###

* Linux渗透
  * [Linux执行命令监控](https://github.com/JnuSimba/MiscSecNotes/blob/master/Linux渗透/Linux执行命令监控.md)  
  * [Linux 入侵检测](https://github.com/JnuSimba/MiscSecNotes/blob/master/Linux渗透/Linux%20入侵检测.md)
  * [Linux 提权](https://github.com/JnuSimba/MiscSecNotes/blob/master/Linux渗透/Linux%20提权.md)
  * [Rootkit 综合教程](https://github.com/JnuSimba/MiscSecNotes/blob/master/Linux渗透/Rootkit%20综合教程.md)

* 端口转发
  * [代理知识](https://github.com/JnuSimba/MiscSecNotes/blob/master/端口转发/代理知识.md)  
  * [渗透测试之代理](https://github.com/JnuSimba/MiscSecNotes/blob/master/端口转发/渗透测试之代理.md)
  * [内网端口转发及穿透](https://github.com/JnuSimba/MiscSecNotes/blob/master/端口转发/内网端口转发及穿透.md)  

* Windows渗透
  * [Windows 入侵检测](https://github.com/JnuSimba/MiscSecNotes/blob/master/Windows渗透/Windows%20入侵检测.md)
  * [Windows 入侵排查](https://github.com/JnuSimba/MiscSecNotes/blob/master/Windows渗透/Windows%20入侵排查.md)
  * [Windows 渗透测试](https://github.com/JnuSimba/MiscSecNotes/blob/master/Windows渗透/Windows%20渗透测试.md)  
  * [Windows 应急响应](https://github.com/JnuSimba/MiscSecNotes/blob/master/Windows渗透/Windows%20应急响应.md)  

### 安全建设 ###

* 安全建设
  * [Google基础设施安全设计概述翻译和导读](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/Google基础设施安全设计概述翻译和导读.md)  
  * [中小企业网络安全建设指引](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/中小企业网络安全建设指引.md)  
  * [大型互联网企业安全实践](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/大型互联网企业安全实践.md)
  * [从Google白皮书看企业安全最佳实践](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/从Google白皮书看企业安全最佳实践.md)
  * [安全产品的自我修养](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/安全产品的自我修养.md)  
  * [产品经理眼中比较理想的WEB扫描器](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/产品经理眼中比较理想的WEB扫描器.md)
  * [产品经理眼中未来两年的完美WAF](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/产品经理眼中未来两年的完美WAF.md) 
  * [如何建立有效的安全策略](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/如何建立有效的安全策略.md)
  * [大型网络入侵体系建设](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/大型网络入侵体系建设.md)
  * [非即时反馈策略和随机噪音在业务安全中的应用](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/非即时反馈策略和随机噪音在业务安全中的应用.md)  
  * [初探 下一代网络隔离和访问限制](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/初探%20下一代网络隔离和访问限制.md)
  * [互联网企业安全之端口扫描监控](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/互联网企业安全之端口扫描监控.md)
  * [谷歌的零信任安全架构实践](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/谷歌的零信任安全架构实践.md)  
  * [互联网企业如何建设数据安全体系](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/互联网企业如何建设数据安全体系.md)  
  * [我理解的安全运营](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/我理解的安全运营.md)  
  * [互联网公司移动安全领域的探索与实践](https://github.com/JnuSimba/MiscSecNotes/blob/master/安全建设/互联网公司移动安全领域的探索与实践.md)  


### docker 安全 ###

* Docker安全
  * [docker安全杂谈](https://github.com/JnuSimba/MiscSecNotes/blob/master/Docker安全/docker安全杂谈.md)  
  * [从自身漏洞和架构缺陷，谈docker安全建设](https://github.com/JnuSimba/MiscSecNotes/blob/master/Docker安全/从自身漏洞和架构缺陷，谈docker安全建设.md) 
  * [关于docker的几点安全解析](https://github.com/JnuSimba/MiscSecNotes/blob/master/Docker安全/关于docker的几点安全解析.md)
  * [绝不避谈docker安全](https://github.com/JnuSimba/MiscSecNotes/blob/master/Docker安全/绝不避谈docker安全.md) 
  * [浅谈docker安全合规建设](https://github.com/JnuSimba/MiscSecNotes/blob/master/Docker安全/浅谈docker安全合规建设.md)
  * [如何打造安全的容器云平台](https://github.com/JnuSimba/MiscSecNotes/blob/master/Docker安全/如何打造安全的容器云平台.md)  

### 云计算安全 ###

* 云计算安全
  * [经典网络与VPC](https://github.com/JnuSimba/MiscSecNotes/blob/master/云计算安全/经典网络与VPC.md)
  * [浅析云计算环境下的安全风险](https://github.com/JnuSimba/MiscSecNotes/blob/master/云计算安全/浅析云计算环境下的安全风险.md)
  * [云安全审计（评估）](https://github.com/JnuSimba/MiscSecNotes/blob/master/云计算安全/云安全审计（评估）.md)
  * [从云消费者的角度谈云安全架构](https://github.com/JnuSimba/MiscSecNotes/blob/master/云计算安全/从云消费者的角度谈云安全架构.md)  