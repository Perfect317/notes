# 安装 metacity
```
apt install -y metacity
```


# 执行
```
sudo metacity --replace &
```

可以把命令添加到开机启动项里：

```
vim /etc/rc0.d/rc.local
```

输入以下内容：

```
@reboot /etc/rc0.d/start.sh
```

