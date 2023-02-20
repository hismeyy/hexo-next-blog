---
title: 13_Java的IO流
date: 2021-7-17
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---

# 一、文件

## 1、什么是文件？
1. 文件就是保存数据的地方

2. 文件在程序中以流的形式操作

   文件流

   - 输入流：文件（磁盘）—> Java程序（内存）

   - 输出流：Java程序（内存）—> 文件（磁盘）

## 2、文件操作

### 2.1 常用构造器

1. 根据路径构建File对象

   ```java
   new File(String pathname);
   ```

2. 根据父目录文件+子路径构建File对象

   ```java
   new File(String parent, String child);
   ```

3. 根据父目录+子路径构建File对象

   ```java
   new File(File parent, String child);
   ```

### 2.2 File类图

![](https://img.yublog.top/firm-img/202301031008385.png)

### 2.3 获取文件相关信息

1. 获取文件名称

   ```java
   getName()
   ```

2. 获取文件绝对路径

   ```java
   getAbsoultePath()
   ```

3. 获取文件父级目录

   ```java
   getParent()
   ```

4. 获取文件中字节大小

   ```java
   length()
   ```

5. 判断文件是否存在

   ```java
   exists()
   ```

6. 判断是否为一个文件

   ```java
   isFile()
   ```

7. 判断是否为一个目录

   ```java
   isDirectory()
   ```

### 2.4 目录的操作和文件删除

1. 创建一级目录

   ```java
   mkdir()
   ```

2. 创建多级目录

   ```java
   mkdirs()
   ```

3. 三处空目录或文件

   ```java
   delete()
   ```

# 二、IO流的分类

## 1、IO包

存放在java.io包下，存放着流类和接口

## 2、流的分类

###  2.1 按数据单位分

1. 字节流（8bit）二进制文件

2. 字符流（按字符）文本文件

###  2.2 按数据流的方向

1. 输入流
2. 输出流

###  2.3 按流的角色

1. 节点流
2. 处理流/包装流

### 2.4 分类表

| 抽象基类 |    字节流    | 字符流 |
| :------: | :----------: | :----: |
|  输入流  | InputStream  | Reader |
|  输出流  | OutPutStream | Writer |

### 2.5 节点流和处理流
![](https://img.yublog.top/img/202211031513164.png)
	
节点流和处理流的区别和联系

- 节点流是底层流/低级流，直接跟数据源相接
- 处理流(包装流)包装节点流，既可以消除不同节点的实现差异，也可以提供更方便的方法来完成输入输出
- 处理流对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连
- 处理流的功能主要体现在一下两个方面
  1. 性能的提高：主要以增加缓冲的方式来提高输入输出的效率
  2. 操作的便捷：处理流可能提供了一系列便捷的方法来一次输出大批量的数据。使用更加灵活方便

# 三、字节流

![](https://img.yublog.top/firm-img/202301031018417.jpg)

## 1、字节输入流（InputStream）
1. FileInputStream
2. FilterInputStream
   - BufferedInputStream
   - DataInputStream
   - PushbakInputStream
3. ObjectInputStream
4. PipedInputStream
5. SequenceInputStream
6. StringBufferInputStream
7. ByteArrayInutStream

![](https://img.yublog.top/img/202211030811245.png)

### 1.1 FileInputStream
1. 文件字节输入流
2. 构造

    ![](https://img.yublog.top/img/202211030815011.png)

3. 使用
	- new对象
	- 调用方法
	- 抛异常
	- 关闭流
4. 常用方法

    ![](https://img.yublog.top/img/202211030818426.png)

### 1.2 BufferedInputStream
1. 缓冲字节输入流，操作二进制文件
2. 使用
	- new对象
	- 调方法
	- 关闭流

### 1.3 ObjectInputStream
1. 对象字节输入流


## 2、字节输出流（OutputStream）

1. FileOutputStream
2. FilterOutputStream
   - BufferedOutputStream
   - DataOutputStream
   - printStream
3. ObjectOutputStream
4. PipedOutputStream
5. ByteArrayOutputStream

	
### 2.1 FileOutputStream
1. 文件字节输出流
2. 构造

    ![](https://img.yublog.top/img/202211030821768.png)

3. 使用
	- new对象
	- 调方法
	- 刷新流
	- 抛异常
	- 关闭流
4. 常用方法

    ![](https://img.yublog.top/img/202211030822759.png)

### 2.2 BufferedOutputStream
1. 缓冲字节输出流

### 2.3 ObjectOutputStream
1. 对象字节输入流

### 2.4 PrintStream
1. 字节打印流
2. 底层是write
3. 修改输出位置默认是显示器
	- System.setOut()

## 3、序列化和反序列化
1. 序列化就是在保存数据时，保存数据的值和数据类型
2. 反序列化就是在恢复数据时，恢复数据的值和数据类型
3. 需要让某个对象支持序列化机制，则必须让其类是可序列化的，需要实现两个接口
	- Serializable是一个标记接口
	- Externalizable
3. 注意
	- 要实现Serializable
	- 序列化的类中建议添加serialVersionUID，为了提高版本的兼容性
	- 序列化对象时，默认将所有属性都进行序列化，除了static或者transient修饰的成员
	- 序列化对象时，要求里面属性的类型也实现序列化接口
	- 序列化具备可继承性，也就是如果某类已经实现了序列化，则他的所有子类默认实现序列化
# 四、字符流

![](https://img.yublog.top/firm-img/202301031028940.jpg)

## 1、字符输入流（Reader）

1. BufferedReader
2. InputStreamReader
   - FileReader
3. StringReader
4. PipedReader
5. ByteArrayReader
6. FilterReader
   - PushbackReader

### 1.1 FileReader
1. 文件字符输入流
2. 构造

    ![](https://img.yublog.top/img/202211030831800.png)
    
3. 使用

      - new对象
      - 调方法
      - 抛异常
      - 关闭流
4. 方法

    ![](https://img.yublog.top/img/202211030836355.png)

### 1.2 BufferedReader
1. 缓冲字符输入流(处理流)，读取文本文件
2. 使用
	- new对象
	- 调方法
	- 关闭流，底层关闭的是传入的流
	
### 1.3 InputStreamReader
1. 输入转换流 字节转字符

## 2、字符输出流（Writer）