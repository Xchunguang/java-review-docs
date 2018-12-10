# java6新特性

![java6](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java6.png)

## 集合框架功能增强

API的主要主题变化是更好的双向收集访问。

#### 提供了新的集合接口：
- Deque - 双端队列
- BlockingDeque - 阻塞双端队列
- NavigableSet - 使用导航方法扩展的SortedSet
- NavigableMap - 使用导航方法扩展的SortedMap
- ConcurrentnavigableMap 
  
#### 添加了具体实现类：
- ArrayDeque - Deque接口的高效可调整大小的数组实现
- ConcurrentSkipListSet - NavigableSet接口的并发可伸缩跳过列表实现 。
- ConcurrentSkipListMap - ConcurrentNavigableMap接口的并发可伸缩跳过列表实现 。
- LinkedBlockingDeque - 由链接节点支持的并发可扩展（可选）有界FIFO阻塞deque。
- AbstractMap.SimpleEntry - Map.Entry的简单可变实现
- AbstractMap.SimpleImmutableEntry - Map.Entry的简单不可变实现
  
#### 修改原有的类实现新的接口：
- LinkedList - 改进以实现 Deque接口。
- TreeSet - 改进以实现 NavigableSet接口。
- TreeMap - 改进以实现 NavigableMap接口。
  
#### Collections工具类添加了两个新的方法：
- newSetFromMap(Map)

  从通用的Map实现创建通用的Set实现
  
      Set<Object> identityHashSet = Collections.newSetFromMap(new IdentityHashMap<Object, Boolean>());
      
- asLifoQueue(Deque)

  返回Deque的一个试图作为一个先进先出的队列
  
#### 新的数组拷贝的方法：
  
    //改动之前的数组复制：
    int[] newArray = new int[newLength];
    System.arraycopy(oldArray, 0, newArray, 0, oldArray.length);
    
    //改动之后数组复制：
    int[] newArray = Arrays.copyOf(a, newLength);
      
## java I/O的增强功能
#### 新增一个类java.io.Console
该类包含了访问基于字符的控制台设备的方法。开发用于Window的cmd程序或linux下的Terminal程序时可以使用该类。
- readPassword()
  该方法禁用输入回显因此适合检索密码等敏感数据。
      
      Console cons ;
      char[] passwd;
      if ((cons = System.console()) != null &&
          (passwd = cons.readPassword("[%s]", "Password:")) != null) {
        System.out.println(passwd);
      }
      
  注意，该示例在eclipse中运行System.console()的结果是null，因为虚拟机的特性决定是否具有控制台取决于底层平台和虚拟机的方式。如果虚拟机从一个交互式命令行开始启动，且没有重定向标准输入和输出流，那么其控制台将存在，通常连接到键盘并从虚拟机启动的地方显示。如果虚拟机是自启动的（例如，有后台作业调度程序启动），那么他通常没有控制台。所以测试该方法可以使用cmd编译运行即可。
  
- System.console()

  该方法即是获得与当前JVM相关联的唯一的控制台
  
#### File类添加新方法：
- 检索磁盘使用信息的方法：
  - long getTotalSpace()

    以字节为单位返回分区的大小
    
  - long getFreeSpace()

    返回分区中未分配的字节数
    
  - long getUsableSpace()

    返回分区上可用的字节数，包括检查写权限和其他操作系统限制
    
- 设置或查询文件权限的方法
  - boolean setWritable(boolean writable, boolean ownerOnly)和 boolean setWritable(boolean writable)
  
    设置所有者或每个人的写入权限
    
  - boolean setReadable(boolean readable, boolean ownerOnly)和 boolean setReadable(boolean readable)
  
    设置所有者或每个人的阅读权限
    
  - boolean setExecutable(boolean executable, boolean ownerOnly)和 boolean setExecutable(boolean executable)
  
    设置所有者或每个人的执行权限
    
  - boolean canExecute() 
  
    测试执行权限的值
    
#### IOException添加新的构造方法支持异常链接

     IOException(String, Throwable) IOException(Throwable)
     
## Jar和Zip增加功能
#### 添加了两个新的压缩流
- java.util.zip.DeflaterInputStream：压缩从此流读取的数据。
- java.util.zip.InflaterOutputStream：写入此流的数据已解压缩。

#### 在所有平台上，zip文件可以包含超过64k的条目。

#### 在Windows上，已删除一些限制：
- 现在支持超过256个字符的文件名。
- 已删除仅超过2,000个同时打开的zip文件的限制。

#### 对jar命令的更改
- 提取文件的时间戳是归档中列出的文件的时间戳，而不是提取的时间。
- 创建jar时，可以为捆绑在可执行jar文件中的独立应用程序指定入口点。'e'选项通过创建或覆盖jar文件清单中的Main-Class属性值来指定应用程序入口点。

## 新增网络功能
- 增强NetworkInterface
  - 提供许多用于访问与系统的网络适配器相关的状态和配置信息的新方法。这包括广播地址，子网掩码，MAC地址和MTU大小等信息。请参阅<a href="https://docs.oracle.com/javase/6/docs/api/java/net/NetworkInterface.html">java.net.NetworkInterface</a>
  - 新类<a href="https://docs.oracle.com/javase/6/docs/api/java/net/InterfaceAddress.html">java.net.InterfaceAddress</a> 封装了有关NetworkInterface的IP地址的所有信息，包括广播地址和子网前缀长度（子网掩码）

- 轻量级HTTP Server
  这为构建轻量级HTTP服务器应用程序提供了一组简单的类和接口。该功能定义了两个新包 com.sun.net.httpserver和com.sun.net.httpserver.spi。服务器支持HTTP和HTTPS。


## 反射的改变
java.lang.Class部分方法被泛化，如：getInterfaces()， getClasses()， getConstructors()等

    //before，java6之后会编译警告
    Class c = warn.getClass();
    //after
    Class<?> c = warn.getClass();
    
## 对脚本语言的支持：
许多脚本和动态类型语言的实现都会生成Java字节码，因此程序可以在Java平台上运行，就像实际的Java程序一样。以这种方式实现语言（或作为脚本语言的Java解释器类）提供了Java平台的所有优点 - 脚本实现可以利用Java平台的二进制可移植性，安全性和高性能字节码执行。
    
    public class BasicScripting {
        public void greet() throws ScriptException {
            ScriptEngineManager manager = new ScriptEngineManager();
            //支持通过名称、文件扩展名、MIMEtype查找
            ScriptEngine engine = manager.getEngineByName("JavaScript");
    //        engine = manager.getEngineByExtension("js");
    //        engine = manager.getEngineByMimeType("text/javascript");
            if (engine == null) {
                throw new RuntimeException("找不到JavaScript语言执行引擎。");
            }
            engine.eval("println('Hello!');");
        }
        public static void main(String[] args) {
            try {
                new BasicScripting().greet();
            } catch (ScriptException ex) {
                Logger.getLogger(BasicScripting.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
    }
    
## 支持JDBC4.0规范
## Java Compiler API

官方文档：<a href="https://www.oracle.com/technetwork/java/javase/features-141434.html">链接</a>