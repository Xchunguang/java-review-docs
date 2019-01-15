# java多线程

java提供了多线程内部支持的实现，一个进程可以并发执行多个线程，每个线程可单独执行任务。

#### 进程与线程的区别
- 进程是一个具有独立功能的程序关于某个数据集合的一次运行活动。他可以申请和拥有系统资源，是一个动态的概念，是一个活动的实体。它不只是程序的代码，还包括当前的活动，通过程序计数器的值和处理寄存器的内容来表示。
- 进程是一个执行中的程序，程序是一个没有生命的实体，只有处理器赋予程序声明时，他才能称为一个活动的实体，称为进程
- 通常在一个进程中包含若干个线程，他们可以利用进程所拥有的资源。在引入线程的操作系统中，通常把进程当作分配资源的基本单位，而把线程作为独立运行和独立调度的基本单位，由于线程比进程小，基本上不用有系统资源，故对线程掉地锁付出的开销会就很小，能高效的提高系统内多个程序间的并发执行的程度。
- 子进程和父进程有不同的代码和数据空间，而多个线程则共享数据空间，每个线程有自己的执行堆栈和程序计数器为其执行上下文，多线程主要是为了节约CPU时间，发挥利用，根据具体情况而定，线程的运行中需要使用计算机的内存资源和CPU
- 归纳：
  - 地址空间和其他资源：进程间相互独立，同一进程的各线程间共享，某进程内的线程在其他线程不可见
  - 通信：进程间通信IPC，线程间可以直接读写进程数据段来进行通信，需要进程同步和互斥手段的辅助，以保证数据的一致性
  - 调度和切换：线程上下文的切换比进程上下文的切换要快得多
  - 在多线程的OS中，进程不是一个可执行的实体
- 进程是具有一定独立运行能力的程序关于某个数据集合上的一次运行活动，进程是系统进行资源分配和调度的一个独立单位，线程是进程的一个实体，是CPU调度和分派的基本单位。线程自己基本上不拥有资源，值拥有在运行中必不可少的资源，如程序计数器，一组寄存器和栈，但他可与同属一个进程的其他线程共享进程所拥有的全部资源。


## 线程的生命周期

#### 线程的状态
查看java源码可见Thread.State枚举类中，java提供了线程的六种状态。注意这些提供的状态都是java虚拟机中的线程状态，并不反映任何操作系统中的状态。例如在RUNNABLE状态，java虚拟机会将线程调度交给操作系统，即在RUNNABLE状态的线程是ready还是running状态都是由操作系统完成，java虚拟机并没有映射调度状态。

- NEW

  新建状态，表示当前的线程还没有开始执行
  
- RUNNABLE

  就绪状态，即表示线程当前是可运行的，Java中是创建的线程执行start()方法会变成该状态。该状态表示当前线程是在java虚拟机里执行，但还在等待某些资源例如CPU，一旦当前线程获得资源就会变成执行状态。实际上当前状态即包含了Running状态
  
- BLOCKED

  阻塞状态，线程等待锁进入一个同步块或同步方法，线程获得锁后会变成RUNNABLE状态
  
- WAITING

  无限等待状态，线程调用以下方法会进入无限等待状态：
  - Object.wait()
  - Thread.join()
  - LockSupport.park()
  
  处于等待的线程等待其他线程做出一些特定的操作。例如：
  - Object.wait()的线程对象 -> 调用 -> Object.notify() 或 Object.notifyAll()
  - Object.join()的线程对象需要等待特定的线程停止
  
- TIMED_WAITING

  超时等待，即是包含时间的等待状态，线程将等待直到达到了指定的时间。线程一般调用以下方法会变成该状态：
  - Thread.sleep(long millis);
  - Object.wait(long timeout);
  - Thread.join(long millis);
  - LockSupport.parkNanos
  - LockSupport.parkUntil
  
- TERMINATED

  终止状态，表示线程终止

#### 线程状态转换

![thread](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/thread.png)

## Java创建线程

java提供了三种创建线程的方法：
- 继承Thread类
- 实现Runnable接口
- 实现Callable接口

#### 继承Thread类创建线程
- 定义Thread类的子类，并重写该类的run方法，该run方法的方法体就代表了线程要完成的任务。因此把run()方法称为执行体。
- 创建Thread子类的实例，即创建了线程对象。
- 调用线程对象的start()方法来启动该线程。

    	package com.thread;  
    	  
    	public class FirstThreadTest extends Thread{  
    	    int i = 0;  
    	    //重写run方法，run方法的方法体就是线程执行体  
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

#### 通过Runnable接口创建类
- 定义runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体。
- 创建 Runnable实现类的实例，并依此实例作为Thread的target来创建Thread对象，该Thread对象才是真正的线程对象。
- 调用线程对象的start()方法来启动该线程。

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
      	                new Thread(rtt,"新线程1").start();  
      	                new Thread(rtt,"新线程2").start();  
      	            }  
      	        }  
      	    }  	  
      	}  

#### 通过Callable和Future创建线程
- 创建 Callable 接口的实现类，并实现 call() 方法，该 call() 方法将作为线程执行体，并且有返回值。
- 创建 Callable 实现类的实例，使用 FutureTask 类来包装 Callable 对象，该 FutureTask 对象封装了该 Callable 对象的 call() 方法的返回值。
- 使用 FutureTask 对象作为 Thread 对象的 target 创建并启动新线程
- 调用 FutureTask 对象的 get() 方法来获得子线程执行结束后的返回值。

      public class CallableThreadTest implements Callable<Integer> {
          public static void main(String[] args)  
          {  
              CallableThreadTest ctt = new CallableThreadTest();  
              FutureTask<Integer> ft = new FutureTask<>(ctt);  
              for(int i = 0;i < 100;i++)  
              {  
                  System.out.println(Thread.currentThread().getName()+" 的循环变量i的值"+i);  
                  if(i==20)  
                  {  
                      new Thread(ft,"有返回值的线程").start();  
                  }  
              }  
              try  
              {  
                  System.out.println("子线程的返回值："+ft.get());  
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

## Java并发包
Java并发包是java提供的多线程实现，可以大大简化多线程开发。
java并发包内容转向<a href="https://github.com/Xchunguang/java-review-docs/blob/master/docs/java5%E6%96%B0%E7%89%B9%E6%80%A7.md#java%E5%B9%B6%E5%8F%91%E5%8C%85javautilconcurrent">链接</a>