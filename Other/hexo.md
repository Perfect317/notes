## 1.博客基础设置

git仓库地：/root/hexo.git

hook函数位置:/root/hexo.git/hook/post-receive

项目地址：/var/www/Hexo

项目部署：更改source/_post中的markdown

然后使用hexo g -d 

## 2.博客插入图片

在source/_post文件夹下使用hexo n "xxxx"(xxxx为要创建的md的名称)

此时会生成一个xxxx的文件夹和xxx.md，将图片放入xxxx文件夹中，在xxxx.md中引用./xxxx/图片.png

<font color=red>切记绝对路径需要加上"./"</font>

