# java9新特性

java9提供了非常多的新功能，包括备受期待的模块化，可交互REPL工具等

## Java平台模块系统
提供一个新的java编程组件，即模块的概念，它是一个命名的，自描述的代码和数据集合。

模块化系统也解决了过去开发的两个问题：
- 当代码库越来越庞大，很难真正的封装代码，系统之间不同部分的依赖也没有明确声明，每一个公共的类都可以被类路劲下其他的公共类访问到，可能会导致无意调用了不是被公开访问的API
- 不知类路径所需要的jar包是否都已经包含了，或是重复

而JDK本身已经被分为一组模块，改变如下：
- 可以将JDK模块组合成各种配置：
  - 与JRE和JDK的对应配置
  - 可以自定义配置指定模块和其所依赖的模块
  - 配置与Java SE 8中定义的每个压缩配置文件的内容大致相同。
- 重构JDK和JRE运行时映像以适应模块并提高性能，安全性和可维护性。
- 定义一个新的URI方案，用于命名存储在运行时映像中的模块，类和资源，而不会泄露图像的内部结构或格式。
- 从Java运行时映像中删除rt.jar和删除tools.jar。
- 默认情况下，大多数JDK的内部API都不可访问，但在所有或大部分功能都支持替换之前，可以访问一些关键的，广泛使用的内部API。

## 新的版本号格式
提供了简化的版本字符串格式，有助于清楚的区分主要/次要/安全和修补程序的更新版本：

新版本字符串：

    $MAJOR.$MINOR.$SECURITY.$PATCH
    
- $MAJOR是主要版本增加的版本号，例如JDK 9，它包含Java SE平台规范指定的重要新功能。主要版本包含新功能和现有功能的更改，这些功能已提前计划和公布。

- $MINOR 是每次次要更新时递增的版本号，例如错误修复，标准API的修订或相关平台规范范围之外的功能的实现。

- $SECURITY 是安全更新版本增加的版本号，其中包含关键修复程序，包括提高安全性所需的修复程序。

- $PATCH 是包含已一起测试的安全性和高优先级客户修订的版本增加的版本号

## jdk9工具增强功能
- Java平台的REPL(Read-Eval-Print Loop)工具：jshell (java shell)
  提供了交互式的命令行界面，可以用来执行java语句的shell工具。例如可以直接使用jshell编写helloworld程序而不是需要线搭建环境等，适合学习java语言和新的API功能。

- 多版本JAR文件
  扩展jar文件版本使得不同的java版本的class文件共存在一个archive中，多语言JAR（MRJAR）包含特定于特定Java平台版本的类和资源的目录，使用--revision配置指定版本目录

## jdk9的一些小的语言变化

- 允许有效的final变量作为资源try-with-resources声明。

- 钻石操作符的使用升级：如果参数类型和推断类型，则允许钻石操作符有匿名类

      //java8
      Set<String> set = new HashSet<String>(){};
      //java9
      Set<String> set = new HashSet<>(){};
      
- 支持接口的私有方法
  接口内支持定义私有方法，将不会向外暴露API
  
- 下划线在java9中变成关键字，不可用于声明变量和方法，而在java8之前可以。

## jdk9核心库的新功能：
- 紧凑字符串String：
  更改了String类的内部存储实现，为了节省空间，之前String类将字符串存储在Char数组中，每个字符使用两个字节(16位)，而String类的新内部结构是使用byte数组存储，配合了一个encodingFlag来配合编码解码，该更改纯粹是对内部实现的更改，而对公共接口没有什么改变。
  
- 集合的工厂方法：
  使得使用少量的元素创建集合和Map的实例变得简单，功能可用于 Set 和 List，也可用于 Map 的类似形式。此时创建的集合，是不可变集合。

      Set<String> alphabet = Set.of("a", "b", "c");

      Map<String, Integer> map = Map.ofEntries(
              Map.entry("a", 1),
              Map.entry("b", 2),
              Map.entry("c", 3)
      );

- 增强了@Deprecated注释

  - @Deprecated（forRemoval = true）表示将在Java SE平台的未来版本中删除API。

  - @Deprecated（since =“version”）包含Java SE版本字符串，该字符串指示不推荐使用API??元素的时间，以及Java SE 9及更高版本中不推荐使用的那些元素。
    
## jdk9的javadoc新增功能 
- 简化了Doclet API
- 支持生成html5
- 为生成的javadoc添加搜索框，使用搜索框可以查找标记的单词和短语
- 支持模块声明中的文档注释。包括新的命令行选项，用于配置要记录的模块集，并为要记录的任何模块生成新的摘要页面。

## jdk9JVM调优的新增功能
- 统一jvm日志记录

  为JVM的所有组件引入通用日志记录系统。
  
- 删除在jdk8中不推荐使用的GC组合：

  意味着以下GC将不再组合：
  
      DefNew + CMS

      ParNew + SerialOld

      Incremental CMS
      
- 使G1成为默认垃圾收集器
  使Garbage-First（G1）成为32位和64位服务器配置的默认垃圾收集器（GC）。对于大多数用户而言，使用低暂停收集器（例如G1）比面向吞吐量的收集器（例如以前默认的Parallel GC）提供了更好的整体体验。
  
- 弃用并发标记扫描（CMS）垃圾收集器
  弃用Concurrent Mark Sweep（CMS）垃圾收集器。使用该-XX:+UseConcMarkSweepGC选项在命令行上请求时会发出警告消息。Garbage-First（G1）垃圾收集器旨在替代CMS的大多数用途。
  
java9新增功能<a href="https://docs.oracle.com/javase/9/whatsnew/toc.htm">官方文档</a>