## 字节码指令

## ![1563618782981](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563618782981.png)

大部分操作指令都有对应不同类型的不同指令，比如iload istore ireturn表示加载（整形变量进操作数栈）、定义一个整形局部变量、返回一个int类型值

return 1+1==》iconst 2；ireturn 

编译期优化：直接将1+1定义为常量2 运行时就不用再做计算了

![1563619341039](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563619341039.png)

![1563619632380](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563619632380.png)

这段代码操作数栈的最大深度是 **2**

iadd：操作数栈顶两个元素出栈（pop）相加之后将结果放回栈顶

1. begin  
2. iload_0    // push the int in local variable 0 onto the stack  
3. iload_1    // push the int in local variable 1 onto the stack  
4. iadd       // pop two ints, add them, push result  
5. istore_2   // pop int, store into local variable 2  
6. end  

![img](https://img-blog.csdn.net/20170605145851355?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYWlyam9yZG9u/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

<https://blog.csdn.net/airjordon/article/details/72867397>

局部变量槽的复用：

![1563623011382](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563623011382.png)

gc的时候buff仍然在局部变量表中因此不会回收!

![1563623154515](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563623154515.png)

此时虽然buff已经超出了作用域，但是手动GC仍然不会回收，因为slot槽复用。![1563623227927](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563623227927.png)

这个时候GC生效了，因为定义a会对局部变量表进行读写。

![1563623306612](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563623306612.png)

<https://blog.csdn.net/android_jiangjun/article/details/78436719>

## 方法调用

### 解析调用：

![1563624168951](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563624168951.png)

### 分派调用：

静态分派：方法重载

动态分派：方法重写

![1563624556799](C:\Users\79343\AppData\Roaming\Typora\typora-user-images\1563624556799.png)