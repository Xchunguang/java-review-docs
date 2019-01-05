# Java网络编程
java提供了网络编程的包java.net，支持两种常见的网络协议的支持：TCP和UDP。

## socket套接字
socket使用TCP协议提供了两台计算机之间的通讯机制。客户端创建套接字并尝试连接服务端的套接字。

当连接创建后，服务端会创建一个Socket对象，客户端和服务端可以通过对Socket对象的写入和读取进行通信。

java.net.Socket 类代表一个套接字，并且 java.net.ServerSocket 类为服务器程序提供了一种来监听客户端，并与他们建立连接的机制

两台计算机之间使用套接字进行TCP连接过程：
- 服务端实例化一个ServerSocket对象，表示通过服务器上的端口通讯。
- 服务端调用ServerSocket类的accpet() 方法，该方法会一直等待直到有客户端连接。
- 客户端实例化一个Socket的对象，指定服务端的地址和端口信息请求连接到服务端。
- Socket 类的构造函数试图将客户端连接到指定的服务器和端口号。如果通信被建立，则在客户端创建一个 Socket 对象能够与服务器进行通信。
- 在服务器端，accept() 方法返回服务器上一个新的 socket 引用，该 socket 连接到客户端的 socket。
- 连接建立后，通过使用 I/O 流在进行通信，每一个socket都有一个输出流和一个输入流，客户端的输出流连接到服务器端的输入流，而客户端的输入流连接到服务器端的输出流。

## TCP示例

    //服务端
    public class ServerTest
    {
      public static void main(String[] args) throws Exception
      {
        ServerSocket server=new ServerSocket(8888);
        System.out.println("等待链接");
        Socket s= server.accept();//阻塞接受请求
        System.out.println("链接成功");
        BufferedReader br=new BufferedReader(new InputStreamReader(s.getInputStream()));
        String str=br.readLine();
        System.out.println(""+str);
        s.close();
        server.close();
      }
    }
    
    //客户端
    public class Clink
    {
      public static void main(String[] args) throws Exception
      { 
            InetAddress a=InetAddress.getLocalHost();
            Socket b=new Socket(a,8888);
            System.out.println("请求链接");
            PrintWriter pw=new PrintWriter(b.getOutputStream());
            pw.println("我发送了数据");
            pw.flush();
            b.close();    
      }
    }
    
## UDP示例

    //发送
    public class SendObj
    {
      public static void main(String[] args) throws IOException
      {
        DatagramSocket ds;
        try{
          ds = new DatagramSocket();
          Person p=new Person("xcg",21);
               byte b[]=new byte[1024*1024];
               ByteArrayOutputStream bs=new ByteArrayOutputStream();
               ObjectOutputStream bo=new ObjectOutputStream(bs);
               bo.writeObject(p);
               b=bs.toByteArray();
               DatagramPacket dp=new DatagramPacket(b,b.length, InetAddress.getLocalHost(), 10004);
               ds.send(dp);
               ds.close();
        } catch (SocketException e) {
          e.printStackTrace();
        }
      }
    }
    
    //接收
    public class GetUDP
    {
      public static void main(String[] args) throws ClassNotFoundException
      {
        byte by[]=new byte[1024*1024];
        DatagramSocket ds;
        try {
          System.out.println("等待接受");
          ds = new DatagramSocket(10004);
          DatagramPacket dp=new DatagramPacket(by, 1024*1024);
          ds.receive(dp);
          ByteArrayInputStream bs=new ByteArrayInputStream(dp.getData());
          ObjectInputStream os;
          os = new ObjectInputStream(bs);
          Person p=(Person)os.readObject();
          System.out.println("接受到的用户是："+p.toString());
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }




    