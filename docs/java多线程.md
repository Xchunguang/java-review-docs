# java���߳�

java�ṩ�˶��߳��ڲ�֧�ֵ�ʵ�֣�һ�����̿��Բ���ִ�ж���̣߳�ÿ���߳̿ɵ���ִ������

#### �������̵߳�����
- ������һ�����ж������ܵĳ������ĳ�����ݼ��ϵ�һ�����л�������������ӵ��ϵͳ��Դ����һ����̬�ĸ����һ�����ʵ�塣����ֻ�ǳ���Ĵ��룬��������ǰ�Ļ��ͨ�������������ֵ�ʹ���Ĵ�������������ʾ��
- ������һ��ִ���еĳ��򣬳�����һ��û��������ʵ�壬ֻ�д����������������ʱ�������ܳ�Ϊһ�����ʵ�壬��Ϊ����
- ͨ����һ�������а������ɸ��̣߳����ǿ������ý�����ӵ�е���Դ���������̵߳Ĳ���ϵͳ�У�ͨ���ѽ��̵���������Դ�Ļ�����λ�������߳���Ϊ�������кͶ������ȵĻ�����λ�������̱߳Ƚ���С�������ϲ�����ϵͳ��Դ���ʶ��̵߳����������Ŀ�����ͺ�С���ܸ�Ч�����ϵͳ�ڶ�������Ĳ���ִ�еĳ̶ȡ�
- �ӽ��̺͸������в�ͬ�Ĵ�������ݿռ䣬������߳��������ݿռ䣬ÿ���߳����Լ���ִ�ж�ջ�ͳ��������Ϊ��ִ�������ģ����߳���Ҫ��Ϊ�˽�ԼCPUʱ�䣬�������ã����ݾ�������������̵߳���������Ҫʹ�ü�������ڴ���Դ��CPU
- ���ɣ�
  - ��ַ�ռ��������Դ�����̼��໥������ͬһ���̵ĸ��̼߳乲��ĳ�����ڵ��߳��������̲߳��ɼ�
  - ͨ�ţ����̼�ͨ��IPC���̼߳����ֱ�Ӷ�д�������ݶ�������ͨ�ţ���Ҫ����ͬ���ͻ����ֶεĸ������Ա�֤���ݵ�һ����
  - ���Ⱥ��л����߳������ĵ��л��Ƚ��������ĵ��л�Ҫ��ö�
  - �ڶ��̵߳�OS�У����̲���һ����ִ�е�ʵ��
- �����Ǿ���һ���������������ĳ������ĳ�����ݼ����ϵ�һ�����л��������ϵͳ������Դ����͵��ȵ�һ��������λ���߳��ǽ��̵�һ��ʵ�壬��CPU���Ⱥͷ��ɵĻ�����λ���߳��Լ������ϲ�ӵ����Դ��ֵӵ���������бز����ٵ���Դ��������������һ��Ĵ�����ջ����������ͬ��һ�����̵������̹߳��������ӵ�е�ȫ����Դ��


## �̵߳���������

#### �̵߳�״̬
�鿴javaԴ��ɼ�Thread.Stateö�����У�java�ṩ���̵߳�����״̬��ע����Щ�ṩ��״̬����java������е��߳�״̬��������ӳ�κβ���ϵͳ�е�״̬��������RUNNABLE״̬��java������Ὣ�̵߳��Ƚ�������ϵͳ������RUNNABLE״̬���߳���ready����running״̬�����ɲ���ϵͳ��ɣ�java�������û��ӳ�����״̬��

- NEW

  �½�״̬����ʾ��ǰ���̻߳�û�п�ʼִ��
  
- RUNNABLE

  ����״̬������ʾ�̵߳�ǰ�ǿ����еģ�Java���Ǵ������߳�ִ��start()�������ɸ�״̬����״̬��ʾ��ǰ�߳�����java�������ִ�У������ڵȴ�ĳЩ��Դ����CPU��һ����ǰ�̻߳����Դ�ͻ���ִ��״̬��ʵ���ϵ�ǰ״̬��������Running״̬
  
- BLOCKED

  ����״̬���̵߳ȴ�������һ��ͬ�����ͬ���������̻߳���������RUNNABLE״̬
  
- WAITING

  ���޵ȴ�״̬���̵߳������·�����������޵ȴ�״̬��
  - Object.wait()
  - Thread.join()
  - LockSupport.park()
  
  ���ڵȴ����̵߳ȴ������߳�����һЩ�ض��Ĳ��������磺
  - Object.wait()���̶߳��� -> ���� -> Object.notify() �� Object.notifyAll()
  - Object.join()���̶߳�����Ҫ�ȴ��ض����߳�ֹͣ
  
