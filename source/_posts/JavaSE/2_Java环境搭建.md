---
title: 2_Java环境搭建
date: 2021-7-2
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---

# 一、JDK安装

1. 下载JDK

   Oracle官方下载就行，也可以找国内镜像比较快，最好下载zip，放到一个自己觉得OK的目录下，exe文件也行，记住安装的目录

2. 配置两个环境变量

   **JAVA_HOME：**JDK安装的目录。 如： C:\Program Files\Java\jdk1.8.0_91
   **PATH：**JDK安装目录下的bin目录 如：%JAVA_HOME%\bin

3. 验证JDK安装是否成功

   启动CMD，输入```java -verison```

   如果出现java的版本说明已安装好

# 二、Hello, Java

1. 创建以.java结尾的文件

   例如：Hello.java

2. 编写代码

   ```java
   public class Hello{
   	public static void main(String[] args){
   		System.out.println("Hello, Java");
   	}
   }
   ```

3. 打开CMD进入java文件的目录

   输入 ```javac java文件``` 生成class文件

4. 运行class文件

   在CMD中输入 ``` java class文件```

5. 查看运行结果

   Hello, Java

   出现上面结果说明程序运行正常

6. 可能遇到问题 

   - 字母大小写 java 大小写敏感

   - 文件名和类名必须一致

   - 符合使用了中文
   - 忘记加分号

# 三、Java 的运行机制

1. 编译性

   编译型语言写的程序执行之前，需要一个专门的编译过程，把程序编译成为机器语言的文件，比如exe文件，以后要运行的话就不用重新翻译了，直接使用编译的结果就行了（exe文件），因为翻译只做了一次，运行时不需要翻译，所以编译型语言的程序执行效率高。

2. 解释性

   解释则不同，解释性语言的程序不需要编译，省了道工序，解释性语言在运行程序的时候才翻译，比如解释性java语言，专门有一个解释器能够直接执行java程序，每个语句都是执行的时候才翻译。这样解释性语言每执行一次就要翻译一次，效率比较低。

3. Java执行流程

   java源文件通过编译器编译成class字节码文件，然后运行时，放入类加载器，然后进行字节码校验，最后又解释器进行解释执行。

   ![](../img/202211041507993.png)

# 四、常用的DOS命令

- 进入cmd窗口的方式 系统键+R ；在源文件所在位置的地址栏中输入cmd
- 选择/更换盘符： 盘符名: 如： e:
- 查看当前目录下的内容： dir
- 进入指定文件夹：cd 文件夹名 如：cd corejava(文件名)
- 返回上一级目录：cd..
- 返回根目录下：cd\
- 清屏：cls
- 删除文件：del 文件名 如：del a.txt
- 删除文件夹：rd 文件夹名
- 打开一个新窗口：start
- 退出窗口：exit



# 五、安装IDEA

> 为了更快的编写程序，更好的管理自己的项目，而安装IDEA

1. 安装

   正常在IDEA官方里面进行下载安装

   https://www.jetbrains.com/idea/download/#section=windows

2. 破解

   github中有一个破解方式，可以试试或者其他地方找找很多的

   https://github.com/wapchief/idea_activate

# 六、package包

1. 作用：类似于文件夹，管理编译之后生成的.class文件
2. 语法：package 包名，(包下可以有子包)
3. 位置：必须放在源文件的有效第一行，一个源文件只能包含一个包
4. 带包编译：javac -d . 源文件名.java （自动的生成包结构）
5. 命名规范

   公司域名倒置
6. 导包

   import

# 七、编码规范

1. 良好的格式习惯

   - 层级之间逆序缩进
   - 每行只写一句代码，语句结束以；结尾
2. 良好的标识符命名规范
    标识符：在代码中凡是可以自己命名的都属于标识符

  - 硬性要求[语法要求 必须遵守]
    (1) 只能是字母，数字，下划线，货币符号组成，且数字不能开头
    (2) 不能使用关键字和保留字（goto ,const）（true,false,null在java中有特殊的用法，不能使用）
    (3) 严格区分大小写(小写是关键字，大写不是)
    (4) 长度没有限制
  - 软性要求
    (1) 望文生义 见文知义
    (2) 常用的标识符
    类名：首字母大写 如：HelloWorld 、TestFirst
    包名：全小写 如：test，p1
    变量名|函数名：首单词首字母小写，其他单词首字母大写
    如：helloWorld 、firstVar
    常量名：全大写 如：HELOOWORLD、VAR
3. 良好的注释习惯

   - 单行注释：//注释内容 不能换行

   - 多行注释：/\*注释内容 可换行\*/

   - 文档注释：javadoc -d doc 源文件名.java
     /** 注释内容 可换行 */

# 八、JavaDoc

1. 参数信息

   - @author 作者
   - @version 版本号
   - @since 最早使用jdk版本
   - @param 参数
   - @return 返回值情况
   - @throws 异常抛出情况

2. 百度可以找到很多idea生成JavaDoc文档的内容

3. 在线帮助文档

   https://www.matools.com/api/java8