# java7新特性

![java7](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java7.png)

## java I/O增强功能
java7提供了<a href="https://docs.oracle.com/javase/7/docs/api/java/nio/file/package-summary.html">java.nio.file</a>包和它相关的包<a href="https://docs.oracle.com/javase/7/docs/api/java/nio/file/attribute/package-summary.html">java.nio.file.attribute</a>包。提供了对访问文件系统全面的支持。同时java7也支持zip的文件系统。

以下为使用新提供的io读写和创建文件示例，具有NIO2.0特性，NIO即非阻塞式IO。<a href="https://docs.oracle.com/javase/tutorial/essential/io/file.html">教程</a>
- OpenOptions参数，Files类中多个方法都有一个可选的参数OpenOptions，他表示打开文件的默认行为：

  StandardOpenOptions支持以下枚举：
  - WRITE - 打开文件以进行写访问。
  - APPEND - 将新数据附加到文件末尾。此选项与WRITE或CREATE选项一起使用。
  - TRUNCATE_EXISTING - 将文件截断为零字节。此选项与WRITE选项一起使用。
  - CREATE_NEW - 如果文件已存在，则创建新文件并引发异常。
  - CREATE - 打开文件（如果存在）或创建新文件（如果不存在）。
  - DELETE_ON_CLOSE - 关闭流时删除文件。此选项对临时文件很有用。
  - SPARSE - 提示新创建的文件将是稀疏的。这个高级选项在某些文件系统上很有用，例如NTFS，其中具有数据“间隙”的大文件可以以更有效的方式存储，其中这些空间隙不占用磁盘空间。
  - SYNC - 使文件（内容和元数据）与底层存储设备保持同步。
  - DSYNC - 使文件内容与底层存储设备保持同步。
  
- 小文件的读写方法：
  
      Path file = FileSystems.getDefault().getPath("logs", "access.log");
      byte[] fileArray;
      fileArray = Files.readAllBytes(file);
      URI filePath = file.toUri();
      System.out.println("文件路径:" + filePath);
      Path fileToWrite = FileSystems.getDefault().getPath("logs", "access-copy.log");
      Files.write(fileToWrite, fileArray,StandardOpenOption.CREATE, StandardOpenOption.APPEND);
      
- 通过缓存流读写文件：

      @Test
      public void readOrWriteFileByBuffer() throws IOException{
        Path file = FileSystems.getDefault().getPath("logs", "access.log");
        //写操作
        Charset charset = Charset.forName("UTF-8");
        BufferedWriter writer = Files.newBufferedWriter(file, charset, StandardOpenOption.WRITE,StandardOpenOption.APPEND);
        String str = "写测试";
        writer.write(str, 0, str.length());
        writer.flush();
        writer.close();
        //读操作
        BufferedReader reader = Files.newBufferedReader(file, charset);
          String line = null;
          while ((line = reader.readLine()) != null) {
              System.out.println(line);
          }
          reader.close();
      }

- 通过流读写文件：

      @Test
      public void readOrWriteFileByStream() throws IOException{
        Path file = FileSystems.getDefault().getPath("logs", "access.log");

        String s = "Hello World! ";
          byte data[] = s.getBytes();

          OutputStream out = new BufferedOutputStream(Files.newOutputStream(file, StandardOpenOption.CREATE, StandardOpenOption.APPEND));
        out.write(data, 0, data.length);
        out.close();
        
        InputStream in = Files.newInputStream(file);
        BufferedReader reader = new BufferedReader(new InputStreamReader(in));
        String line = null;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
        reader.close();
        
      }
      
- 使用Channel I/O读写文件：

      //读
      @Test
      public void readChannel() throws IOException{
        Path file = FileSystems.getDefault().getPath("logs", "access.log");
        SeekableByteChannel sbc = Files.newByteChannel(file);
          ByteBuffer buf = ByteBuffer.allocate(10);

          String encoding = System.getProperty("file.encoding");
          while (sbc.read(buf) > 0) {
              buf.rewind();
              System.out.print(Charset.forName(encoding).decode(buf));
              buf.flip();
          }
          sbc.close();
      }
  
      //写
      @Test
      public void writeChannel() throws IOException{
          Set<OpenOption> options = new HashSet<OpenOption>();
          options.add(StandardOpenOption.APPEND);
          options.add(StandardOpenOption.CREATE);
          String s = "Hello World! ";
          byte data[] = s.getBytes();
          ByteBuffer bb = ByteBuffer.wrap(data);
          
          Path file = FileSystems.getDefault().getPath("logs", "access.log");

          SeekableByteChannel sbc = Files.newByteChannel(file, options);
          sbc.write(bb);
          sbc.close();
      }

