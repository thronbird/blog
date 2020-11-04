---
title: Lang_C
date: 2020-05-03 15:14:27
tags:
---

#####	基础语法

> 定义常量避免魔数
> ```c
> #define LOWER  0
> ```

> **unsigned and signed:** unsigned的作用就是将数字类型无符号化， 例如 int 型的范围：-2^31 ~ 2^31 - 1，而unsigned int的范围：0 ~ 2^32

> 函数定义如果省略了返回值类型，默认为int类型

>  多文件编译命令：
>
> the command ``` cc main.c getline.c strindex.c```
> compiles the three files, placing the resulting object code in files main.o, getline.o, and strindex.o, then loads them all into an executable file called a.out. If there is an error, say in main.c, the file can be recompiled by itself and the result loaded with the previous object files, with the command
>  ``` cc main.c getline.o strindex.o```
> The cc command uses the  .c versus .o naming convention to distinguish source files from object files.