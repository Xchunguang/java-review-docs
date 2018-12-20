# java11新特性
java11是一个长期支持的LTS版本，同时带来了很多特性，java11的推出刚好赶上java8免费更新的最后时间，可以预见java11将会在不久登上各公司的生产环境。
## 新增功能

官方提供了重要的新增功能如下：

![java11](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java11.png)

## 重要的变化和信息

- Applet和Web start应用程序所需要的部署堆栈，在java9中已经被弃用，在java11正式删除
- 如果没有部署堆栈，则已从JDK 11支持的配置列表中删除了支持的浏览器的整个部分。
- 在windows和macOS上的JRE的自动更新将不再可用
- 在windows和maxOS系统中安装之前版本的JDK时可选是否安装一个JRE，在java11中不在可选。
- 当前版本不再提供JRE，只提供JDK。可以使用jlink工具创建一个更小的自定义的运行时环境。
- javafx不再包含在jdk中，现在从openjfx.io可以单独下载。
- 装在JDK 7/8/9/10的Java Mission Control,不再包含在Oracle JDK。现在是一个独立的下载。
- 以前的版本被译成英语、日语、简体中文、法语、德语、意大利语、韩语、葡萄牙语(巴西),西班牙,瑞典。然而,在JDK 11之后,法国、德国、意大利、韩国、葡萄牙语(巴西),西班牙语和瑞典语翻译不再提供
- 更新window的打包格式从`tar.gz`改到`zip`格式。
- 更新macOS的打包格式从`.app`到`.dmg`

## Unicode 10 

升级现有平台api支持Unicode标准的10.0版本,自从支持Unicode 8.0.0的JDK 10发布以来，JDK 11结合了Unicode 9.0.0和10.0.0版本

## HTTP Client (Standard) 

HTTP客户端已在Java 11中标准化。作为此工作的一部分，位于jdk.incubator.http包中的先前孵化的API 已被删除。改为java.net.http模块。

使用java自带的http client基本可以替代apache的HttpClient工具包
    
    var request = HttpRequest.newBuilder()
            .uri(URI.create("https://github.com"))
            .GET()
            .build();
    var client = HttpClient.newHttpClient();
    // 同步
    HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
    System.out.println(response.body());
    // 异步
    client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
            .thenApply(HttpResponse::body)
            .thenAccept(System.out::println);

## 新的 Collection.toArray(IntFunction)

在java.util.Collection接口中添加了一个新的默认方法toArray（IntFunction）。此方法允许将集合的元素传输到新创建的所需运行时类型的数组。

    Set<String> items = Set.of("item1", "item2", "item3");
    //JDK11之前
    items.toArray(new String[items.size()]);
    //JDK11之后
    items.toArray(size -> new String[size]);
    items.toArray(String[]::new);

## ZGC：一个可伸缩的低延迟动作垃圾收集器(实验)

ZGC作为实验性功能包含在内。要启用它，因此需要将-XX：+ UnlockExperimentalVMOptions选项与-XX：+ UseZGC选项结合使用。

ZGC的核心是并发垃圾收集器，这意味着在Java线程继续执行的同时完成所有繁重的工作（标记，压缩，引用处理，字符串表清理等）。这极大地限制了垃圾收集对应用程序响应时间的负面影响。

#### ZGC的设计目标
- 暂停时间不超过10 ms
- 暂停时间不增加堆或live-set大小
- 处理大小从几百兆到几千兆字节不等的堆

#### ZGC的这个实验版具有以下限制
- 只在Linux/x64系统中可用
- 不支持使用压缩的oops和/或压缩的类点。默认情况下禁用XX:+UseCompressedOops和-XX:+UseCompressedClassPointers选项。启用它们将不起作用。
- 不支持类卸载。默认情况下禁用-XX:+ClassUnloading和 -XX:+ClassUnloadingWithConcurrentMark选项。启用它们将不起作用。
- 不支持将ZGC与Graal结合使用。

## Epsilon GC 无操作的垃圾收集器

Epsilon GC是新的实验性无操作垃圾收集器。Epsilon GC仅处理内存分配，并且不实现任何内存回收机制。它对性能测试非常有用，可以与其他GC的成本/收益进行对比。它可用于在测试中方便地断言内存占用和内存压力。在极端情况下，它可能对非常短暂的作业很有用，其中内存回收将在JVM终止时发生，或者在低垃圾应用程序中获得最后一次延迟改进。

官方文档：<a href="https://www.oracle.com/technetwork/java/javase/11-relnote-issues-5012449.html#NewFeature">链接</a>