- TIMED_WAITING

  ��ʱ�ȴ������ǰ���ʱ��ĵȴ�״̬���߳̽��ȴ�ֱ���ﵽ��ָ����ʱ�䡣�߳�һ��������·������ɸ�״̬��
  - Thread.sleep(long millis);
  - Object.wait(long timeout);
  - Thread.join(long millis);
  - LockSupport.parkNanos
  - LockSupport.parkUntil
  
- TERMINATED

  ��ֹ״̬����ʾ�߳���ֹ

#### �߳�״̬ת��

![thread](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/thread.png)

## Java�����߳�

java�ṩ�����ִ����̵߳ķ�����
- �̳�Thread��
- ʵ��Runnable�ӿ�
- ʵ��Callable�ӿ�

#### �̳�Thread�ഴ���߳�
- ����Thread������࣬����д�����run��������run�����ķ�����ʹ������߳�Ҫ��ɵ�������˰�run()������Ϊִ���塣
- ����Thread�����ʵ�������������̶߳���
- �����̶߳����start()�������������̡߳�

    	package com.thread;  
    	  
    	public class FirstThreadTest extends Thread{  
    	    int i = 0;  
    	    //��дrun������run�����ķ���������߳�ִ����  
    	    public void run()  
    	    {  
    	        for(;i<100;i++){  
                System.out.println(getName()+"  "+i);  
    	        }  
    	    }  
    	    public static void main(String[] args)  
    	    {  
    	        for(int i = 0;i< 100;i++)  
    	        {  
    	            System.out.println(Thread.currentThread().getName()+"  : "+i);  
    	            if(i==20)  
    	            {  
    	                new FirstThreadTest().start();  
    	                new FirstThreadTest().start();  
    	            }  
    	        }  
    	    }  
    	}  

#### ͨ��Runnable�ӿڴ�����
- ����runnable�ӿڵ�ʵ���࣬����д�ýӿڵ�run()��������run()�����ķ�����ͬ���Ǹ��̵߳��߳�ִ���塣
- ���� Runnableʵ�����ʵ����������ʵ����ΪThread��target������Thread���󣬸�Thread��������������̶߳���
- �����̶߳����start()�������������̡߳�

      	package com.thread;  
      	  
      	public class RunnableThreadTest implements Runnable  
      	{  
      	  
      	    private int i;  
      	    public void run()  
      	    {  
      	        for(i = 0;i <100;i++)  
      	        {  
      	            System.out.println(Thread.currentThread().getName()+" "+i);  
      	        }  
      	    }  
      	    public static void main(String[] args)  
      	    {  
      	        for(int i = 0;i < 100;i++)  
      	        {  
      	            System.out.println(Thread.currentThread().getName()+" "+i);  
      	            if(i==20)  
      	            {  
      	                RunnableThreadTest rtt = new RunnableThreadTest();  
      	                new Thread(rtt,"���߳�1").start();  
      	                new Thread(rtt,"���߳�2").start();  
      	            }  
      	        }  
      	    }  	  
      	}  

#### ͨ��Callable��Future�����߳�
- ���� Callable �ӿڵ�ʵ���࣬��ʵ�� call() �������� call() ��������Ϊ�߳�ִ���壬�����з���ֵ��
- ���� Callable ʵ�����ʵ����ʹ�� FutureTask ������װ Callable ���󣬸� FutureTask �����װ�˸� Callable ����� call() �����ķ���ֵ��
- ʹ�� FutureTask ������Ϊ Thread ����� target �������������߳�
- ���� FutureTask ����� get() ������������߳�ִ�н�����ķ���ֵ��

      public class CallableThreadTest implements Callable<Integer> {
          public static void main(String[] args)  
          {  
              CallableThreadTest ctt = new CallableThreadTest();  
              FutureTask<Integer> ft = new FutureTask<>(ctt);  
              for(int i = 0;i < 100;i++)  
              {  
                  System.out.println(Thread.currentThread().getName()+" ��ѭ������i��ֵ"+i);  
                  if(i==20)  
                  {  
                      new Thread(ft,"�з���ֵ���߳�").start();  
                  }  
              }  
              try  
              {  
                  System.out.println("���̵߳ķ���ֵ��"+ft.get());  
              } catch (InterruptedException e)  
              {  
                  e.printStackTrace();  
              } catch (ExecutionException e)  
              {  
                  e.printStackTrace();  
              }  
        
          }
          @Override  
          public Integer call() throws Exception  
          {  
              int i = 0;  
              for(;i<100;i++)  
              {  
                  System.out.println(Thread.currentThread().getName()+" "+i);  
              }  
              return i;  
          }  
      }

## Java������
Java��������java�ṩ�Ķ��߳�ʵ�֣����Դ��򻯶��߳̿�����
java����������ת��<a href="https://github.com/Xchunguang/java-review-docs/blob/master/docs/java5%E6%96%B0%E7%89%B9%E6%80%A7.md#java%E5%B9%B6%E5%8F%91%E5%8C%85javautilconcurrent">����</a>