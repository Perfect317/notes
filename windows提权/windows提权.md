# 信息枚举

## 用户

查看当前用户

```
whoami
```

查看所有用户

```
net user
```

查看用户信息

```
net user xxxx
```

查看主机名

```
hostname
```

## 操作系统

```
systeminfo
```

## 查看进程

```
tasklist -V
```

计划任务

```
schtasks /Query /？
```



## 网络信息

```
ipconfig
```

#### 查看路由表

```
route print
```

## 连接信息

```
netstat -ano
```

## 防火墙信息

```
netsh advfirewall +参数
```

> 此上下文中的命令:                                                     
>  ?        - 显示命令列表。                                               
>  consec     - 更改到 `netsh advfirewall consec' 上下文。                                 
>  dump      - 显示一个配置脚本。                                              
>  export     - 将当前策略导出到文件。                                             
>  firewall    - 更改到 `netsh advfirewall firewall' 上下文。                                
>  help      - 显示命令列表。                                               
>  import     - 将策略文件导入当前策略存储。                                           
>  mainmode    - 更改到 `netsh advfirewall mainmode' 上下文。                                
>  monitor     - 更改到 `netsh advfirewall monitor' 上下文。                                
>  reset      - 将策略重置为默认全新策略。                                            
>  set       - 设置每个配置文件或全局设置。                                           
>  show      - 显示配置文件或全局属性。

> ```
> netsh advfirewall show +参数
> ```

> 此上下文中的命令:                                                       
>  show allprofiles - 显示所有配置文件的属性。                                           
>  show currentprofile - 显示活动配置文件的属性。                                          
>  show domainprofile - 显示域配置文件的属性。                                           
>  show global   - 显示全局属性。                                               
>  show privateprofile - 显示专用配置文件的属性。                                          
>  show publicprofile - 显示公用配置文件的属性。                                          
>  show store   - 显示当前交互式会话的策略存储。

**常用命令**

查看防火墙状态

```
netsh advfirewall show currentprofile
```

查看防火墙规则

```
netsh advfirewall firewall show rule name=all
```

## 已安装的应用程序

```
wmic /?
```

```
wmic product get name,version,vendor 
```

## **常规信息搜集**

```
systeminfo 查询系统信息

hostname 主机名

net user 查看用户信息

netstat -ano|find "3389" 查看服务pid号

wmic os get caption 查看系统名

wmic qfe get Description,HotFixID,InstalledOn 查看补丁信息

wmic product get name,version 查看当前安装程序

wmic service list brief 查询本机服务

wmic process list brief 查询本机进程

net share 查看本机共享列表

netsh firewall show config 查看防火墙配置
```

## 常见的杀软如下：

```


360sd.exe 360杀毒

360tray.exe 360实时保护

ZhuDongFangYu.exe 360主动防御

KSafeTray.exe 金山卫士

SafeDogUpdateCenter.exe 安全狗

McAfee McShield.exe McAfee

egui.exe NOD32

AVP.exe 卡巴斯基

avguard.exe 小红伞

bdagent.exe BitDefender

```

## **要搜集的信息大致如下几点：**

机器的系统及其版本

机器的打补丁情况

机器安装的服务

机器的防火墙策略配置

机器的防护软件情况