[toc]

# 通讯

## netstat

- `-t` tcp协议
- `-u` udp协议
- `-l` listen模式
- `-s` 列出网络使用统计信息
- `-t` 列出带有服务器名称和`PID`连接信息
- `-i` 显示具体网口信息
- `-a` 显示所有套接字
- `-n` 不解析名称
- `-o` 显示计时器

查看开放的端口

```
netstat -tuln
```



## nc

```
语　　法：nc [-hlnruz][-g<网关...>][-G<指向器数目>][-i<延迟秒数>][-o<输出文件>][-p<通信端口>]
		  [-s<来源位址>][-v...][-w<超时秒数>][主机名称][通信端口...]
参　　数：
  -g<网关>         设置路由器跃程通信网关，最多可设置8个。
  -G<指向器数目>   设置来源路由指向器，其数值为4的倍数。
  -h               在线帮助。
  -i<延迟秒数>     设置时间间隔，以便传送信息及扫描通信端口。
  -l               监听模式，用于入站连接 (监听本地端口)。
  -n               直接使用IP地址，而不通过域名服务器。
  -o<输出文件>     指定文件名称，把往来传输的数据以16进制字码倾倒成该文件保存。
  -p<通信端口>     设置本地主机使用的通信端口。
  -r               随机指定本地与远端主机的通信端口。
  -s<来源位址>     设置本地主机送出数据包的IP地址。
  -u               使用UDP传输协议。
  -v               显示指令执行过程。
  -w<超时秒数>     设置等待连线的时间。
  -z               使用0输入/输出模式，只在扫描通信端口时使用。
 
└─# type -a nc
nc is /usr/bin/nc
nc is /bin/nc

```

| -l   | 使用监听模式，管控传入的资料               |
| ---- | ------------------------------------------ |
| -p   | 设置本地主机使用的通信端口                 |
| -s   | 设置本地主机送出数据包的IP地址             |
| -u   | 使用UDP传输协议                            |
| -v   | 显示指令执行过程                           |
| -w   | 设置等待连线的时间                         |
| -z   | 使用0输入/输出模式，只在扫描通信端口时使用 |
| -n   | 不使用dns反向查询ip地址域名                |

```
从server1拷贝文件到server2上。
先在server2上，用nc激活监听，server2上运行：
[root@hatest2 tmp]# nc -lp 1234 > install.log
 
server1上运行：
[root@hatest1 ~]# ll install.log
-rw-r--r--   1 root root 39693 12月 20   2007 install.log 
[root@hatest1 ~]# nc -w 1 192.168.228.222 1234 < install.log
 
目的主机监听 
nc -l 监听端口  > 要接收的文件名
nc -l 4444 > cache.tar.gz
源主机发起请求
nc  目的主机ip    目的端口 
nc  192.168.0.85  4444 
netstat  如下
[root@localhost jiangbao]# netstat -tpln
tcp        0      0 0.0.0.0:4444                0.0.0.0:*                   LISTEN      18166/nc  
 
目录传输
从server1拷贝nginx-0.6.34目录内容到server2上。
先在server2上，用nc激活监听，server2上运行：
[root@hatest2 tmp]# nc -l 1234 |tar xzvf -
 
server1上运行：
[root@hatest1 ~]# ll -d nginx-0.6.34
drwxr-xr-x 8 1000 1000 4096 12-23 17:25 nginx-0.6.34
[root@hatest1 ~]# tar czvf - nginx-0.6.34|nc 192.168.228.222 1234
 
 
从192.168.2.33拷贝文件到192.168.2.34
在192.168.2.34上： nc -l 1234 > test.txt
在192.168.2.33上： nc 192.168.2.34 < test.txt
 
 
克隆硬盘或分区
操作与上面的拷贝是雷同的，只需要由dd获得硬盘或分区的数据，然后传输即可。
克隆硬盘或分区的操作，不应在已经mount的的系统上进行。
所以，需要使用安装光盘引导后，进入拯救模式（或使用Knoppix工具光盘）启动系统后，
在server2上进行类似的监听动作：
# nc -l -p 1234 | dd of=/dev/sda
server1上执行传输，即可完成从server1克隆sda硬盘到server2的任务：
# dd if=/dev/sda | nc 192.168.228.222 1234
```





## find

