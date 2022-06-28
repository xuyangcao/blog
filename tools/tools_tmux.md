---
layout: post 
---

## 前言

远程使用Linux服务器时，若网络不稳定获本地关机等原因，ssh一旦断开则运行的程序就会被kill掉。在图形界面的解决方案中，vnc server将本地和远程解耦开，解决了上述问题。但是很多服务器一般不配置图形界面，只能通过ssh进行远程访问。

本文介绍了使用ssh连接服务器时意外中断的解决方案，笔者对比了screen和tmux两种工具，发现tmux更强大一些。相比于screen，tmux工具不仅可以将当前窗口会话与正在执行的任务分离（使任务在后台运行），而且具有更丰富的切屏和图形界面。因此未来考虑用tmux工具。

## Tmux效果图

Tmux的核心思想是一个会话框上可以创建多个终端，因此更加简洁。以下是tmux的样例截图：


<div class="fig figcenter">
    <img src="https://img-blog.csdnimg.cn/7747b915292d46b881a8f67aa21e76ae.png" width=700px>
    <div class="figcaption"> Tmux 截图</div>
</div>

## 常用功能列表

常用的快捷键组合其实并不多，以下列出了笔者个人常用的快捷键，未来根据需要可能会适当增加。

> 说明：Tmux为组合命令，除黑体外的命令，以下所有命令均需要首先按Ctrl-b。

### 系统操作

   |命令|功能|
   |:-:|:-:|
   |**tmux**|**打开一个tmux会话，并会自动创建一个窗口（window）和面板（pane）**|
   |d|（detach）分离当前客户端（client）|
   |**tmux attach-session**|**恢复tmux会话**| 
   |**tmux attach**|**恢复tmux会话（实践发现和上一条命令效果相同）**|


### 窗口操作

   |命令|功能|
   |:-:|:-:|
   |c|（create）创建一个新的窗口|
   |n|（next）进入下一个窗口|
   |p|（previous）进入上一个窗口|
   |0-9|进入对应编号的窗口|
   |&|关闭当前窗口|

### 面板操作

   |命令|功能|
   |:-:|:-:|
   |%|将当前面板进行纵向分屏|
   |"|将当前面板进行水平分屏|
   |方向键|进入不同面板|
   |Ctrl+方向键|调整当前面板大小|
   |x|关闭当前面板|

### 其他操作

   |命令|功能|
   |:-:|:-:|
   |[|按PgUp PgDn或使用鼠标进行翻页，按q退出|
   |t|显示系统时间|

## 参考文献

[1] [用screen 在后台运行程序](https://blog.csdn.net/hejunqing14/article/details/50338161)

[2] [http://man.openbsd.org/OpenBSD-current/man1/tmux.1](http://man.openbsd.org/OpenBSD-current/man1/tmux.1)

[3] [Linux终端复用神器-Tmux使用梳理](https://www.cnblogs.com/kevingrace/p/6496899.html)

[4] [比Screen更好用的神器：tmux](https://www.linuxprobe.com/better-screen-tmux.html)
