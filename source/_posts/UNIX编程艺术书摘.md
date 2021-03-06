---
title: UNIX编程艺术书摘
date: 2020-07-21 19:51:05
tags:
---

- mechanism, not policy    致力于提供一套 机制 而非 策略，行为最终逻辑被推到使用端，因而可以支持眼花缭乱的定制功能，缺点是策略的缺失需要用户自己设置。但优势在于：策略相对短寿 机制才会长存。

- 跨平台可移植的开放 Unix API 硬件无关标准

- 程序接口小巧 简洁 正交

- 自下而上 而非自上而下的设计，注重实效，立足于丰富的经验

- 每个程序只做一件事，如果有新任务就重新开始而不是在原程序增加新功能

- 假定每个程序的输出都会成为另一个程序的输入，输出中不要有无关信息干扰（管道设计）

- 程序要能协作，要能处理文本流（最通用的接口）

- **数据压倒一切**，编程的核心是数据结构而非算法，选择了正确的数据结构一切都井井有条那么正确的算法也就不言自明

  

### 原则-KISS

1. **模块原则：使用简洁的接口拼合简单的部件**--计算机编程的本质是控制复杂度
2. **清晰原则：清晰生于机巧**--为了一丁点性能而降低可读性增加软件复杂性得不偿失
3. **组合原则：设计时考虑拼接组合**--多数程序接收文本流输入处理成文本流输出（简单过滤器模式）
4. **分离原则：策略同机制分离，接口同引擎分离**--策略的变化远远快于机制，eg:前端实现策略后端实现机制的双进程结构
5. **简洁原则：设计要简洁，复杂度能低则低**
6. **吝啬原则：除非却无他法，不要编写庞大的程序**
7. **透明性原则：设计要可见，以便审查和调试**--透明性（一目了然）|显见性（监视工具、调试脚本）
8. **健壮原则：健壮源于透明与简洁**--避免出现特例，简洁+透明=》健壮（程序模块化是方法之一）
9. **表示原则：把知识叠入数据以求逻辑质朴而健壮**--数据比编程逻辑更容易驾驭(eg:数组>switch)，代码复杂度尽力转移到数据上
10. **通俗原则：接口设计避免标新立异**--最少惊奇原则
11. **缄默原则：如果一个程序没什么好说的，就沉默**--仅仅输出重要的信息,用户注意力是有限的
12. **补救原则：出现异常时，马上退出并给出足够多的错误信息**--宽容的收,谨慎的发,尽可能理解不规范输入并要么退出要么给严谨的输出
13. **经济原则：宁花机器一分，不花程序员一秒**--采用高级语言如python\java\shell
14. **生成原则：避免手工hack，尽量编写程序去生成程序**-- makefile|dao层代码生成器
15. **优化原则：雕琢前要先有圆形，跑之前先学会走**
16. **多样性原则：绝不相信所谓”不二法门“的断言**
17. **扩展原则：设计着眼未来，未来总比预想来的快**--充分的自描述性

