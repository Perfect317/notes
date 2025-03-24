# 信息枚举

## nmap

```
Not shown: 64871 closed tcp ports (reset), 662 filtered tcp ports (no-response)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 58.88 seconds

```

# 80端口

添加DNS解析记录

```
echo '10.10.11.227 tickets.keeper.htb' >>/etc/hosts
```

访问后是一个best practical solutions的网站，网上寻找源码，找到默认账号密码，尝试登录

![image-20250319233514848](Keeper/image-20250319233514848.png)

```
https://github.com/bestpractical/rt
```

默认账号密码`root:passwd` 

在用户中查看lnorgaard的详细信息时找到

```
New user. Initial password set to Welcome2023!
```

使用ssh连接可以成功连接