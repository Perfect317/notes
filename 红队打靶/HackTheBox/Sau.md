# nmap

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-03-12 16:07 CST
Warning: 10.10.11.224 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.11.224
Host is up (0.30s latency).
Not shown: 65531 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    filtered http
8338/tcp  filtered unknown
55555/tcp open     unknown

```

# web

55555端口存在http服务

为Request Baskets1.2.1服务，有SSRF漏洞（CVE-2023-27163），80端口是无法访问的，可以使用SSRF读取到80端口的内容

80端口为Maltrail (v0.53)服务，存在RCE漏洞，反弹shell之后

sudo -l发现有systemctl 245权限

CVE-2023-26604 systemctl 提权

