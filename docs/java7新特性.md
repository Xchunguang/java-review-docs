# java7������

![java7](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java7.png)

## java I/O��ǿ����
java7�ṩ��<a href="https://docs.oracle.com/javase/7/docs/api/java/nio/file/package-summary.html">java.nio.file</a>��������صİ�<a href="https://docs.oracle.com/javase/7/docs/api/java/nio/file/attribute/package-summary.html">java.nio.file.attribute</a>�����ṩ�˶Է����ļ�ϵͳȫ���֧�֡�ͬʱjava7Ҳ֧��zip���ļ�ϵͳ��

����Ϊʹ�����ṩ��io��д�ʹ����ļ�ʾ��������NIO2.0���ԣ�NIO��������ʽIO��<a href="https://docs.oracle.com/javase/tutorial/essential/io/file.html">�̳�</a>
- OpenOptions������Files���ж����������һ����ѡ�Ĳ���OpenOptions������ʾ���ļ���Ĭ����Ϊ��

  StandardOpenOptions֧������ö�٣�
  - WRITE - ���ļ��Խ���д���ʡ�
  - APPEND - �������ݸ��ӵ��ļ�ĩβ����ѡ����WRITE��CREATEѡ��һ��ʹ�á�
  - TRUNCATE_EXISTING - ���ļ��ض�Ϊ���ֽڡ���ѡ����WRITEѡ��һ��ʹ�á�
  - CREATE_NEW - ����ļ��Ѵ��ڣ��򴴽����ļ��������쳣��
  - CREATE - ���ļ���������ڣ��򴴽����ļ�����������ڣ���
  - DELETE_ON_CLOSE - �ر���ʱɾ���ļ�����ѡ�����ʱ�ļ������á�
  - SPARSE - ��ʾ�´������ļ�����ϡ��ġ�����߼�ѡ����ĳЩ�ļ�ϵͳ�Ϻ����ã�����NTFS�����о������ݡ���϶���Ĵ��ļ������Ը���Ч�ķ�ʽ�洢��������Щ�ռ�϶��ռ�ô��̿ռ䡣
  - SYNC - ʹ�ļ������ݺ�Ԫ���ݣ���ײ�洢�豸����ͬ����
  - DSYNC - ʹ�ļ�������ײ�洢�豸����ͬ����
  
- С�ļ��Ķ�д������
  
      Path file = FileSystems.getDefault().getPath("logs", "access.log");
      byte[] fileArray;
      fileArray = Files.readAllBytes(file);
      URI filePath = file.toUri();
      System.out.println("�ļ�·��:" + filePath);
      Path fileToWrite = FileSystems.getDefault().getPath("logs", "access-copy.log");
      Files.write(fileToWrite, fileArray,StandardOpenOption.CREATE, StandardOpenOption.APPEND);
      
- ͨ����������д�ļ���

      @Test
      public void readOrWriteFileByBuffer() throws IOException{
        Path file = FileSystems.getDefault().getPath("logs", "access.log");
        //д����
        Charset charset = Charset.forName("UTF-8");
        BufferedWriter writer = Files.newBufferedWriter(file, charset, StandardOpenOption.WRITE,StandardOpenOption.APPEND);
        String str = "д����";
        writer.write(str, 0, str.length());
        writer.flush();
        writer.close();
        //������
        BufferedReader reader = Files.newBufferedReader(file, charset);
          String line = null;
          while ((line = reader.readLine()) != null) {
              System.out.println(line);
          }
          reader.close();
      }

- ͨ������д�ļ���

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
      
- ʹ��Channel I/O��д�ļ���

      //��
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
  
      //д
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

- ������ͨ�ļ���

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
      
- ������ʱ�ļ���

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
    
## Java����������ӹ���
#### ����������
��java7�е��������ͣ�byte��short��int��long��Ҳ����ʹ�ö��������ֱ�ʾ��Ҫָ�����������֣���Ҫ������ǰ���`0b`��`0B`

    int a = 0B11110001;
    
#### �����»��ߣ������������»��߶����Գ������������͵ı������κ�λ�ã�������ߴ���Ŀɶ���

    int a = 0B1111_0001;
    
#### switch����ʹ���ַ�����

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

#### ͨ�õ�ʵ�������ƶ�: <>
ֻҪ���������Դ��������ƶ����Ͳ������Ϳ�����һ��յ����Ͳ��������滻���÷�����Ĺ��캯����������Ͳ�����Ҳ�з���ʽ�ĳ���Լ�����Ϊdiamond(��ʯ��)

    List<String> stringList = new ArrayList<>();
    
#### try-with-resource
ͨ����try������һ��������Դ��䣬����Դʹ����ɺ󣬱���ر���Դ����try-with-resourceȷ��ÿ����Դ��ʹ�ý���ʱ�Զ��Ĺرյ���Դ��ʵ��java7�����ӿ�`java.lang.AutoCloseable`�ӿڻ�`java.io.Closeable`�ӿڵ��κζ��󶼿���������Դ

�������ᵽ��ʹ��Channel I/Oд�ļ��ķ����Ľ�Ϊ����

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
  
#### ʹ�øĽ������ͼ�鲶�����쳣���ͺ������׳��쳣
����catch����Դ���������͵��쳣��

    //java7֮ǰ
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
���⣬�����ڰ汾��Java SE��ȣ����������Ը�׼ȷ�ط��������׳����쳣����ʹ��������throws�����������Ӿ���ָ����������쳣���͡�<a href="https://docs.oracle.com/javase/7/docs/technotes/guides/language/catch-multiple.html">�ٷ��ĵ�</a>

    //java7֮ǰ
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
    
## Java7��java�����<a href="https://docs.oracle.com/javase/7/docs/technotes/guides/vm/enhancements-7.html">��������</a>
- Java�����֧�ַ�Java���ԣ�Java SE 7������һ���µ�JVMָ��ɼ�JVM�϶�̬���ͱ�����Ե�ʵ�֡�
- ʹ��G1(Garbage-First Collector)�����ռ���ȡ��CMS(Concurrent Mark-Sweep Collector)
- ��ǿjava HotSpot���������

�ٷ��ĵ���<a href="https://www.oracle.com/technetwork/java/javase/jdk7-relnotes-418459.html#changes">����</a>