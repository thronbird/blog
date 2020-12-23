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





## FAQ

- bee工具下载： GO111MODULE=on go get -u github.com/beego/bee@v1.10.0
- bee api根据注解生成commentsRouter不生效：把新增的接口定义去掉，重新生成一遍，然后加上再重新生成
- 版本不一致：多个gopath最好去掉，保持一个gopath，方便管理


