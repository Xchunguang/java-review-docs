# java6������

![java6](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java6.png)

## ���Ͽ�ܹ�����ǿ

API����Ҫ����仯�Ǹ��õ�˫���ռ����ʡ�

#### �ṩ���µļ��Ͻӿڣ�
- Deque - ˫�˶���
- BlockingDeque - ����˫�˶���
- NavigableSet - ʹ�õ���������չ��SortedSet
- NavigableMap - ʹ�õ���������չ��SortedMap
- ConcurrentnavigableMap 
  
#### ����˾���ʵ���ࣺ
- ArrayDeque - Deque�ӿڵĸ�Ч�ɵ�����С������ʵ��
- ConcurrentSkipListSet - NavigableSet�ӿڵĲ��������������б�ʵ�� ��
- ConcurrentSkipListMap - ConcurrentNavigableMap�ӿڵĲ��������������б�ʵ�� ��
- LinkedBlockingDeque - �����ӽڵ�֧�ֵĲ�������չ����ѡ���н�FIFO����deque��
- AbstractMap.SimpleEntry - Map.Entry�ļ򵥿ɱ�ʵ��
- AbstractMap.SimpleImmutableEntry - Map.Entry�ļ򵥲��ɱ�ʵ��
  
#### �޸�ԭ�е���ʵ���µĽӿڣ�
- LinkedList - �Ľ���ʵ�� Deque�ӿڡ�
- TreeSet - �Ľ���ʵ�� NavigableSet�ӿڡ�
- TreeMap - �Ľ���ʵ�� NavigableMap�ӿڡ�
  
#### Collections����������������µķ�����
- newSetFromMap(Map)

  ��ͨ�õ�Mapʵ�ִ���ͨ�õ�Setʵ��
  
      Set<Object> identityHashSet = Collections.newSetFromMap(new IdentityHashMap<Object, Boolean>());
      
- asLifoQueue(Deque)

  ����Deque��һ����ͼ��Ϊһ���Ƚ��ȳ��Ķ���
  
#### �µ����鿽���ķ�����
  
    //�Ķ�֮ǰ�����鸴�ƣ�
    int[] newArray = new int[newLength];
    System.arraycopy(oldArray, 0, newArray, 0, oldArray.length);
    
    //�Ķ�֮�����鸴�ƣ�
    int[] newArray = Arrays.copyOf(a, newLength);
      
## java I/O����ǿ����
#### ����һ����java.io.Console
��������˷��ʻ����ַ��Ŀ���̨�豸�ķ�������������Window��cmd�����linux�µ�Terminal����ʱ����ʹ�ø��ࡣ
- readPassword()
  �÷������������������ʺϼ���������������ݡ�
      
      Console cons ;
      char[] passwd;
      if ((cons = System.console()) != null &&
          (passwd = cons.readPassword("[%s]", "Password:")) != null) {
        System.out.println(passwd);
      }
      
  ע�⣬��ʾ����eclipse������System.console()�Ľ����null����Ϊ����������Ծ����Ƿ���п���̨ȡ���ڵײ�ƽ̨��������ķ�ʽ������������һ������ʽ�����п�ʼ��������û���ض����׼��������������ô�����̨�����ڣ�ͨ�����ӵ����̲�������������ĵط���ʾ�������������������ģ����磬�к�̨��ҵ���ȳ�������������ô��ͨ��û�п���̨�����Բ��Ը÷�������ʹ��cmd�������м��ɡ�
  
- System.console()

  �÷������ǻ���뵱ǰJVM�������Ψһ�Ŀ���̨
  
#### File������·�����
- ��������ʹ����Ϣ�ķ�����
  - long getTotalSpace()

    ���ֽ�Ϊ��λ���ط����Ĵ�С
    
  - long getFreeSpace()

    ���ط�����δ������ֽ���
    
  - long getUsableSpace()

    ���ط����Ͽ��õ��ֽ������������дȨ�޺���������ϵͳ����
    
