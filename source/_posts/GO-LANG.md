---
title: GO_LANG
date: 2020-06-28 17:51:45
tags:
---

## 语法

- map 类型需要make才能用是因为map是引用类型，默认的零值是nil 
- 引用类型（chan、map、string、slice）方法为值类型接收即可（值本身即是引用），结构类型一般方法用指针类型接收（一般需要修改原值，Time这种创建之后不变的类型例外，判断是用值还是指针方法依据是该类的本质）





## 命名规范

- By convention, one-method interfaces are named by the method name plus an -er suffix or similar modification to construct an agent noun: `Reader`, `Writer`, `Formatter`, `CloseNotifier` etc.


