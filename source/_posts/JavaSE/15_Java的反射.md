---
title: 15_Java的反射
date: 2021-7-22
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---

# 一、反射机制简介

## 1、JavaRefilection是什么？
1. 反射机制允许程序在执行期借助Reflection的一些API可以获取任何类的内部信息，比如成员变量，构造器，成员方法等。并能操作对象的属性和方法，反射是设计模式和框架底层最重要的！
2. 加载类之后，会在对中产生一个Class类型的对象，一个类只有一个Class类型对象，这个对象包含了类的完整结构信息，通过对象可以得到类的结构。

## 2、Java程序在计算机中的三个阶段
![](https://img.yublog.top/img/202211060936751.png)

## 3、反射机制可以干什么？
1. 在运行时判断任意一个对象所属的类
2. 在运行时构造任意一个类的对象
3. 在运行时得到任意一个类具有的成员变量和方法
4. 在运行时调用任意一个对象的成员变量和方法
5. 生产动态代理

## 4、反射相关的主要类
1. java.lang.Class 代表一个类
	Class对象表示某个类加载后在堆中的对象
2. java.lang.reflect.Method 代表类的方法
	Method对象表示某个类的方法
3. java.lang.reflect.Field 代表类的成员变量
	Field对象表示某个类的成员变量
4. java.lang.reflect.Constructor 代表类的构造器
	Constructor对象表示某个类的构造器

## 5、反射机制的优缺点
1. 优点
	- 可以动态的创建和使用对象（框架核心）
	- 使用灵活，无反射，无框架
2. 缺点
	- 反射基本时解释执行，效率低，执行速度慢

## 6、反射机制效率调优
1. Method和Field，Constructor对象都有setAccessible()方法
2. setAccessible的作用是启动和禁止访问安全检查的开关
3. 参数为true时取消访问检查，可以提高反射效率

# 二、Class类的介绍
## 1、结构
![](https://img.yublog.top/img/202211060953546.png)

## 2、介绍
1. Class也是类，也继承了Object类
2. Class类对象不是new出来的，而是系统创建的
```java
public Class<?> loadClass(String name) throws ClassNotFoundException {
        return loadClass(name, false);
}
```
3. 对于某个类的CLass对象，在内存中只有一份，只加载一次
4. 每个类的实例都会记得自己时由哪个Class实例所生成的
5. 通过CLass对象的API可以完整的得到对于类的结构
6. Class对象存放在堆中
7. 类的字节码二进制数据，存放在方法区，有的地方称为类的原数据（包括方法代码，变量名，方法名，访问权限等）

# 三、Class类常用方法

1. 返回指定类名name的Class对象

   ```java
   forName(String name)
   ```

2. 调用缺省构造函数，返回该Class对象的一个实例

   ```java
   Object newInstance()
   ```

3. 返回此Class对象所表示的实体（类，接口，数组类，基本类型）名称

   ```java
   getName()
   ```

4. 返回当前Class对象的父类的Class对象

   ```java
   getSuperClass()
   ```

5. 返回当前Class对象的接口

   ```java
   getInterfaces()
   ```

6. 返回该类的类加载器

   ```java
   getClassLoader()
   ```

7. 返回表示此Class所表示的实体的超类的Class

   ```java
   getSuperclass()
   ```

8. 返回包含默写Constructor对象的数组

   ```java
   getConstructors()
   ```

9. 返回Field对象的一个数组

   ```java
   getDeclaredFidlds()
   ```

10. 返回一个Method对象

    ```java
    getMethod()
    ```

# 四、获取Class对象
1. 用于配置文件，读取类全路径，加载类
  ```java
  forName()
  ```

2. 用于参数传递，比如通过反射得到对应构造器对象
  ```java
  .class
  ```

3. 通过创建好的对象，获取Class对象对象
  ```java
  .getClass()
  ```

4. ClassLoader cl = 对象.getClass().getClassLoader();

  ```java
  Class clazz4 = cl.loadClass("类的全类名");
  ```

5. 基本数据类型

  ```java
  Class cls = 基本数据类型.class
  ```

6. 基本数据类型的包装类

  ```java
  Class cls = 包装类.TYPE
  ```

# 五、哪些类型有Class对象
1. 外部类，成员内部类，静态内部类，局部内部类，匿名内部类
2. interface 接口
3. 数组
4. enum 枚举
5. annotation 注解
6. 基本数据类型
7. void

# 六、类加载
## 1、基本说明
1. 反射机制时Java实现动态语言的关键，可以通过反射实现类动态加载
2. 静态加载和动态加载
	- 静态加载：编译时加载相关的类，如果没有就会报错，依赖性太强。
	- 动态加载：运行时加载需要的类，如果运行时不用该类，则不报错，降低了依赖性。

## 2、类加载时机
1. 当创建对象时（new）
2. 当子类被加载时
3. 调用类中的静态成员时
4. 通过反射

## 3、类加载过程图
![](https://img.yublog.top/img/202211061358558.png)

## 4、类加载各个阶段
![](https://img.yublog.top/img/202211061406947.png)

1. 加载阶段
JVM在该阶段的主要目的是将字节码从不同的数据源（可能是class文件、可以能是jar包、甚至是网络）转化为二进制字节流加载到内存中，并生成一个代表该类的java.lang.Class对象
2. 连接阶段-验证

   - 目的是为了确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全
   - 包含：文件格式验证（是否以魔数oxcafebabe开头）、元数据验证、字节码验证和符号引用验证
   - 考虑使用-Xverify:none参数来关闭大部分的类验证措施，缩短虚拟机类加载的时间
3. 连接阶段-准备
   - JVM会在该阶段对静态变量，分配内存并初始化（对应数据类型的默认初始值，如0、0L、null、false等）。这些变量所使用的内存都将在方法区中进行分配
4. 连接阶段-解析
   - 虚拟机将常量池内的**符号引用**替换为**直接引用**的过程
5. initialization（初始化）

   - 正真执行类中定义的Java代码，此阶段是执行clinit()方法的过程

   - clinit()方法是由编译器按语句在源文件中出现的顺序，依次自动收集类中的所有**静态变量**的赋值动作和静态代码块中的语句，并进行合并

   - 虚拟机会保证一个类的clinit()方法在多线程环境中被正确地加锁，同步，如果多个线程同时去初始化一个类，那么只会有一个线程去执行这个类clinit()方法，其他线程都需要阻塞等待，知道活动线程执行clinit()方法完毕

# 七、通过反射获取结构类信息
## 1、ava.lang.Class类
1. getName 获取全类名
2. getSimpleName 获取简单类名
3. getFields 获取public修饰的属性，包含本类以及父类的
4. getDeclaredFields 获取本类中所有属性
5. getMethods 获取所有public修饰的方法，包含本类以及父类
6. getDeclaredMethods 获取本类中所有的方法
7. getConstructors 获取所有public修饰的构造器，包含本类
8. getDeclaredConstructors 获取本类中所有构造器
9. getPackage 以Package心事返回包信息
10. getSuperClass 以Class心事返回父类信息
11. getInterfaces 以Class[]形式返回接口信息
12. getAnnotations 以Annotation[] 形式返回注解信息

## 2、java.lang.refkect.Field
1. getModifiers 以int形式返回修饰符【默认修饰符0，public是1，private是2，protected是4，static是8，final是16】
2. getType 是以Class心事返回类型
3. getName 返回属性名

## 3、java.lang.reflect.Method类
1.  getModifiers 以int形式返回修饰符【默认修饰符0，public是1，private是2，protected是4，static是8，final是16】
2. getReturnType 以Class形式获取 返回类型
3. getName 返回方法名
4. getParameterTypes 以Class[]返回参数类型组

## 4、java.lang.reflect.Constructor类
1. getModifiers 以int形式返回修饰符
2. getName 返回构造器名（全类名）
3. getParameterTypes 以Class[]返回参数类型数组


# 八、通过反射创建对象
1. 调用类中的public修饰的无参构造器
2. 调用类中的指定构造器
3. Class类相关方法
	- newInstance 调用类中的无参构造器，获取对应类的对象
	- getConstructor 根据参数列表，获取对应的public构造器对象
	- getDecalaredConstructor 根据参数列表，获取对应的所有的构造器对象
4. Constructor类相关方法
	- setAccessible 爆破 
	- newInstance 调用构造器

# 九、通过反射访问类中的成员
1. 根据属性名获取Field对象
Field f = clazz对象.getDeclaredField(属性名)
2. 爆破
f.setAccessible(true)
3. 访问
f.set(obj,值)
f.get(obj)
4. 如果是静态属性，则set和get中的参数obj可以写成null

# 十、通过反射访问类中的方法
1. 根据方法名和参数列表获取Method方法对象
	Method m = clazz.getDeclaredMethod(方法名, XX.class)
2. 获取对象 Object o = clazz.newInstance()
3. 爆破 m.setAccessible(true)
4. 访问 Object returnValue = m.invoke(o, 实参列表)
5. 注意 如果是静态方法，则invoke的参数o可以写成null