`-name` 最常用的find命令， / 表示的是目录

```
find / -name flag.txt
```

***



`-type` 查找对应格式

- `-d(directory)` 为目录

- `-f(file)` 为文件

> 查找名为config的目录
>
> ```
> find / -type d -name config
> ```
>
> 查找名为passwd的文件
>
> ```
> find / -type f -name passwd
> ```

***



`-perm` 查找对应权限

> **4=r 2=w 1=x -=0**

```
find / -type f -perm 777
//查找可执行文件
find / -perm a=x
//a：表示所有用户（所有者、组用户、其他用户）
//u：表示文件的所有者（user）。
//g：表示文件的所属组（group）。
//o：表示其他用户（others）。

```

***



`-user` 查找用户的所有文件

```
find / -user frank
```

***



`-mtime`查找最近10天内修改的文件

`-atime`查找最近10天内访问的文件

`cmin`查找过去一小时内更改的文件

`amin`查找过去一小时内访问的文件

`size`查找对应大小的文件，此命令还可以与 （+） 和 （-） 符号一起使用，以指定大于或小于给定大小的文件。

```shell
fing / -size +100M
```

查找全局可写文件

> ```shell
> find / -writable -type d 2>/dev/null
> ```
>
> ```shell
> find / -perm -222 -type d 2>/dev/null
> ```
>
> ```shell
> find / -perm -o w -type d 2>/dev/null
> ```

查找开发工具和支持的语言：

> ```
> find / -name perl*
> ```
>
> ```
> find / -name python*
> ```
>
> ```
> find / -name gcc*
> ```

查找特定文件权限：

```
find / -perm -u=s -type f 2>/dev/null
```



## 重定向

|          |        | 文件描述符 |
| -------- | ------ | ---------- |
| 标准输入 | stdin  | 0          |
| 标准输出 | stdout | 1          |
| 标准错误 | stderr | 2          |

****

**管道输出符只会识别标准输出的内容**

****

如果想将输出重定向到某一文件描述符，则需要借助`>&`运算符并跟上文件描述符来完成

```shell
echo "no changes" >&1 | sed "s/no/some/"
some changes
//将no changes当做stdout输出
echo "no changes" >&2 | sed "s/no/some/"
no changes
//将no changes当做stderr输出
```

### 常见用法

将标准错误输出到/dev/null，只显示标准输出

```shell
find / -size +100M -type f 2>/dev/null
```

将错误重定向为标准输出

```shell
cat ttt 2>&1
```

## ps

##### 查看进程树

```
ps axjf
ps aux
```

`aux` 选项将显示所有用户的进程 （a），显示启动进程的用户 （u），并显示未连接到终端的进程 （x）。查看 ps aux 命令输出，我们可以更好地了解系统和潜在漏洞。

## sort

语法

```shell
sort [-bcdfimMnr][-o<输出文件>][-t<分隔字符>][+<起始栏位>-<结束栏位>][--help][--verison][文件][-k field1[,field2]]

```

参数说明：

> - -b 忽略每行前面开始出的空格字符。
> - -c 检查文件是否已经按照顺序排序。
> - -d 排序时，处理英文字母、数字及空格字符外，忽略其他的字符。
> - -f 排序时，将小写字母视为大写字母。
> - -i 排序时，除了040至176之间的ASCII字符外，忽略其他的字符。
> - -m 将几个排序好的文件进行合并。
> - -M 将前面3个字母依照月份的缩写进行排序。
> - -n 依照数值的大小排序。
> - -u 意味着是唯一的(unique)，输出的结果是去完重了的。
> - -o<输出文件> 将排序后的结果存入指定的文件。
> - -r 以相反的顺序来排序。
> - -t<分隔字符> 指定排序时所用的栏位分隔字符。
> - +<起始栏位>-<结束栏位> 以指定的栏位来排序，范围由起始栏位到结束栏位的前一栏位。
> - --help 显示帮助。
> - --version 显示版本信息。
> - [-k field1[,field2]] 按指定的列进行排序。



## ln

 硬链接可认为是一个文件拥有两个文件名;而软链接则是系统新建一个链接文件，此文件指向其所要指的文件。此外，软链接可对文件和文件夹，而硬链接仅针对文件。

```
ln -s abc cde    #建立abc 的软连接 
ln abc cde       #建立abc的硬连接
```

