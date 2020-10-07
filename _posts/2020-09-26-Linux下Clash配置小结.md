---
title: Linux下Clash配置小结
date: 2020-09-26 15:19:44 +0800
categories: [踩坑总结, 环境配置]
tags: [tips]
---
Linu下Clash的配置文件编写相当不友好，但是学院服务器boot已满，apt乱七八糟也难以修复，没法下载ss，只好用clash配置代理。配置文件的编写对缩进要求很高。配置文件放在github私密仓库里。

## Proxychains的配置
用有root权限的账户操作，把最后一行填clash对应的socks5端口或https端口即可。
```bash
sudo vim /etc/proxychains.conf
```

```bash
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks5  127.0.0.1 8889
```