- 创建普通文件：

      @Test
      public void createFile(){
        Path file = FileSystems.getDefault().getPath("logs", "access.log");
        try {
            // Create the empty file with default permissions, etc.
            Files.createFile(file);
        } catch (FileAlreadyExistsException x) {
            System.err.format("file named %s" +
                " already exists%n", file);
        } catch (IOException x) {
            // Some other sort of failure, such as permissions.
            System.err.format("createFile error: %s%n", x);
        }
      }
      
- 创建临时文件：

      @Test
      public void createTempFile(){
        try {
            Path tempFile = Files.createTempFile(null, ".myapp");
            System.out.format("The temporary file" +
                " has been created: %s%n", tempFile);
        } catch (IOException x) {
            System.err.format("IOException: %s%n", x);
        }
      }
    
## Java编程语言增加功能
#### 二进制数字
在java7中的整数类型：byte、short、int和long，也可以使用二进制数字表示，要指定二进制数字，需要在数字前添加`0b`或`0B`

    int a = 0B11110001;
    
#### 数字下划线，任意数量的下划线都可以出现在数字类型的变量的任何位置，可以提高代码的可读性

    int a = 0B1111_0001;
    
#### switch可以使用字符串：

    String str = "hello";
    switch (str) {  
       case "hello":  
           System.out.println("you choose hello"); 
           break;  
       case "world":  
           System.out.println("you choose world");  
           break;  
       default:  
           System.out.println("you choose other");  
    }  

#### 通用的实例类型推断: <>
只要编译器可以从上下文推断类型参数，就可以用一组空的类型参数（）替换调用泛型类的构造函数所需的类型参数。也有非正式的称这对尖括号为diamond(钻石？)

    List<String> stringList = new ArrayList<>();
    
#### try-with-resource
通过在try中声明一个或多个资源语句，当资源使用完成后，必须关闭资源，而try-with-resource确保每个资源在使用结束时自动的关闭掉资源。实现java7新增接口`java.lang.AutoCloseable`接口或`java.io.Closeable`接口的任何对象都可以用作资源

以上面提到的使用Channel I/O写文件的方法改进为例：

    @Test
    public void writeChannel() throws IOException{
        Set<OpenOption> options = new HashSet<OpenOption>();
        options.add(StandardOpenOption.APPEND);
        options.add(StandardOpenOption.CREATE);
        String s = "Hello World! ";
        byte data[] = s.getBytes();
        ByteBuffer bb = ByteBuffer.wrap(data);
        
        Path file = FileSystems.getDefault().getPath("logs", "access.log");

        try(SeekableByteChannel sbc = Files.newByteChannel(file, options)){
          sbc.write(bb);
        }catch(IOException e){
          e.printStackTrace();
        }
    }
  
#### 使用改进的类型检查捕获多个异常类型和重新抛出异常
单个catch块可以处理多种类型的异常。

    //java7之前
    catch (IOException ex) {
         logger.log(ex);
         throw ex;
    catch (SQLException ex) {
         logger.log(ex);
         throw ex;
    }

    //java7
    catch (IOException|SQLException ex) {
        logger.log(ex);
        throw ex;
    }
此外，与早期版本的Java SE相比，编译器可以更准确地分析重新抛出的异常。这使您可以在throws方法声明的子句中指定更具体的异常类型。<a href="https://docs.oracle.com/javase/7/docs/technotes/guides/language/catch-multiple.html">官方文档</a>

    //java7之前
    static class FirstException extends Exception { }
    static class SecondException extends Exception { }

    public void rethrowException(String exceptionName) throws Exception {
      try {
        if (exceptionName.equals("First")) {
          throw new FirstException();
        } else {
          throw new SecondException();
        }
      } catch (Exception e) {
        throw e;
      }
    }
    
    //java7
    public void rethrowException(String exceptionName)
    throws FirstException, SecondException {
      try {
        // ...
      }
      catch (Exception e) {
        throw e;
      }
    }
    
## Java7中java虚拟机<a href="https://docs.oracle.com/javase/7/docs/technotes/guides/vm/enhancements-7.html">新增功能</a>
- Java虚拟机支持非Java语言，Java SE 7引入了一种新的JVM指令，可简化JVM上动态类型编程语言的实现。
- 使用G1(Garbage-First Collector)垃圾收集器取代CMS(Concurrent Mark-Sweep Collector)
- 增强java HotSpot虚拟机性能

官方文档：<a href="https://www.oracle.com/technetwork/java/javase/jdk7-relnotes-418459.html#changes">链接</a>