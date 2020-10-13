---
title: Zotero+iCloud多端文献同步管理及阅读
date: 2020-10-13 15:18:57 +0800
categories: [学习总结, 应用技能]
tags: [tips]
---
趁教育优惠送耳机一波终于配齐全家桶，而且渐渐文献越来越多，合理管理文献迫在眉睫。终于找到了目前最优的解决方案，Zotero+iCloud。  
## Zotero
Zotero是一个免费、易用的个人研究助手，可以帮助你收集、组织、引用和分享文献。与此同时，Zotero是一个开源软件，很多人为Zotero开发了各种各样的插件，使得Zotero功能丰富，探索潜力无穷。此处不再介绍具体的使用方法。  

## Zotfile  
美中不足的是，Zotero在存储文献时，会以数字+字母的乱码形式给文件命名，我们在其他没有Zotero软件的平台（如：iPad），难以查阅文献，因此我们需要插件的帮助：  
https://github.com/jlegewie/zotfile/releases  
下载.xpi文件，打开Zotero，选择工具-插件，将文件拖入进行安装即可。安装之后需要对其进行配置。首先打开Zotero的偏好设置，前往 高级->文件和文件夹，查看数据存储位置，默认是~/Username/Zotero这是存放文献条目数据的文件夹目录。我为了将其同步，选择的是iCloud的同步文件夹。  

然后点击Zotero上方工具->Zotfile Preferences，打开Zotfile的偏好设置。将  Source Folder for Attaching New Files 的路径设置为 Zotero 数据文件夹下的storage，这是为了让Zotfile监控Zotero默认的附件存放目录，从而对附件进行重命名、移动等操作。 

![avatar](https://i.postimg.cc/J7pTt3c9/612-E40-A8-7-FF7-4534-94-F7-F2-D277-B4-B47-E.jpg)


当我们右键一个条目，会出现rename的选项，进行rename之后，就会按照配置的格式生成附件。之后我们就可以在各个设备中通过iCloud文件夹访问文献。  


## 参考
- https://www.mdeditor.tw/pl/pJl0