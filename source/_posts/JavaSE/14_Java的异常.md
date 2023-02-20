---
title: 14_Java的异常
date: 2021-7-20
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---
# 一、异常处理

## 1、异常分类

1. 检查性异常
2. 运行时异常
3. 错误

## 2、异常的体系结构

![](https://img.yublog.top/img/202211041506554.png)

## 3、捕获和抛出异常

1. 关键字

   try

   catch

   finally

   throw

   throws

2. 快捷键

   Ctrl + Alt + T

3. 用finally进行释放资源

## 4、自定义异常

1. 创建自定义异常类
2. 在方法中通过throw关键字抛出异常对象
3. 如果在当前抛出异常的方法中处理异常可以使用try-catch语句捕获并处理，否则在方法的声明处通过throws关键字抛出异常给调用者
4. 在出现异常方法的调用者中捕获并处理异常