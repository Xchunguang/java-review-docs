# Java5的新增特性
一看标题，现今Java11都已经发行了，提Java5是不是有些太过于老套，但我觉得Java5作为一个关键的大版本，还是有意义再提一遍。
- 泛型(Generics)
  Java泛型提供了`编译时类型安全检测机制`,泛型的本质是`类型参数化`，也就是说所操作的数据类型被指定为一个参数。
  
  java泛型方法和泛型类的使用可以是泛型方法声明指定一组相关方法，泛型类可以声明指定一组相关类。
  
  - 泛型方法
  
    泛型方法在调用时可以接收不同类型的参数，根据传递给泛型方法的参数类型，编译器适当的处理每一个方法的调用。
    
    泛型方法的定义规则如下：
    
    - 每一个泛型方法声明都有一个类型参数声明部分，该参数在方法的返回值前，使用尖角号包围，内部逗号隔开，也被称为类型变量，用来指定一个泛型类型名称的标识符：
    
          public static <T,K> void genericsMethod(T t,K k){...}
          
    - 类型的参数可以被用来声明返回值的类型
    
          public static <T,K> T genericsMethod(T t,K k){...}
    
    - 泛型方法体的声明和其他方法一样。注意类型参数只能代表引用型类型，不能是原始类型（像int,double,char的等）。
    
          例如：
          Age<Integer> age = new Age<Integer>();//正确

          Age<int> age = new Age<int>();//编译出错
    
    - 泛型方法的类型参数可以设置成有界的类型参数，即确定类型的范围，使用extends和super表示：
    
          public static <T extends BaseVO> T genericsMethod(T t){...}
          //上面语句表示T的类型范围为BaseVO的子类
          
          public static <T super BaseVO> T genericsMethod(T t){...}
          //上面语句表示T的类型范围为BaseVO的父类
          
  - 泛型类
  
    泛型类的声明和非泛型类的声明类似，只是在类的后面添加了参数声明。泛型类的类型参数也可以使用extends或super确定类型范围。  
    
          /**
           * 定义基础类
           */
          class Person{
            private String name;
            
            public String getName(){
              return name;
            }
            
            public Person(String name){
              this.name = name;
            }
          }

          /**
           * 定义学生类
           */
          class Student extends Person{
            
            public Student(String name){
              super(name);
            }
          }

          /**
           * 定义教师类
           */
          class Teacher extends Person{
            
            public Teacher(String name){
              super(name);
            }
          }

          /**
           * 定义班级类，班级里有教师和学生
           * @param <T>
           */
          class Classes<T extends Person>{
            
            private List<T> personList;
            
            public Classes(){
              personList = new ArrayList<T>();
            }
            
            public void addPerson(T t){
              personList.add(t);
            }
            
            public void sayNames(){
              for(T t : personList){
                System.out.println("name is " + t.getName());
              }
            }
          }
          public class Java5 {
            
            public static void main(String[] args) {
              Student stu1 = new Student("Xiaoming");
              Teacher tea1 = new Teacher("Mr Wang");
              
              Classes<Person> class1 = new Classes<Person>();
              class1.addPerson(stu1);
              class1.addPerson(tea1);
              
              class1.sayNames();
              
            }
          }
    
  - 类型通配符
  
    类型通配符一般是使用`?`代替具体的类型参数，例如List<?>即包括了List<String>、List<Integer>等所有类型参数。
    
        public void genericsMethod(List<?> list){...}
  
    类型通配符也可以使用extends和super确定类型范围：
    
        public void genericsMethod(List<? extends Person> list){...}

  - 类型参数名约定写法：
  
    按照约定，类型参数名称命名规则为单个大写子母，常用的类型参数名称如下：
    
        E - 元素，主要由Java集合(Collections)框架使用。
        K - 键，主要用于表示映射中的键的参数类型。
        V - 值，主要用于表示映射中的值的参数类型。
        N - 数字，主要用于表示数字。
        T - 类型，主要用于表示第一类通用型参数。
        S - 类型，主要用于表示第二类通用类型参数。
        U - 类型，主要用于表示第三类通用类型参数。
        V - 类型，主要用于表示第四个通用类型参数。
  
  - 类型推断：
  
    类型推断表示Java编辑器方法的调用及其声明自动检查和确认参数类型，例如：
    
        List<String> list = new ArrayList<>();
        //上面写法中<>尖括号运算符表示类型推断，该操作符是从Java7开始的，而Java7之前，你只能：
        List<String> list = new ArrayList<String>();
        
  - 泛型的限制：
  
    - 泛型的类型参数不能使用原始类型：在上面已经介绍过了，不过可以使用封装类型
    - 泛型中类型参数不能声明实例：
    
          public <T> void addClasses(Classes<T> classes){
            T t = new T();  //编译错误
            classes.addPerson(t);
          }
          
          //如果想要实现实例化泛型类，通过反射进行操作：
          public <T> void addClasses(Classes<T> classes,Class<T> clazz) throws Exception{
            T t = clazz.newInstance();
            classes.addPerson(t);
          }

    - 类型参数不能为静态，静态参数在类实例加载之前就已经加载，编译器无法知道要使用的类型：
    
          class Classes<T extends Person>{
          
              private static List<T> personList;//编译报错
              ...
          }
    
    - 类型参数无法做强制类型转换：
    
          Age<Integer> integerAge = new Age<Integer>();
          Age<Number> numberAge = new Age<Number>();
          
          integerBox = (Age<Integer>)numberBox;//编译报错
          
          但使用无界的通配符可以：
          
          private static void add(Age<?> age){
             Age<Integer> integerAge = (Age<Integer>)age;
          }
          
    - 在方法中不能catch一个类型参数的实例，但是可以throws 一个类型参数：
    
          public static <T extends Exception, J> 
             void execute(List<J> jobs) {
                try {
                   for (J job : jobs){}

                } catch (T e) { //编译报错Cannot use the type parameter T in a catch block
                   ...
             }
          }