## 用户，用户组权限

### usermod

只有 root 或具有[sudo](https://linuxize.com/post/sudo-command-in-linux/) 访问权限的用户才能调用`usermod`和修改用户帐户。成功后，该命令不会显示任何输出。

最典型的用例`usermod`是将用户添加到组中。

要将现有用户添加到辅助组，请使用`-a -G`组名和用户名后面的选项：

```
usermod -a -G GROUP USER
```

### chown

修改文件和目录的所有者和所属组

当只需要修改所有者时，可使用如下 chown 命令的基本格式：

```
[root@localhost ~]# chown [-R] 所有者 文件或目录
```

-R（注意大写）选项表示连同子目录中的所有文件，都更改所有者。

如果需要同时更改所有者和所属组，chown 命令的基本格式为：

```
[root@localhost ~]# chown [-R] 所有者:所属组 文件或目录
```

## lsof -p

-a 列出打开文件存在的进程

-c<进程名> 列出指定进程所打开的文件

-g 列出GID号进程详情

-d<文件号> 列出占用该文件号的进程

+d<目录> 列出目录下被打开的文件

+D<目录> 递归列出目录下被打开的文件

-n<目录> 列出使用NFS的文件

-i<条件> 列出符合条件的进程。（4、6、协议、:端口、 @ip ）

-p<进程号> 列出指定进程号所打开的文件

-u 列出UID号进程详情

-h 显示帮助信息

-v 显示版本信息





## ltrace

打印进程对系统库的调用信息

# 文本处理

## cut 命令

**使用说明:**

cut 命令从文件的每一行剪切字节、字符和字段并将这些字节、字符和字段写至标准输出。

如果不指定 File 参数，cut 命令将读取标准输入。必须指定 -b、-c 或 -f 标志之一。

**参数:**

- `-b` ：以字节为单位进行分割。这些字节位置将忽略多字节字符边界，除非也指定了 -n 标志。
- `-c`：以字符为单位进行分割。
- `-d`：自定义分隔符，默认为制表符。
- `-f` ：与-d一起使用，指定显示哪个区域。
- `-n` ：取消分割多字节字符。仅和 -b 标志一起使用。如果字符的最后一个字节落在由 -b 标志的 List 参数指示的 范围之内，该字符将被写出；否则，该字符将被排除

例：只输出/etc/passwd 中的用户名

```
cat /etc/passwd | cut -d ":" -f 1
```

## sed

- `-n` 默认情况下，sed会打印所有行到标准输出，如果使用了`-n`选项只有被sed处理的行才会被打印
- `p`打印匹配的行
- `d`删除匹配的行
- `a\text`：在匹配的行后追加text
- `i\text`：在匹配的行前插入text
- `c\text`：用文本text替换匹配的行
- `s\pattern\replacement\flags`：替换文本，pattern是要被替换的字符，replacement是被替换后的字符，flags是修饰符，（g代表全局 ）

## awk

`$0`	: 代表当前行(相当于匹配所有)

`$n`	: 代表第n列

```
案例1:(以:为分隔符)
awk -F: '{print $1}' /etc/passwd 
```

```
案例2:(默认空格为分隔符)
awk '{print $1}' /etc/passwd
```

```
统计每行的总字段
awk '{print NF}' 2.txt
```

## tr

**参数说明：**

- -c, --complement：反选设定字符。也就是符合 SET1 的部份不做处理，不符合的剩余部份才进行转换
- -d, --delete：删除指令字符
- -s, --squeeze-repeats：缩减连续重复的字符成指定的单个字符
- -t, --truncate-set1：削减 SET1 指定范围，使之与 SET2 设定长度相等
- --help：显示程序用法信息
- --version：显示程序本身的版本信息

```
所有小写字母转换为大写字母
cat testfile |tr a-z A-Z 
```

## tee

Linux tee命令用于读取标准输入的数据，并将其内容输出成文件。



## Linux tee命令

Linux tee命令用于读取标准输入的数据，并将其内容输出成文件。

tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件。

## wc（word count）

统计行数

wc -l file.txt

## uniq

uniq删除重复相邻行

-c 打印重复行的次数

## grep

想查看多个，-e "filed" -e "accept"

-i 忽略大小写

-n 输出行号

- `-v`：反向查找，只打印不匹配的行。

- `-r`：递归查找子目录中的文件。
- `-l`：只打印匹配的文件名。
- `-c`：只打印匹配的行数。
- **-a 或 --text** : 不要忽略二进制的数据。
- **-A<显示行数> 或 --after-context=<显示行数>** : 除了显示符合范本样式的那一列之外，并显示该行之后的内容。
- **-b 或 --byte-offset** : 在显示符合样式的那一行之前，标示出该行第一个字符的编号。
- **-B<显示行数> 或 --before-context=<显示行数>** : 除了显示符合样式的那一行之外，并显示该行之前的内容。
- **-c 或 --count** : 计算符合样式的列数。
- **-C<显示行数> 或 --context=<显示行数>或-<显示行数>** : 除了显示符合样式的那一行之外，并显示该行之前后的内容。
- **-d <动作> 或 --directories=<动作>** : 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。
- **-e<范本样式> 或 --regexp=<范本样式>** : 指定字符串做为查找文件内容的样式。
- **-E 或 --extended-regexp** : 将样式为延伸的正则表达式来使用。
- **-f<规则文件> 或 --file=<规则文件>** : 指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。
- **-F 或 --fixed-regexp** : 将样式视为固定字符串的列表。
- **-G 或 --basic-regexp** : 将样式视为普通的表示法来使用。
- **-h 或 --no-filename** : 在显示符合样式的那一行之前，不标示该行所属的文件名称。
- **-H 或 --with-filename** : 在显示符合样式的那一行之前，表示该行所属的文件名称。
- **-i 或 --ignore-case** : 忽略字符大小写的差别。
- **-l 或 --file-with-matches** : 列出文件内容符合指定的样式的文件名称。
- **-L 或 --files-without-match** : 列出文件内容不符合指定的样式的文件名称。
- **-n 或 --line-number** : 在显示符合样式的那一行之前，标示出该行的列数编号。
- **-o 或 --only-matching** : 只显示匹配PATTERN 部分。
- **-q 或 --quiet或--silent** : 不显示任何信息。
- **-r 或 --recursive** : 此参数的效果和指定"-d recurse"参数相同。
- **-s 或 --no-messages** : 不显示错误信息。
- **-v 或 --invert-match** : 显示不包含匹配文本的所有行。
- **-V 或 --version** : 显示版本信息。
- **-w 或 --word-regexp** : 只显示全字符合的列。
- **-x --line-regexp** : 只显示全列符合的列。
- **-y** : 此参数的效果和指定"-i"参数相同。

# 解压

**ZIP格式**

压缩文件：

```
zip compressed.zip file1.txt file2.txt folder/
```

解压文件：

```
unzip compressed.zip -d destination_folder/
```

**TAR格式**

压缩文件（使用GZIP）：

```
tar -cvzf archive.tar.gz file1.txt file2.txt folder/
```

解压文件（使用GZIP）：

```
tar -xvzf archive.tar.gz -C destination_folder/
```

**GZIP格式**

压缩文件：

```
gzip file.txt
```

解压文件：

```
gzip -d file.txt.gz
```

**BZIP2格式**

压缩文件：

```
bzip2 file.txt
```

解压文件：

```
bzip2 -d file.txt.bz2
```

**7z格式**

```
7z x file.7z -o directory
```

# 防火墙

## iptables

### 开放端口

```shell
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```

### 删除端口

```shell
iptables -L -n
或
iptables -L -n --line-number

iptables -D INPUT 1
iptables -D INPUT 2
iptables -D INPUT 3
iptables -D INPUT 4
```

## firewalld

### 开放端口

```shell
1. firewall-cmd --zone=public --add-port=80/tcp --permanent  #放行80端口
2. firewall-cmd --zone=public --add-port=443/tcp --permanent #放行443端口
3. 开放多个端口
 firewall-cmd --zone=public --add-port=80-85/tcp --permanent

 firewall-cmd --reload  #重新载入 返回 success 代表成功
 
4.查看 返回 yes 代表开启成功
 firewall-cmd --zone=public --query-port=80/tcp

```

### 关闭端口

```shell
删除端口 返回 success 代表成功
1 firewall-cmd --zone=public --remove-port=80/tcp --permanent   #关闭80端口

2、重新载入 返回 success 代表成功
 firewall-cmd --reload

```

