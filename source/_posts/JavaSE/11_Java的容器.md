---
title: 11_Java的容器
date: 2021-7-13
description: 2021年7月，我决定学习Java，后端之路从这里开始。
tags:
  - 学习
categories: JavaSE基础
---

# 一、集合的好处
## 1、数组
1. 长度指定后，不能更改
2. 元素单一
3. 增加删除元素比较麻烦

## 2、数组扩容
1. 创建新的数组
2. 拷贝原先数组
3. 添加新的数组元素

## 3、集合
1. 动态保存任意多个对象
2. 提供了很多操作对象的方法
3. 添加，删除新的元素代码更简介

# 二、集合的框架体系

![](https://img.yublog.top/img/202210280812299.png)

![](https://img.yublog.top/img/202210280814241.png)

## 1、集合分类
1. 单列集合
	- 放单个对象
	- Collection接口两个重要的子类List Set 这就是单列集合
2. 双列集合
	- 放键值对
	- Map接口的实现子类是双列集合 存放K-V

# 三、Collection接口
## 1、Collection实现类的特点
1. 可以存放多个元素，每个元素可以是Object
2. 有些可以存放重复元素，有些不可以
3. 有些是有序的（List），有些事无序的（Set）
4. COllection接口没有直接的实现子类，是通过他的子接口List和Set来实现的

## 2、Collection常用方法
1. add——添加单个元素
2. remove——删除指定元素
3. contains——查找元素是否存在
4. size——获取元素个数
5. isEmpty——判断是否为空
6. clear——清空
7. addAll——添加多个元素
8. containsAll——判断多个元素是否都存在
9. removeAll——删除多个元素

## 3、迭代器遍历
### 3.1 迭代器说明
1. Iterator对象是迭代器，可以用来遍历集合元素
2. 所有实现Collection接口的集合都有一个Iterator()方法，用以返回一个迭代器
3. Iterator用于遍历集合，本身不存放对象

### 3.2 迭代器执行原理
1. 得到一个集合的迭代器
	```java
	Iterator iterator = coll.iterator();
	```

2. hasNext()判断是否还有下一个元素
	```java
	while(iterator.hasNext()){}
	```

3. 调用next()返回指针指向的集合元素，并且把指针向下移动
	```java
	iterator.next();
	```
	**注意：如果找不到集合元素会抛出NoSuchElementException异常**

4. 重置iterator()
	为了把指针放到开头
	```java
	iterator = coll.iterator();
	```

## 4、增加For循环
1. 可以在COllection中使用，也可以在集合中使用
2. 底层实际上也是迭代器
3. 简化版的迭代器遍历
4. 代码
	```java
	for(Object obj : col){
		
	}
	```

# 四、List接口
## 4.1 List基本介绍
1. List集合中的元素是有序的，添加和取出的顺序是一致的，而且元素可以重复
2. List集合中的每一个元素都有其对应的顺序索引

## 4.2 List常用方法
1. add(int index, Object ele)——在index位置插入ele元素
2. addAll(int index, Collection eles)——在index位置将所有的eles元素加进来
3. get(int index)——获取指定索引的元素
4. indexOf(Object obj)——返回obj首次出现的位置
5. lastIndexOf(Object obj)——返obj末次出现的位置
6. remove(int index)——移除指定索引的元素
7. set(int index, Object ele)——修改指定索引的元素

## 4.3 List的三种遍历方式
1. 迭代器
2. 增强for
3. 使用普通for

# 五、ArrayList
## 5.1 ArrayList注意
1. 可以加入null，并且是多个
2. ArrayList是由数组来实现存储数据的
3. ArrayList基本等同于Vector，除了ArrayList是线程不安全，前面没有加synchronized关键字（效率比较高），多线程考虑用Vector

## 5.2 ArrayList源码分析结论
1. ArrayList中维护了一个Object类型的数组elementData，transient Object[] elementData;
	transient 表示瞬间，短暂的，表示该属性不会被序列化
2. 创建ArrayList对象时，如果使用无参构造器，则初始elemetData容量为0，第1次添加，扩容到10，如再次扩容，则扩容1.5倍
3. 如果使用指定大小的构造器，初始化大小为指定大小，扩容时，直接按1.5倍扩容

## 5.3 ArrayList源码分析
```java
// 测试代码
ArrayList arrayList = new ArrayList();
for(int i = 0; i < 10; i++){
	arrayList.add(i);
}
```
### 5.3.1 ArrayList无参构造分析
1. 调用ArrayList的无参构造，并且初始化elementData
	![](https://img.yublog.top/img/202210281335866.png)
	- DEFAULTCAPACITY_EMPTY_ELEMENTDATA是一个空数组

2. 初始化完毕后，进入for循环，对i进行包装
	![](https://img.yublog.top/img/202210281337522.png)

3. 调用add方法
	![](https://img.yublog.top/img/202210281339823.png)

4. ensureCapacityInternal()确认要不要扩容
	![](https://img.yublog.top/img/202210281345169.png)
	- 调用ensureExplicitCapacity()肯定容量
	- 肯定容量之前调用calculateCapacity来计算容量

5. 计算容量
	![](https://img.yublog.top/img/202210281349031.png)
	- 如果elementData是一个空数组则返回一个较大的容量
	- DEFAULT_CAPACITY = 10

6. 计算完容量后，调用ensureExplicitCapacity
	![](https://img.yublog.top/img/202210281353962.png)
	- modCount++ 是计算修改的次数，防止多线程修改
	- minCapacity - elementData.length > 0 是当前集合容量是否满足最小容量
	- 不满足时，调用grow()进行扩容

7. 真正的扩容grow
	![](https://img.yublog.top/img/202210281357762.png)
	- newCapacity = oldCapacity + (oldCapacity >> 1) 新容量等于旧容量加旧容量的0.5倍
	- 如果计算后的新容量不够最小需要的容量，则把新容量直接变成最小需要的容量
	- 如果新容量的大小比规定数组的大小还大，则使用hugeCapacity()方法进行处理
	- 最终，调用数组的copyOf进行扩容，调用此方法的目的是，防止原来的数据丢失

8. elementData扩容完毕，进行返回
	![](https://img.yublog.top/img/202210281339823.png)
	- elementData[size++] = e 对elementData以size为下标，进行赋值然后size++，目的是让下标后移一位

### 5.3.2 ArrayList的有参构造
1. 调用ArrayList的有参构造，并且初始化指定容elementData
	![](https://img.yublog.top/img/202210281425003.png)

# 六、Vector
## 6.1 Vector注意
1. Vector底层也是对象数组
2. Vector时线程同步的，即线程安全，Vector类的操作方法带有synchronized
3. 在开发中，需要线程同步安全，考虑使用Vector

## 6.2 Vector和ArrayList比较
1. 底层都是可变数组
2. ArrayList是1.2出现的，Vector是1.0出现的
3. ArrayList是线程不安全的，但是效率高。Vector是线程安全的，但是效率低
4. ArrayList有参扩容是1.5倍，无参扩容第一次是10，第二次以后是1.5倍
	Vector有参扩容是2倍，无参扩容第一次是10，第二次以后是2倍

# 七、LinkedList
## 7.1 LinkedList说明
1. 底层实现了一个**双向链表**和**双端队列**特点
2. 可以添加任意元素，可以重复，包括null
3. 线程不安全，没有实现同步

## 7.2 LinkedList底层结构
1. LinkedList底层维护了一个双向链表
2. LinkedList中维护了两个属性first和last分别指向首节点和尾节点
3. 每个节点(Node对象)，里面又维护了prev，next，item三个属性，其中通过prev指向前一个节点，通过next指向后一个节点。最终实现双向链表
4. 所以LinkedList的元素的添加和删除，不是通过数组完成的，相对来说效率高
![](https://img.yublog.top/img/202210281457679.png)

## 7.3 LinkedListCRUD源码分析
### 7.3.1 add()
1. 调用LinkList无参构造。这时，LinkList的属性first=null，last=null
2. 执行添加add
3. 将新的结点加入到双向链表的最后

### 7.3.2 remove()
1. 默认删除第一个
2. unlinkFirst()删除元素的核心
	- 把第一个的last变成null，就不会指向下一个节点
	- 把farst指向下一个节点
	- 把下一个节点的prev变成null，就不会指向上一个节点
	- 这时候所有删除的节点，farst不指向他，last=null，下一个节点也不指向他，他就会被GC回收

### 7.3.3 set()
1. set(int index, Object obj)

### 7.3.4 get()
1. get(int index)

# 八、实际开发中的选择
## 8.1 ArratList和LinkedList的比较
1. ArrayList底层结构是可变数组，LinkedList底层结构是双向链表
2. ArrayList增删效率低，因为要数组扩容。LinkedList增删效率高，通过链表追加
3. ArrayList改查效率高，LinkedList改查效率低

## 8.2 选择
1. 如果改查多，选择ArrayList
2. 如果增删多，选择LinkedList

# 九、Set接口
## 9.1 Set基本介绍
1. 无序（添加和取出顺序不同，没有索引）
2. 不允许重复，最多包含一个null
3. 取出数据的顺序是固定的
## 9.2 Set遍历
1. 可以用迭代器
2. 可以用增强for

# 十、HashSet
## 10.1 HashSet说明
1. 实现了Set接口
2. 底层是一个HashMap
3. 可以存放null值，但只能存一个
4. HashSet不确保元素是有序的，取决于hash后，再确定索引的结构
5. 不能有重复的元素/对象

## 10.2 HashSet底层机制
1. HashSet底层是HashMap，HashMao底层是数组+链表+红黑树
2. 添加元素时，先得到hash值会转换成索引值
3. 找到存储数据表table，看这个索引位置是否已经存放元素
4. 如果没有，直接加入
5. 如果有，调用equals比较，如果相同，就放弃添加，如果不同则添加在最后
6. 在Java8中，如果一条链表的个数达到TREEIFY_THRESHOLD默认是8，并且table的大小>=Min_TREEIFY_CAPACITY(默认64)就会进行树化(红黑树)

## 10.3 HashSet底层源码分析
```java
HashSet hashSet = new HashSet();
hashSet.add("java");
hashSet.add("php");
hashSet.add("java");
System.out.println(hashSet);
```
1. 第一次执行，new对象底层调用HashMap的无参构造
2. 第一次添加，add方法底层调用put(),PRESENT是一个展位Object对象，是一个空对象
	![](https://img.yublog.top/img/202210300952115.png)
3. put方法有两个参数，key和val，key为需要加入的值，val是PRESENT，put调用putVal()
	![](https://img.yublog.top/img/202210300956659.png)
4. hash()通过算法，算出一个hash值
	![](https://img.yublog.top/img/202210300957867.png)
5. 进入putVal()
	- 首先看一下HashMap的table是不是null或者长度是0，如果是就把table变成16，调用resize()
		![](https://img.yublog.top/img/202210301007073.png)
	- 第二步 通过(n - 1) & hash算出tab的索引值，如果该索引值中是null就在该索引值的tab中new一个Node
		![](https://img.yublog.top/img/202210301007240.png)
	- 第三步 判断当前size是不是大于threshold，threshold是临界值，当达到该临界值时进行扩容，怎么来的呢？看resize()中源码，里面有一个加载因子0.75乘上table的大小得到的
		![](https://img.yublog.top/img/202210301017004.png)
	- 第四步 长度足够，继续执行afterNodeInsertion()，但是该方法是空的，主要是留给HashMap的子类去实现，最后返回null
6. 返回到add进行判断，如果是null则添加成功
7. 第二次添加，因为table不等于空，所以跳过，创建table长度的代码
	![](https://img.yublog.top/img/202210301023381.png)
8. 后续执行和第一次一样
9. 第三次添加，因为是添加的值是java所以跳过判断table是不是null和他的长度，因为该索引上不是null，所以继续跳过执行else中的代码
	- 首先，看一下原先的hash是不是和现在值的hash一致，一致的话，把p赋值给e
	- 最后返回oldValue
10. 因为返回到add不等于null，所以添加失败

## 10.4 HashSet注意
1. 为了使集合不添加重复对象，所以一定要根据实际要求重写equals和hashCode

# 十一、LinkedHashSet
## 11.1 LinkedHashSet说明
1. 是HashSet的一个子类，底层是LinkedHashMap，底层维护的是一个数组+双向链表
2. LinkedHashSet根据元素的hashCOde值来确定元素的存储位置，同时使链表维护元素的次序，这使得元素看起来是以插入顺序保存的
3. LinkedHashSet也是不允许添加重复元素

## 11.2 LinkedHashSet底层源码说明
1. 在LinkedHashSet中维护了一个hash表和双向链表(LinkedHashSet有headhetail)
2. 在每一个节点有before和after属性，这样可以形成双向链
3. 在添加一个元素时，先求hash值，再求索引，确定该元素再table的位置，然后将添加的元素加入到双向链表(如果已经存在，不添加，和Hashset一样)
	```java
	taiil.next = newElement;
	newElement.pre = tail;
	tail = newElement;
	```
4. 这样的话，我们的遍历LinkedHashSet，也能确保插入顺序和遍历顺序一致

# 十二、Map接口
## 12.1 Map接口实现的特点
1. Map用于保存具有映射关系的数据：Key-Val
2. Map中的Key和Val可以是任何引用类型的数据，会封装到HashMap对象中
3. Map中的Key不允许重复
4. Map中的Val可以重复
5. Map中的key可以是null，Val也可以是null，注意Key为null，只能有一个，Val为null可以有多个
6. 常用String类作为Map的key
7. Key和Val之间存在单向一对一关系，即指定key总是可以找到对应的value
8. Key和Val存放在Node中，Node实现了Entry接口
	![](https://img.yublog.top/img/202210301340906.png)
	- 为了遍历方便，还会创建EntrySet集合，该集合存放的元素类型是Entry，而Entry对象就有K，V，只是引用了一下Node中的K，V
	- entrySet中，定义的类型是Map.Entry，但是实际上存放的还是HashMap$Node
	- 这时因为HashMap$Node implements Map.Entry，多态
	- 总之：**Node封装到Entry中，再把Entry放入EntrySet集合中，封装的时候只是引用，对象地址都相同**

## 12.2 Map接口常用方法
1. put()——添加
2. remove()——根据键删除映射关系
3. get()——根据键获取值
4. size()——获取元素个数
5. isEMmpty()——判断个数是否为0
6. clear()——清楚
7. containsKey()——查找键是否存在
	
## 12.3 Map接口遍历方法
### 12.3.1 第一组
先获取所有key(KeySet)，通过key取出val
1. 增强for
2. 迭代器
### 12.3.2 第二组
把所有的val取出，用map.values()
1. 迭代器
2. 增强for
3. for遍历
### 12.3.3 第三组
通过EntrySet来获取k-v
1. 增强for
	- 将entry转成Map.Entry
	- 调用getKey()和getValue()
2. 迭代器
	- 将entry转成Map.Entry
	- 调用getKey()和getValue()

# 十三、HashMap
## 13.1 HashMap源码分析
1. HashMap底层维护Node类型的数组table，默认是null
2. 当创建对象时，将加载因子(loadfactor)初始化为0.75
3. 当添加key-val时通过key和哈希值得到table的索引，然后判断该索引处是否有元素，如果没有元素直接添加，如果相同，直接替换，如果不相同需要判断是树结构还是链表结构，做出相应处理。如果添加时发现容量不够，则需要进行扩容。
4. 第一次添加，则徐涛扩容table容量为16，临界值(threshold)为12
5. 以后再扩容，需要扩容table容量为原来的2倍，即24依次类推
6. 在Java8，如果一条链表的袁术个数超过TREEIFY_THRESHOLD(默认是8)，并且table的大小>=MIN_TREEIFY_CAPACITY(默认是64)，就会进行树化(红黑树)

# 十四、Hashtable
## 14.1 Hashtable说明
1. 存放的的元素是键值对：即K-V
2. Hashtable的键和值都不能为null，如果是null则抛出NullPointerException异常
3. Hashtable使用方法本上和HashMap一样
4. Hashtable是线程安全的(synchronized)，HashMap是线程不安全的
5. 底层
	- 底层有数组 Hashtable$Entry[] 初始化大小为11
		![](https://img.yublog.top/img/202210301607656.png)
	- 临界值 threshold 8 = 11 * 0.75
6. 扩容
	![](https://img.yublog.top/img/202210301628954.png)
	- 当达到临界值8时，进行扩容，新容量等于老容量乘2加1

# 十五、Properties
## 15.1 Properties基本介绍
1. Properties类继承Hashtable类并且实现了Map接口，也是使用一种键值对的形式保存数据
2. 他的使用特点和Hashtable类似
3. Properties还可以用于从xxx.properties文件中，加载数据到Properties类对象，并进行读取和修改
4. Properties，值不能为空，如果是空，则抛出NullPointerException

# 十六、TreeSet
## 16.1 TreeSet特点
1. 当使用无参构造时，输出按自然顺序排列，字母小到大，数字小到大，和输入顺序不同
2. 指定排序，传入一个比较器，Comparator实现该接口
3. 底层是TreeMap