- 增强for循环(Enhanced for Loop)

  Java5简化了for循环的操作：
  
       int[] arr = {1, 2, 3, 4, 5};
       for (int i : arr) {
          System.out.println(i);
       } 
       
- 自动拆装箱(Autoboxing/Unboxing)

  Java的八种基本类型都分别有一个包装类，当需要时，基本类型和其包装类之间就会自动转换，基本类型转为包装类称为装箱，包装类转换为基础类型称为拆箱。
  
      Integer integerValue = new Integer(1);
      int intValue = integerValue;
      
- 枚举(Typesafe Enums)

  Java枚举一般代表一组常用的常量，且代表一类相同类型的常量值。
  
  同时Java枚举有线程安全、自由序列化、保证单例的特性，所以也可以作为单例模式的一种实现：
  
      public enum SingletonEnum {
          INSTANCE;
          public void otherMethods(){
              System.out.println("do something");
          }
      }

- 可变参数 (Varargs)

  可变参数是一个类似数组一样的参数，可用循环遍历，而且可变参数只能放在方法参数列表的最后一个。
  
      public static void varags(String... names){
        for(String name : names){
          System.out.println(name);
        }
      }
      
      public static void main(String[] args) {
        varags("1","2","3");
      }
      
- 静态导入（Static Import）

  通过import导入一些静态的方法和变量，可以不用每次使用都加`类型.静态方法`或`类名.静态变量`形式
      
      //单个导入
      import static java.lang.Math.PI;
      //批量导入
      import static java.lang.Math.*;

- 注解（Annotations）

  Annotations是Java5提供的新功能，它可以将元数据与程序元素相关联。声明注解：
  
      <modifiers> @interface <annotation-type-name>  {
          // Annotation type body
      }
      
  注解声明的<modifiers>修辞符与接口声明的相同。可以将注解类型声明为public或package级别。@符号与interface关键字之间可以用空格分隔，也可以连接在一起。按照惯例，它们一般是连接放在一起，如：@interface。interface关键字后面是注解类型名称。 它应该是有效的Java标识符。

      public  @interface  Version {
          int  major();
          int  minor();
      }
      
      @Version(major=1, minor=0)
      public  class  VersionTest {

      }
- Java并发包(java.util.concurrent)

  Java5新增的并发包，使得并发编程更加轻松简单。此处总结一些并发包中的常用类：
  
  - 阻塞队列：BlockingQueue
  
    BlockingQueue接口：该接口表示的是一个线程安全的插入和提取实例的队列。
    
    ![阻塞队列](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/BlockQueue.png)

    - 用法：
    
      BlockingQueue通常用于一个线程生产对象，另一个线程消费对象的场景。
      
      ![阻塞队列](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/queue-oper.png)

      该队列有容纳临界点，即有限制的，当该阻塞队列到达了临界点，负责插入对象的线程将进入阻塞状态，直到消费的线程将对象拿走。且如果一个消费线程尝试从一个空的卒社队列提取对象时，也会进入阻塞状态，直到生产线程将新的对象放入阻塞队列。
      
    - 对象的操作方法：
      
      BlockingQueue提供了4组不同的方法用于插入、删除和检查元素。每组方法对不同的结果的表现不同
      
