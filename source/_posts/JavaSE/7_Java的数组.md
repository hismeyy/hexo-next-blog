---
title: 7_Java的数组
date: 2021-7-8
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---

# 一、什么是数组

1. 数组是相同类型数据的有序集合
2. 通过下标访问

# 二、数组声明

1. 声明数组

   ```java
   int[] i;	// 常用
   int i[];	// 为了适应C和C++
   ```

2. 创建数组

   ```java
   int[] i = new int[10];
   ```

3. 获取数组长度

   ```java
   int i = array.length
   ```

4. 初始化

   - 静态初始化

     ```java
     int[] a = {1, 2, 3};
     ```

   - 动态初始化

     ```java
     int[] b = new int[10];
     // 未赋值时会给一个默认值
     // 后期手动赋值
     ```

# 三、内存分析

1. Java内存分析

   ![](https://img.yublog.top/img/202211041505098.png)

   

# 四、数组基本特点

1. 数组一旦创建就不可改变

2. 数组类型必须相同

3. 数组类型可以是任意类型

4. 数组本身是一个对象，所以在堆中

5. 下表越界异常

   java.lang.ArrayIndexOutOfBoundsException

# 五、数组使用

1. 普通for循环
2. for-each循环
3. 数组作方法入参
4. 数组做返回值

# 六、多维数组

1. 数组中有数组
2. 嵌套循环遍历数组

# 七、Arrays类

1. 数组工具类

   java.util.Arrays

2. 查看JDK文档

3. 常用功能

   - 给数组赋值

     fill方法

   - 对数组排序

     sort方法

   - 比较数组

     equals方法

   - 查找数组元素

     binarySearch方法可以对排序好的数组进行二分查找
     
# 八、排序算法

[十大经典的排序算法](https://www.runoob.com/w3cnote/ten-sorting-algorithm.html)

# 九、稀疏数组

把数组内不需要的值压缩掉只记录行数、列数、个数和对应坐标的值