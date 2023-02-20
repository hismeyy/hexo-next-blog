---
title: 8_Java的注解
date: 2021-7-9
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---

# 一、什么是注解

1. 注解(Annotation)被称为元数据(Metadata)，用于修饰解释包、类、方法、属性、构造器、局部变量等数据信息
2. 和注释一样，注解不影响程序逻辑，但注解可以被编译或运行，相当于嵌入代码中的补存信息
3. 在javaSE中，注解的使用目的比较简单，标记过时功能，忽略警告等，当在JavaEE中比较重要
# 二、注解的使用
1. 使用Annotation时要在其前面加@ 符号，并把该Annotation当成一个修饰符使用。用于修饰它支持的程序元素
# 三、常用的注解
1. @Override 限定某个方法，是重写父类方法，该注解只能用于方法
2. @Deprecated 用于表示某个程序元素(类，方法等)已过时
3. @SuppressWarnings 抑制编译器警告

# 四、四大元注解
1. Retention
	指定注解的范围，三种
		- SOURCE
		- CLASS
		- RUNTIME
2. Target
	指定注解可以在哪些地方使用
3. Documented
	指定该注解是否会在JavaDoc体现
4. Inherited
	子类会继承父类注解