|             | 抛出异常  |  特定返回值  | 阻塞   |  超时  |
| --------    | :-----:   | :----:       |:----:  |:----:  |
| 插入        | add(e)    |  offer(e)    |put(e)  |offer(e,time,unit)  |
| 删除        | remove()  |  poll()      |take()  |poll(time,unit)   |
| 检查        | element() |  peek()      |不支持  |不支持     |
    
      - 抛出异常：
      
        如果试图的操作无法立即执行，将抛出一个异常
        
      - 返回特定值：
      
        如果试图的操作无法立即执行，将返回一个特定值，通常为true/false
        
      - 阻塞：
      
        如果试图的操作无法立即执行，该方法将发生阻塞，直到能够执行
        
      - 超时：
      
        如果试图的操作无法立即执行则该方法将会阻塞，直到能够执行，但是有一个超时时间，阻塞时间不会超过超时时间，并会返回一个值表示操作是否成功true/false。
  
    - 注意
    
      无法向一个BlockingQueue中插入null，如果试图插入null，则阻塞队列会抛出一个异常NullPointerException。
      
      可以访问阻塞队列的所有元素，而不仅仅是开始和结束的元素。例如可以移除掉队列中的任一一个元素，但这种操作的效率很低，除非必须如此。
      
    - 实现类示例：
    
          class Producer implements Runnable {
             private final BlockingQueue queue;
             Producer(BlockingQueue q) { queue = q; }
             public void run() {
               try {
                 while (true) { queue.put(produce()); }
               } catch (InterruptedException ex) { ... handle ...}
             }
             Object produce() { ... }
           }

           class Consumer implements Runnable {
             private final BlockingQueue queue;
             Consumer(BlockingQueue q) { queue = q; }
             public void run() {
               try {
                 while (true) { consume(queue.take()); }
               } catch (InterruptedException ex) { ... handle ...}
             }
             void consume(Object x) { ... }
           }

           class Setup {
             void main() {
               BlockingQueue q = new SomeQueueImplementation();
               Producer p = new Producer(q);
               Consumer c1 = new Consumer(q);
               Consumer c2 = new Consumer(q);
               new Thread(p).start();
               new Thread(c1).start();
               new Thread(c2).start();
             }
           }

  - 数组阻塞队列：ArrayBlockingQueue
    
    ArrayBlockingQueue是BlockingQueue的一种实现。该队列的内部是使用数组进行对象的存放，ArrayBlockingQueue是一个有界的阻塞队列，初始化时就要确定该队列的容量，并且初始化后无法更改容量。
    
    ArrayBlockingQueue是一个FIFO先进先出的队列。头部的元素是队列中最长时间的元素，尾部的元素是队列中最短时间的元素，ArrayBlockingQueue队列从尾部插入新元素，从头部检索提取元素。
  
        BlockingQueue<String> queue = new ArrayBlockingQueue<String>(1024);  
        queue.put("InserDemoStr");  
        String string = queue.take();  
        
  - 延迟队列：DelayQueue
  
    延迟队列也是阻塞队列的一种实现类，该队列对元素的持有直到一个特定的延迟时期，延迟队列要求注入的元素必须实现java.util.concurrent.Delayed接口：
    
        public interface Delayed extends Comparable<Delayed> {  

         public long getDelay(TimeUnit timeUnit);  

        }  
        
    其中延迟队列会根据每个元素的getDelay()方法的返回值的时间段后就会释放该元素。
    
  - 链阻塞队列：LinkedBlockingQueue
  
    链式阻塞队列内部是使用链表实现的阻塞队列，该队列可选是否定义上限。链式阻塞队列也是一种先进先出队列。
    
  - 优先级阻塞队列：PriorityBlockingQueue
  
    优先级阻塞队列是一个无界的并发队列，它使用了和类 java.util.PriorityQueue 一样的排序规则。你无法向这个队列中插入 null 值。
    
    该队列所有插入的元素必须实现java.lang.Comparable接口，该队列中的元素排列是取决于插入元素对Comparable接口的实现。
    
  - 同步队列：SynchronousQueue
  
    同步队列是个特殊的队列，该队列内部只能同时容纳单个元素，如果该队列中包含了一个元素，那么所有插入队列的线程都会阻塞。
    
  - 阻塞双队列：BlockingDeque
  
    双端队列是一个可以从任意一端插入或提取元素的队列。
    
    ![双端队列](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/BlockDueue.png)

    在线程的使用中，该线程即是生产者又是消费者可以使用阻塞双端队列，即生产者可以从队列的两端都可以插入数据，也可以从队列的两段提取数据。
    
    - 对象的操作方法：
      
      BlockingDueue也提供了4组不同的方法用于插入、删除和检查元素。每组方法对不同的结果的表现不同
      
