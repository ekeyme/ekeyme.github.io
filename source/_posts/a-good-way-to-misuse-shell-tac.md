title: shell命令 tac 帮助阻塞程序直至EOF
date: 2017-01-13 17:58:01
tags:
---
起源于我要写的一个脚本，[`joinline`](https://gist.github.com/ekeyme/652cf5fe4ce1540d74d9cbab68125600)。`joinline`首先从`stdin（e.g. 终端）`读入多行文本，然后用一个字符串（join string）将每行连接起来，最后打印。考虑到性能问题，`joinline`需要每读1行文本，就要做连接并打印（到`stdout`）。此时有1个问题需要注意，使用`joinline`时，如果输入和输出都是同个终端，且输入未结束又同时做连接打印，便会出现下面的字符串流的“错串问题”。

本来输入是`1\n2\n3\n4\n5\n` + `Ctr-D`（End-of-File, 文件末尾信号），期待结果应该是`1;2;3;4;5`，却
```
$ bad-joinline ";"
1
2
1;3
2;4
3;5
4;5
$
```

因此，我首先想到我需要一个能接受终端输入的同时将程序阻塞直输入结束（`EOF(Ctr+D)`），再将其释放的命令。
最终上[unix.stackexchange](http://unix.stackexchange.com/questions/337055/a-program-that-could-buffer-stdin-or-file/337061#337061)问了问，原来最简单的便是使用管道将2个`tac`，`tac | tac`，串起来。tac一般是用来按行倒着打印文件的，且在接受标准输入的同时会一直阻塞至文件结束才开始打印。第1个`tac`会将文件倒着打印，第2个又将行顺序反了的文件倒正回来。
```
$ tac | tac
line 1
line 2
line 3
^D    # Ctr-D 接受到EOF，后面输出 
line 1
line 2
line 3
```

虽然有点滥用，却胜在简单。在公司的服务器，30+万行的数据输入，处理时间还可以接受。
另外，还有1种更高效的方法便是使用命令`sed ':loop; N; b loop'`，这个命令解释起来不容易，简单来说就是采用`sed`的`N`命令，不停地将***下一行***又***下一行***（看到`loop`没有）的内容append入`sed`的模式空间（`pattern space`）中，直至文件结尾再打印出来（感觉说了1句废话）。两者的性能差别有2倍左右。
```
$ cat * | wc -l
383803
$ time cat * | tac | tac 1>/dev/null

real    0m0.710s
user    0m0.179s
sys     0m0.915s
$ time cat * | sed ':loop; N; b loop' 1>/dev/null

real    0m0.468s
user    0m0.143s
sys     0m0.484s

```