- ���û��ѯ�ļ�Ȩ�޵ķ���
  - boolean setWritable(boolean writable, boolean ownerOnly)�� boolean setWritable(boolean writable)
  
    ���������߻�ÿ���˵�д��Ȩ��
    
  - boolean setReadable(boolean readable, boolean ownerOnly)�� boolean setReadable(boolean readable)
  
    ���������߻�ÿ���˵��Ķ�Ȩ��
    
  - boolean setExecutable(boolean executable, boolean ownerOnly)�� boolean setExecutable(boolean executable)
  
    ���������߻�ÿ���˵�ִ��Ȩ��
    
  - boolean canExecute() 
  
    ����ִ��Ȩ�޵�ֵ
    
#### IOException����µĹ��췽��֧���쳣����

     IOException(String, Throwable) IOException(Throwable)
     
## Jar��Zip���ӹ���
#### ����������µ�ѹ����
- java.util.zip.DeflaterInputStream��ѹ���Ӵ�����ȡ�����ݡ�
- java.util.zip.InflaterOutputStream��д������������ѽ�ѹ����

#### ������ƽ̨�ϣ�zip�ļ����԰�������64k����Ŀ��

#### ��Windows�ϣ���ɾ��һЩ���ƣ�
- ����֧�ֳ���256���ַ����ļ�����
- ��ɾ��������2,000��ͬʱ�򿪵�zip�ļ������ơ�

#### ��jar����ĸ���
- ��ȡ�ļ���ʱ����ǹ鵵���г����ļ���ʱ�������������ȡ��ʱ�䡣
- ����jarʱ������Ϊ�����ڿ�ִ��jar�ļ��еĶ���Ӧ�ó���ָ����ڵ㡣'e'ѡ��ͨ�������򸲸�jar�ļ��嵥�е�Main-Class����ֵ��ָ��Ӧ�ó�����ڵ㡣

## �������繦��
- ��ǿNetworkInterface
  - �ṩ������ڷ�����ϵͳ��������������ص�״̬��������Ϣ���·�����������㲥��ַ���������룬MAC��ַ��MTU��С����Ϣ�������<a href="https://docs.oracle.com/javase/6/docs/api/java/net/NetworkInterface.html">java.net.NetworkInterface</a>
  - ����<a href="https://docs.oracle.com/javase/6/docs/api/java/net/InterfaceAddress.html">java.net.InterfaceAddress</a> ��װ���й�NetworkInterface��IP��ַ��������Ϣ�������㲥��ַ������ǰ׺���ȣ��������룩

- ������HTTP Server
  ��Ϊ����������HTTP������Ӧ�ó����ṩ��һ��򵥵���ͽӿڡ��ù��ܶ����������°� com.sun.net.httpserver��com.sun.net.httpserver.spi��������֧��HTTP��HTTPS��


## ����ĸı�
java.lang.Class���ַ������������磺getInterfaces()�� getClasses()�� getConstructors()��

    //before��java6֮�����뾯��
    Class c = warn.getClass();
    //after
    Class<?> c = warn.getClass();
    
## �Խű����Ե�֧�֣�
���ű��Ͷ�̬�������Ե�ʵ�ֶ�������Java�ֽ��룬��˳��������Javaƽ̨�����У�����ʵ�ʵ�Java����һ���������ַ�ʽʵ�����ԣ�����Ϊ�ű����Ե�Java�������ࣩ�ṩ��Javaƽ̨�������ŵ� - �ű�ʵ�ֿ�������Javaƽ̨�Ķ����ƿ���ֲ�ԣ���ȫ�Ժ͸������ֽ���ִ�С�
    
    public class BasicScripting {
        public void greet() throws ScriptException {
            ScriptEngineManager manager = new ScriptEngineManager();
            //֧��ͨ�����ơ��ļ���չ����MIMEtype����
            ScriptEngine engine = manager.getEngineByName("JavaScript");
    //        engine = manager.getEngineByExtension("js");
    //        engine = manager.getEngineByMimeType("text/javascript");
            if (engine == null) {
                throw new RuntimeException("�Ҳ���JavaScript����ִ�����档");
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
    
## ֧��JDBC4.0�淶
## Java Compiler API

�ٷ��ĵ���<a href="https://www.oracle.com/technetwork/java/javase/features-141434.html">����</a>