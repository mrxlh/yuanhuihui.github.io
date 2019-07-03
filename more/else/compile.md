---
layout: post
title:  "编译优化(一)"
date:   2016-03-01 19:09:12
categories: android
excerpt:  编译优化(一)
---

* content
{:toc}

---

## 一、概述

Java代码的生命周期：*.java  ->  *.class ->  机器码


- 前端编译器： *.java  ->  *.class
- JIT编译器： *.class ->  机器码
- AOT编译器： *.java  ->  机器码

本文先讲讲前端编译，早期编译期更多妥帖。

## 早期编译

早期编译：javac编译器，本身就是由Java语言编写的。

编译过程：

1. 解析与填充符号表： 
2. 注解(annotation)处理
3. 分析与生成字节码

画图： Java-> 上述3步骤-> ByteCode

### 1. 解析与填充符号表

该阶段主要包括词法分析、语法分析，填充到符号表。

词法分析：将Java源码划分成Token序列，何为Token呢，Token是编译过程的最小单位，不可拆分，例如关键字、变量名、运算符、常量都认为是一个Token。例如代码 `int age=18`，转换为Token集合则为`int、age、=、18`共4个Token。语法分析：根据Token序列构造抽象语法树，该树的每个节点代表一种语法结构，例如接口、返回值、修饰符等。填充符号表：该符号表中记录着符号地址和符号信息。

### 2. 注解处理

注解处理阶段，可以对抽象语法树的任意元素进行读取、修改、增删操作，当对抽象语法树进行修改时，编译器回到阶段1，重新处理直到所有的注解处理完成。

### 3. 分析与生成字节码

语义分析：对抽象语法树进行检查，包含标注检查，数据及控制类流分析；

- 标注检查：**变量使用前是否已声明，赋值操作两边类型是否匹配**。 
**常量折叠，**例如 int a = 2 +1; 编译后会变成 int a = 3; 这样不会增加程序运行的任何CPU指令的负载。

- 数据及控制类流分析：进一步检查，比如**局部变量使用前是否赋值，方法是否有返回值，异常是否有处理**。 编译期的数据及控制类流分析与类加载时运行期的数据及控制类流分析类似，检查项类似。
- **解语法糖：泛型、变长参数、自动装箱/拆箱**。这些都是虚拟机所不支持，经过编译后还原成基本语法。
- 字节码生成：将抽象语法树转换成字节码，以及收集整理实例构造器和类构造器，实例构造器包含构造器、{}、实例变量赋值，类构造器包含static{}、类变量赋值。

## Java语法糖

泛型擦除，Java泛型只存在Java源码，编译成字节码文件时会将泛型替换为原生类型（Raw Type），并相应地方插入强制转换代码。






## 晚期编译

1. 分支预测
2. 裁剪不可达分支
3. 栈上分配
4. 同步消除
5. 标量替换
6. final类，减少虚方法
7. 方法内联（重要）
8. 数组边界检查清除
9. 公共子表达式清除
3.  逃逸分析（前沿）



