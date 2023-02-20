---
title: 16_Java的图形化
date: 2021-7-25
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---

# 一、简介
1. Gui核心：Swing，AWT
2. 缺点
	1. 界面不好看
	2. 需要jre环境
3. 为什么要学习Gui？
	1. 可以写一些小工具
	2. 可能会维护到Swing界面
	3. 了解MVC架构，了解监听

# 二、AWT
## 1、结构
![](https://img.yublog.top/img/202211011226042.png)

## 2、组件和容器
### 2.1 第一个窗口程序
1. new Frame
2. 设置可见性 setVisible(true)
3. 设置窗口大小 setSize()
4. 设置背景颜色 setBackground(new Color())
5. 设置初始位置 setLocation()
6. 设置大小固定 setResizable()
7. 设置布局 setLayout()
8. 设置大小和坐标 setBounds()

2.2 创建多个窗口

1. 进行封装

### 2.3 面板Panel
1. new Panel
2. 调用frame的add()可以把panel放入

### 2.4 组件
1. 按钮		Button
2. 输入框	TextField
3. 标签		Label

### 2.5 布局管理器
1. 分类
	- 流式布局	FlowLayout
		需要在setLayout中new
	- 东南西北中	BorderLayout	
		添加的时候指定
	- 表格布局	GridLayout
		需要在setLayout中new
	- 绝对布局
		null

### 2.6 事件监听
1. 按下按钮：addActionListener()

### 2.7 画笔
在frame中重写paint方法

### 2.8 鼠标监听
addMouseListener()

### 2.9 窗口监听
addWindowListener()

### 2.10 键盘监听
addKeyListener()

# 三、Swing
## 1、Swing说明
封装了AWT，比AWT更好看

## 2、窗口和面板

### 2.1 关闭事件
可以直接调用setDefaultCloseOperation()设置关闭事件

### 2.2 获取容器
getContentPane()

## 3、弹窗
1. 设置按钮监听事件
2. 提供弹窗类需要继承JDialog

## 4、组件
1. 标签		Label
2. 面板		JPanel和JScroll
3. 按钮
	- 单选		JRadioButton需要分组ButtonGroup
	- 复选		JCheckBox
4. 列表
	- 下拉框	JComboBox
	- 列表框	JList
5. 文本框	
	- 文本框	JTextField
	- 密码框	JPasswordField	
	- 文本域	JTextArea


> GUI笔记大概OK，后续可能会有点添加，方法涉及太多，结合百度。冲！！！