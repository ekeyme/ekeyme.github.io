title: shell命令 tac 的妙用
date: 2017-01-13 17:58:01
tags:
---
起源于我在公司要写一个[`joinline`](https://gist.github.com/ekeyme/652cf5fe4ce1540d74d9cbab68125600)的脚本，帮助我快速地join起`stdin`传入的行内容用于快速生成SQL语句或者其他用途等等。e.g. `mysql -uroot -p -e "SELECT name FROM user WHERE id in $(joinline)"`。

```
tac | tac

```