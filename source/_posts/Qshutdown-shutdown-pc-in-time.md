title: 'Qshutdown 快速关机bat脚本'
date: 2015-12-26 16:51:45
tags:
---
这个脚本写成的初衷就是为了解决下载过程中，突然有事离开，又需要等待下载完后，关闭电脑的情形；或者你将要睡觉却有些数据正在上传到云端，时间大致还需要10分钟，你自然会想让电脑在10~20分钟后会自动关机。

- 首先右键另存为从[这里](https://raw.githubusercontent.com/ekeyme/simple-script-kit-4X/master/windows/Qshutdown.bat)取得 *Qshutdown.bat* 脚本，保存为 *Qshutdown.bat* 同名文件（你也可以使用自己喜欢的名字保存，但务必选择文件类型为 *所有文件* ，且扩展名为 *.bat* 的可执行脚本）。
-  *Qshutdown.bat* 脚本预设了3个参数
 - *00*：2个0，立刻关机
 - *c*：cancel，取消原先的定时关机计划
 - *任何大于0的数字*：？分钟后关机，如输入 *10* ，就是10分钟后关机

- 如果你想20分钟后关机，双击运行 *Qshutdown.bat* ，然后输入 *20* 即可开启一个20分钟后关机的计划，非常方便。
- 然后如果你想取消原定计划，那就运行 *Qshutdown.bat* 后，输入 *c* 即可。
- 此外你还可以按自己的实际情况设定*shutdown.exe*的实际路径与惯用参数，具体位置如下
``` bat
:: Shutdown program, default: C:WindowsSystem32shutdown.exe
set shutdown=C:WindowsSystem32shutdown.exe
:: para to cancel shutdown plan
set cancel_para=c
:: para to shutdown PC now
set shutdown_now_para=00
```