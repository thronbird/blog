---
title: Clean Architectur 读书笔记
tags:
---

###WHAT IS DESIGN AND ARCHITECTURE?

The goal of software architecture is to minimize the human resources required to build and maintain the required system.

###POLYMORPHISM？

OO is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system. It allows the architect to create a plugin architecture, in which modules that contain high-level policies are independent of modules that contain low-level details.

###SEGREGATION OF MUTABILITY

One of the most common compromises in regard to immutability is to segregate the application, or the services within the application, into mutable and immutable components.

###DIP: THE DEPENDENCY INVERSION PRINCIPLE

In a statically typed language, like Java, this means that the use, import, and include statements should refer only to source modules containing interfaces, abstract classes, or some other kind of abstract declaration. Nothing concrete should be depended on.

看了youtube上bob大叔在yale的演讲，和想象中的标准闷骚码农不同，bob叔是个很风趣的人，还有点毒舌，哈哈，内容大体就是这本书前几章的内容

（链接：https://www.youtube.com/watch?v=TMuno5RZNeE&t=2880s）

一些观点很有参考价值：

In a statically typed language, like Java, this means that the use, import, and include statements should refer only to source modules containing interfaces, abstract classes, or some other kind of abstract declaration. Nothing concrete should be depended on.

Part of the art of developing a software architecture is carefully separating those policies from one another, and regrouping them based on the ways that they change.Policies that change for the same reasons, and at the same times, are at the same level and belong together in the same component. Policies that change for different reasons, or at different times, are at different levels and should be separated into different components.

 Divide our application into business rules and plugins.

Rigidity:  What is bad code:If you modify something, you break something else; You must modify massive amounts of other code to come back to consistency with this modification.That is rigid code that has dependencies that snake out in so many directions that you cannot make isolated change without change everything around it;

Fragility：Change one thing leads to error in another spot. good code are copuled with bad code and cannot be reused. 

吐槽OO：

OO did not give us encapsulation; OO weaken encapsulation;  OO = Moudling the real world? nonsense, OO is about managing dependencies by selectively inverting certain key dependencies in your architecture so that you can prevent rigility,fragility,nonusablility;



