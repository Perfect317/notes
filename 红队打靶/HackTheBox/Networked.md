## nmap

![image-20250326193206383](Networked/image-20250326193206383.png)

## 80端口

网站中间件为apache

![image-20250326205630726](Networked/image-20250326205630726.png)

扫目录，在backup有网站源码备份，upload.php是文件上传,photo.php显示了上传之后的文件

![image-20250326193226693](Networked/image-20250326193226693.png)

源码中lib.php中是一些检查函数，photo.php和upload.php引用了这些检查函数

![image-20250326210829774](Networked/image-20250326210829774.png)

但如果上传的文件为image.php.png，分离时只会把image分离出来，成功上传后的文件名还是image.php.png，如果apache存在解析漏洞，就会将文件解析为php

并且上传时会识别`mime_content_type`，必须为image/开头![image-20250326211930947](Networked/image-20250326211930947.png)

写一句话木马时可以加上GIF文件的16进制文件头即可绕过对`mime_content_type`的检测

![image-20250326212023283](Networked/image-20250326212023283.png)

然后将文件上传，访问之后即可命令执行

![image-20250326212239962](Networked/image-20250326212239962.png)

![image-20250326212316414](Networked/image-20250326212316414.png)

反弹shell到攻击机

![image-20250326213138788](Networked/image-20250326213138788.png)