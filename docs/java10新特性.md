# java10新特性

java10是一个小步迭代版本，号称是有一百多个新特性，但该版本不是一个LTS长期支持的版本，所以可以先了解特性，等java11时考虑应用在生产中。

## 局部变量类型推断

该特性可以说是java10最受关注的特性，该功能是提供了一些语法糖，改善开发体验：

    var list = new ArrayList<String>();
    list.add("hello，world！");
    
看起来越来越像js了，但不同的是java毕竟是强类型的语言，所以以下语句编译报错：
    
    var list = new ArrayList<String>();
    list = new ArrayList<Integer>(); //编译报错
    
而局部变量类型推断只能用于以下场景：

- 局部变量的初始化
- for循环内部的索引变黄
- 传统for循环的声明变量

并且局部变量推断不能用于以下场景

- 方法参数
- 构造函数参数
- 方法返回类型
- 字段
- 捕获表达式

## G1的并行Full GC

改善并行Full GC来改善G1最坏情况的等待时间，G1垃圾收集器是旨在避免完整的集合,但是当并发集合无法足够快地回收内存时，将发生回退Full GC。

旧的G1的Full GC实现使用单线程标记-清除-整理算法。而新的Full GC并行化，并且现在使用与年轻和混合集合相同数量的并行工作线程。

## 新的创建不可变集合的API

java10提供了几个API创建不可变集合：

#### 从已经存在的实例创建不可变集合：

- List.copyOf
- Set.copyOf
- Map.copyOf

#### Stream包中的Collectors类新增几个方法，这些方法使流中的元素收集成一个不可改变的集合

- toUnmodifiableList
- toUnmodifiableSet
- toUnmodifiableMap 

## Optional.orElseThrow() 方法

Optional类中添加新的方法orElseThrow()，首选替换原有的get方法

## 根证书(Root Certificates)

为jdk提供了一个默认的CA证书

## javadoc支持多个样式表

javadoc tool提供了一个新的javadoc命令行选项--add-stylesheet，支持生成的文档中的多个样式表的使用


java10新增功能<a href="https://www.oracle.com/technetwork/java/javase/10-relnote-issues-4108729.html#NewFeature">官方文档</a>
