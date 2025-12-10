# IP段信息收集

绕过CDN寻找真实IP地址

## 判断是否存在CDN

### 多地ping

如果没有使用CDN的话，只会显示同一个IP地址

在线工具：

[多个地点Ping服务器,网站测速 - 站长工具](https://ping.chinaz.com/)

[网站测速工具_超级ping _多地点ping检测 - 爱站网](https://ping.aizhan.com/)

[网站测速工具_超级ping _多地点ping检测 - 爱站网](https://ping.aizhan.com/)

### nslookup

用国外的dns服务器解析域名，如果也得到了多个IP地址，多半也是使用了CDN

```bash
nslookup -qt=A www.baidu.com 8.8.8.8 
```

像百度就是存在CDN：

![image-20251127172202592](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251127172202708.png)

### IP反查域名

查看这个IP地址是否存在大量不相干的地址

在线工具：

[14.26.160.95属于中国广东_IP反查域名_同IP站点查询_同ip网站查询_爱站网](https://dns.aizhan.com/)

![image-20251127172407801](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251127172407901.png)

### 观察header

观察请求响应返回的数据的头部，有没有CDN服务商表示

如果asp或者asp.net网站的server不是IIS而是Nginx，则多半使用了nginx反向代理到CDN

### 在线工具检测

[CDN Finder - CDN Planet](https://www.cdnplanet.com/tools/cdnfinder/)

[全球 CDN 服务商查询_专业精准的IP库服务商_IPIP](https://tools.ipip.net/cdn.php)

## 寻找真实IP地址

### 网络空间测绘引擎

zoomeye、fofa、hunter、quake等

搜索站点的时候有几率获取目标的真实IP地址

### 二级域名

由于部分CDN的价格昂贵，所以一些重要的站点会是用CDN加速，但是一些子域名没有，而子域名极有可能和主站在同一个C段，可以通过发现目标子域名，从而根据IP地址定位主域名的IP地址

### 邮箱

让目标的右键服务器给我们发送邮件给自己的qq邮箱再查看邮件源码有概率可以找到真实IP地址

> 这里还有一个`奇淫技巧`，通过发送邮件给一个不存在的邮箱地址，比如 [000xxx@domain.com](mailto:000xxx@domain.com) ，因为该用户不存在，所以发送将失败，并且还会收到一个包含发送该电子邮件给你的服务器的真实 IP 通知。

- RSS 订阅
- 邮箱注册、激活处
- 邮箱找回密码处
- 产品更新的邮件推送
- 某业务执行后发送的邮件通知
- 员工邮箱、邮件管理平台等入口处的忘记密码

### 海外ping

由于国内厂家可能只针对国内使用CDN，对海外没有铺设，所有可以用海外的服务器可以获取到真实的IP地址

在线工具：

[多个地点Ping服务器,网站测速 - 站长工具](https://ping.chinaz.com/)

![image-20251127193838494](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251127193838552.png)

http://www.webpagetest.org/

https://dnscheck.pingdom.com/

### DNS历史解析记录

[Bad Request | Netcraft](https://sitereport.netcraft.com/?url=)

[IP History - ViewDNS.info](https://viewdns.info/iphistory/?domain=)

### SSl证书

[Censys Search](https://search.censys.io/)

命令行工具

- openssl

  ```bash
  openssl s_client -connect hao123.com:443 | grep subject
  ```

- curl

  ```bash
  curl -v https://hao123.com | grep 'subject'
  ```

### 敏感文件泄露

- 服务器日志文件
- 探针文件，例如phpinfo
- 网站备份压缩文件
- .DS_Store
- .hg
- .git
- SVN
- Web.xml

### 历史域名

- 很多网站再发展过程中会更换域名，例如京东以前的域名就是360buy.com，后面斥巨资买了jd.com
- 而历史域名很可能没有开启CDN就有可能找到真实IP地址

### 通过XML-RPC PINGBACK通信

- XML-RPC 是支持 WordPress 与其他系统之间通信的规范，它通过使用 HTTP 作为传输机制和 XML 作为编码机制来标准化这些通信过程。
- 在 WordPress 的早期版本中，默认情况下已关闭 XML-RPC，但是从3.5版本开始，默认情况下开启。
- XML-RPC 支持 trackback 和 pingback。
- 虽然 WordPress 启用了 REST API 来代替 XML-RPC ，不过 XML-RPX 不会过时的，放心大胆的使用就好，虽然 XML-RPC 这个技术很老了，但依然可以通杀很多网站。

```bash
POST /xmlrpc.php HTTP/1.1
Host: domain.com
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: en,zh-CN;q=0.9,zh;q=0.8
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 323

<?xml version="1.0" encoding="UTF-8"?>
<methodCall>
<methodName>pingback.ping</methodName>
<params>
<param>
<value><string>http://2vbis4.dnslog.cn</string></value>
</param>
<param>
<value><string>https://domain.com/</string></value>
</param>
</params>
</methodCall>
```

### APP

如果目标存在APP，APP有可能通过IP直接和服务器端进行通信，若最新版采用了域名通信，也可以找一下历史版本分析一下。

> 历史版本宝贝比较多

### 漏洞

如果目标本身存在漏洞，例如SSRF、XXE、XSS、文件上传、命令执行等漏洞，或我们找到的突破点，注入我们自己带外的服务器地址，然后再服务器上检查HTTP日志记录

应用本身的错误会造成传递一些错误的参数值或者错误的地址时，内部会抛出异常，也就是报错，我们可以查看报错点，经常会泄露一些真实IP地址或内网IP段

### 通过virtual-host碰撞

 当我们收集了目标足够多的IP地址的时候可以通过IP和子域名的碰撞，来批量命中目标以及目标的哪些子域

在线收集工具：[Virtual host discovery for any IP address - Pentest-Tools.com](https://pentest-tools.com/information-gathering/find-virtual-hosts)

### 通过favicon.ico哈希特征

favicon就是浏览器title的图标

获取某个网站的favicon之后，计算它的指纹哈希，然后去fofa等搜索引擎搜索和这个相同的主机，从而进一步探测是否能够挖掘目标网站的真实IP地址

![image-20251127200624938](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251127200625010.png)

![image-20251127200631441](https://cdn.jsdelivr.net/gh/wudiloveyh/my-image-bucket@main/images/20251127200631476.png)

### 通过网站特征

查看这套源码相似的其他主机，用fofa等搜索引擎搜索出来的主机可能会带有真实IP地址

### 通过域名备案信息广域探测（成功率高）

由于钱的问题，一般多个域名会公用同一台服务器，如果这个网站存在备案，我们可以收集这个单位或个人备案的其他网站域名和子域名，在探测一遍，很有可能其中某个边缘子域名没有做CDN，暴露了真实IP地址，然后验证一下这个IP地址是不是也是目标网站的真实IP地址

### 找到真实IP地址如何使用

获取到真实IP地址之后直接访问可能无法访问，这里提供两种思路解决

- 修改hosts文件，将域名和IP地址绑定
- 如使用 burpsuite 测试，可以在 Project options ----> Connections ----> Hostname Resolution 中增加域名和 IP 记录。



## 网络空间搜索引擎

利用网络空间搜索引擎搜索目标IP和C段，快速发现目标资产的IP地址段上的端口开放情况（时间充裕还是得手动扫）

尽量尝试证书、图标、备案内容等方式，还有关键词如：body="南方电网"，不一定要全程，简写往往又更好的效果。

由于搜索引擎是从数据库拿的数据，所以有时效性，部分结果不准确



## C段扫描工具

nmap

王牌扫描工具，缺点是速度比较慢

常用探测手段：

```bash
nmap -vvv -Pn -p- ip/24 -n -T4
```

Goby

图像化的资产收集工具

[Goby— 资产绘测及实战化漏洞扫描工具](https://gobysec.net/#dl)

个人感觉不是很好用



水泽：shuize

自带资产探测存活、漏洞验证、host碰撞

项目地址：[0x727/ShuiZe_0x727: 信息收集自动化工具](https://github.com/0x727/ShuiZe_0x727)



fscan

本人最喜欢的工具，速度快，支持漏洞验证

项目地址：[shadow1ng/fscan: 一款内网综合扫描工具，方便一键自动化、全方位漏扫扫描。](https://github.com/shadow1ng/fscan)

```bash
fscan.exe -h ip/24
fscan.exe -h 192.168.1.1 -p 1-65535
```

## 组合工具

快速获取C段信息，验证目标存活，获取title、架构、组件、框架、使用产品等信息，可以帮我们节省大量时间

Ehole：https://github.com/EdgeSecurityTeam/EHole

ALLiN：[P1-Team/AlliN: A flexible scanner](https://github.com/P1-Team/AlliN)

小巧轻便快速全面的扫描工具

浏览器插件：fofa pro view

https://addons.mozilla.org/zh-CN/firefox/addon/fofa-pro-view/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search

## IP反查域名

在线平台

搜索引擎直接搜索IP反查域名就可以了，推荐：

https://securitytrails.com/dns-trails

好像挂了

网络空间搜索引擎：

直接搜索IP地址就可以出现很多域名出来