| 头          | 抛出异常       |  特定返回值       | 阻塞        |  超时  |
| --------    | :-----:        | :----:            |:----:       |:----:  |
| 插入        | addFirst(e)    |  offerFirst(e)    |putFirst(e)  |offerFirst(e,time,unit)  |
| 删除        | removeFirst()  |  pollFirst()      |takeFirst()  |pollFirst(time,unit)   |
| 检查        | getFirst()     |  peekFirst()      |不支持       |不支持     |

| 尾          | 抛出异常      |  特定返回值      | 阻塞       |  超时  |
| --------    | :-----:       | :----:           |:----:      |:----:  |
| 插入        | addLast(e)    |  offerLast(e)    |putLast(e)  |offerLast(e,time,unit)  |
| 删除        | removeLast()  |  pollLast()      |takeLast()  |pollLast(time,unit)   |
| 检查        | getLast()     |  peekLast()      |不支持      |不支持     |
    
      - 抛出异常：
      
        如果试图的操作无法立即执行，将抛出一个异常
        
      - 返回特定值：
      
        如果试图的操作无法立即执行，将返回一个特定值，通常为true/false
        
      - 阻塞：
      
        如果试图的操作无法立即执行，该方法将发生阻塞，直到能够执行
        
      - 超时：
      
        如果试图的操作无法立即执行则该方法将会阻塞，直到能够执行，但是有一个超时时间，阻塞时间不会超过超时时间，并会返回一个值表示操作是否成功true/false。
  
  - 链式阻塞双端队列：LinkedBlockDuque
  
    链式阻塞双端队列内部是通过链表实现，在它为空的时候，一个试图从中抽取数据的线程将会阻塞，无论该线程是试图从哪一端抽取数据。
    
  - 并发Map：ConcurrentMap:
  
    并发Map表示并发处理的Map，并发Map除继承自Map的方法外还提供了一些原子方法。
    
    ![并发Map](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/ConcurrentMap.png)

    - ConcurrentHashMap:
    
      concurrentHashMap就是并发的HashMap，在写入和读取ConcurrentHashMap时，并不会将整个Map都锁住，而是锁住写入的部分。

  - 闭锁：CountDownLatch:
  
    java.util.concurrent.CountDownLatch类似是一种计数器类型的功能，它定义了一个初始大小，使用await()方法开始计数，然后通过countDown()方法进行计数操作，当计数完成时将执行await()后面的方法。
    
        简单用法：
        class Driver { // ...
         void main() throws InterruptedException {
           CountDownLatch startSignal = new CountDownLatch(1);
           CountDownLatch doneSignal = new CountDownLatch(N);

           for (int i = 0; i < N; ++i) // create and start threads
             new Thread(new Worker(startSignal, doneSignal)).start();

           doSomethingElse();            // don't let run yet
           startSignal.countDown();      // let all threads proceed
           doSomethingElse();
           doneSignal.await();           // wait for all to finish
         }
       }

       class Worker implements Runnable {
         private final CountDownLatch startSignal;
         private final CountDownLatch doneSignal;
         Worker(CountDownLatch startSignal, CountDownLatch doneSignal) {
            this.startSignal = startSignal;
            this.doneSignal = doneSignal;
         }
         public void run() {
            try {
              startSignal.await();
              doWork();
              doneSignal.countDown();
            } catch (InterruptedException ex) {} // return;
         }

         void doWork() { ... }
       }

  - 回环栅栏 CyclicBarrier：
  
    它可以实现让一组线程等待至某个状态之后再全部同时执行。叫做回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用。
    
        public class CyclicBarrierTest {
          public static void main(String[] args) {
              int N = 4;
              CyclicBarrier barrier  = new CyclicBarrier(N);
              for(int i=0;i<N;i++)
                  new Writer(barrier).start();
          }
          static class Writer extends Thread{
              private CyclicBarrier cyclicBarrier;
              public Writer(CyclicBarrier cyclicBarrier) {
                  this.cyclicBarrier = cyclicBarrier;
              }
       
              @Override
              public void run() {
                  System.out.println("线程"+Thread.currentThread().getName()+"正在写入数据");
                  try {
                      Thread.sleep(5000);      
                      System.out.println("线程"+Thread.currentThread().getName()+"写入数据完毕，等待其他线程写入完毕");
                      cyclicBarrier.await();
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }catch(BrokenBarrierException e){
                      e.printStackTrace();
                  }
                  System.out.println("所有线程写入完毕");
              }
          }
      }