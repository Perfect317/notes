# 信息收集

端口扫描

![image-20250224154912134](Kioptrix%20Level2/image-20250224154912134.png)

将端口分离

```
ports=$(grep open kioptix2.nmap | awk -F'/' '{print $1}' | paste -sd',')
```

