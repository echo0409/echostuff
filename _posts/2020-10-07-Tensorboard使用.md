---
title: 远程服务器Tensorboard使用总结
date: 2020-10-07 15:18:57 +0800
categories: [学习总结, 应用技能]
tags: [tips]
---
## ssh登录
```bash
ssh -L 16006:127.0.0.1:6006 server_name@server.address
```
tensorboard默认端口为1006，在登录时将服务器的6006端口映射到本地的1006端口。

## 在代码中加入tensorboard
```python
from torch.utils.tensorboard import SummaryWriter 

def main():
    count=0
    for weight in [0.1,1,5,10]:
        count=1
        writer = SummaryWriter('log/para'+str(count))
        ## 将每一轮的结果存放在para_i文件夹下，他们在共同的父目录log下
        for e in range(args.epoch):
            print('Epoch is %03d' % e)
            train_loss,cca_r = model_train(weight,audio_net_cuda, visual_net_cuda, train_dataloader, optimizer, kld_loss, cca_loss, 'both')
            PATH = 'ckpt/para'+str(count)+'_%03d.pth' % e
            torch.save(audio_net_cuda.state_dict(), PATH)

            writer.add_scalar('cca',cca_r,e)
            writer.add_scalar('loss',train_loss,e)
```


### 可视化
在log文件夹的父目录执行：
```bash
tensorboard --logdir log
```
log文件夹下就会有多个文件夹，但是对于图片的命名是一样的，tensorboardx会自动识别到同一张图下。可以对比不同参数的执行效果。



***
参考：
1. https://www.pythonf.cn/read/103082
2. https://www.cnblogs.com/gdut-gordon/p/9843744.html