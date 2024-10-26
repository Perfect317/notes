# 信息收集

## 1.子域名收集

### 1.oneforall

```
Python ./oneforall.py --targets baibu.com run
```

### 2.ksubdomain、httpx、subfinder

配合使用可以判断域名是否存活

```
./subfinder -d dlou.edu.cn -silent|./ksubdomain -verify -silent| ./httpx -title -content-length -status-code
```

## 2.查IP，查网段

whois查网段，可以防止打偏

![image-20241002163637428](SRC%E6%BC%8F%E6%B4%9E%E6%8C%96%E6%8E%98/image-20241002163637428.png)

## 3.主动信息收集

### 1.goole

```
inurl:login|admin|manage|member|admin_login|login_admin|system|login|user|main|cms
查找文本内容：
site:域名 intext:管理|后台|登陆|用户名|密码|验证码|系统|帐号|admin|login|sys|managetem|password|username

查找可注入点：site:域名 inurl:aspx|jsp|php|asp

查找上传漏洞：site:域名 inurl:file|load|editor|Files
找eweb编辑器：
site:域名 inurl:ewebeditor|editor|uploadfile|eweb|edit
存在的数据库：site:域名 filetype:mdb|asp|#
查看脚本类型：site:域名 filetype:asp/aspx/php/jsp
迂回策略入侵：inurl:cms/data/templates/images/index/
```

### 2.fofa

org查找

icon_hash查找



### 3.微步在线

可以通过ip反查域名

### 4.360 quake

## 4.JS接口

可能存在未授权js接口

findsomething可以找到泄露的敏感信息