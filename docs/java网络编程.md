# Java������
java�ṩ�������̵İ�java.net��֧�����ֳ���������Э���֧�֣�TCP��UDP��

## socket�׽���
socketʹ��TCPЭ���ṩ����̨�����֮���ͨѶ���ơ��ͻ��˴����׽��ֲ��������ӷ���˵��׽��֡�

�����Ӵ����󣬷���˻ᴴ��һ��Socket���󣬿ͻ��˺ͷ���˿���ͨ����Socket�����д��Ͷ�ȡ����ͨ�š�

java.net.Socket �����һ���׽��֣����� java.net.ServerSocket ��Ϊ�����������ṩ��һ���������ͻ��ˣ��������ǽ������ӵĻ���

��̨�����֮��ʹ���׽��ֽ���TCP���ӹ��̣�
- �����ʵ����һ��ServerSocket���󣬱�ʾͨ���������ϵĶ˿�ͨѶ��
- ����˵���ServerSocket���accpet() �������÷�����һֱ�ȴ�ֱ���пͻ������ӡ�
- �ͻ���ʵ����һ��Socket�Ķ���ָ������˵ĵ�ַ�Ͷ˿���Ϣ�������ӵ�����ˡ�
- Socket ��Ĺ��캯����ͼ���ͻ������ӵ�ָ���ķ������Ͷ˿ںš����ͨ�ű����������ڿͻ��˴���һ�� Socket �����ܹ������������ͨ�š�
- �ڷ������ˣ�accept() �������ط�������һ���µ� socket ���ã��� socket ���ӵ��ͻ��˵� socket��
- ���ӽ�����ͨ��ʹ�� I/O ���ڽ���ͨ�ţ�ÿһ��socket����һ���������һ�����������ͻ��˵���������ӵ��������˵������������ͻ��˵����������ӵ��������˵��������

## TCPʾ��

    //�����
    public class ServerTest
    {
      public static void main(String[] args) throws Exception
      {
        ServerSocket server=new ServerSocket(8888);
        System.out.println("�ȴ�����");
        Socket s= server.accept();//������������
        System.out.println("���ӳɹ�");
        BufferedReader br=new BufferedReader(new InputStreamReader(s.getInputStream()));
        String str=br.readLine();
        System.out.println(""+str);
        s.close();
        server.close();
      }
    }
    
    //�ͻ���
    public class Clink
    {
      public static void main(String[] args) throws Exception
      { 
            InetAddress a=InetAddress.getLocalHost();
            Socket b=new Socket(a,8888);
            System.out.println("��������");
            PrintWriter pw=new PrintWriter(b.getOutputStream());
            pw.println("�ҷ���������");
            pw.flush();
            b.close();    
      }
    }
    
## UDPʾ��

    //����
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
    
    //����
    public class GetUDP
    {
      public static void main(String[] args) throws ClassNotFoundException
      {
        byte by[]=new byte[1024*1024];
        DatagramSocket ds;
        try {
          System.out.println("�ȴ�����");
          ds = new DatagramSocket(10004);
          DatagramPacket dp=new DatagramPacket(by, 1024*1024);
          ds.receive(dp);
          ByteArrayInputStream bs=new ByteArrayInputStream(dp.getData());
          ObjectInputStream os;
          os = new ObjectInputStream(bs);
          Person p=(Person)os.readObject();
          System.out.println("���ܵ����û��ǣ�"+p.toString());
        } catch (IOException e) {
          e.printStackTrace();
        }
      }
    }




    