---
title: Mac Catalina + Anaconda
date: 2020-03-11 15:00:27 +0800
categories: [踩坑总结, 环境配置]
tags: [tips]
---
## 常用命令
```python
# 查看conda版本
$ conda --version

# 更新conda版本
$ conda update conda#

# 查看都安装了那些依赖库
$ conda list

# 创建新的python环境并且还指定python版本

$ conda create -n myenv python=3.7

# 创建新环境并指定包含的库
$ conda create -n myenv scipy

# 并且还可以指定库的版本

$ conda create -n myenv scipy=0.15.0

# 复制环境
$ conda create --name myclone --clone myenv

# 查看所有环境

$ conda info --envs

# 激活、进入某个环境
$ source activate myenv

# 退出环境
$ conda deactivate

# 删除环境
$ conda remove --name myenv --all

# 查看当前的环境列表
$ conda info --envs  
$ conda env list

# 查看某个环境下安装的库
$ conda list -n myenv

# 查找包
$ conda search XXX

# 安装包
$ conda install XXX

# 更新包
$ conda update XXX

# 删除包
$ conda remove XXX

# 安装到指定环境
$ conda install -n myenv XXX
```

## 参考
1. https://zhuanlan.zhihu.com/p/51137150