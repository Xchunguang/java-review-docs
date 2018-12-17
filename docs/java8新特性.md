# java8新特性

![java8](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java8.png)

## Lambda表达式


- Lambda允许把函数作为一个方法的参数（函数作为参数传递进方法中），或者把代码看成数据

- 最简单的形式中，一个lambda可以由用逗号分隔的参数列表、–>符号与函数体三部分表示：

		Arrays.asList( 1, 2, 3 ).forEach( e -> System.out.println( e ) );

- Lambda可以引用类的成员变量与局部变量（如果这些变量不是final的话，它们会被隐含的转为final，这样效率更高）：

		String str = ",";			//等价于：final String str = ",";
		Arrays.asList( "a", "b", "c" ).forEach( 
		    ( String e ) -> System.out.print( e + str ) );	
		    


- Lambda可能会返回一个值。返回值的类型也是由编译器推测出来的。如果lambda的函数体只有一行的话，那么没有必要显式使用return语句:

		List<Integer> list = Arrays.asList( 4, 2, 1,3);
		list.sort( ( e1, e2 ) -> e1.compareTo( e2 ) );
		list.forEach(e->System.out.println(e));	
					
			
## 函数式接口

- 如何使现有的函数友好地支持lambda，java8采取了增加函数式接口的概念。
- 函数式接口就是一个只有一个方法的普通接口，可以隐式的转换成lambda表达式，除了特殊方法：默认方法，静态方法以及继承自Object类的一些方法（toString(),equels()等）
- 在使用中，函数式接口容易出错，如果接口中被定义了另一个方法，那么接口将不再是函数式接口，导致编译失败。为此Java8新增注解@FunctionalInterface。注意default默认方法和静态方法不会影响函数式接口。

		@FunctionalInterface
		public interface Runnable {
		    public abstract void run();
		}

- java8新增默认方法和静态方法扩展接口的声明。

		public interface Convert {

			//可以包含静态方法
			public static void hasStaticMethod(){
				System.out.println("包含静态方法");
			}
			
			default void hasDefaultMethod(){
				System.out.println("包含默认方法");
			}
		}
		
- 函数式接口示例

		@FunctionalInterface
		public interface Convert<F,T> {
		
			T convert(F from);
			
			//可以包含静态方法
			public static void hasStaticMethod(){
				System.out.println("包含静态方法");
			}
			
			default void hasDefaultMethod(){
				System.out.println("包含默认方法");
			}
		}
		
		public class ConvertMain {
		public static void main(String[] args) {
	
			Convert<String, Integer> converter = (from) -> Integer.valueOf(from);
			Integer converted = converter.convert("123");
			System.out.println(converted);
			converter.hasDefaultMethod();
			Convert.hasStaticMethod();
			
			// Function<T, R> -T作为输入，返回的R作为输出
			Function<String,String> function = (x) -> {System.out.print(x+": ");return "Function";};
			System.out.println(function.apply("hello world"));
	
			//Predicate<T> -T作为输入，返回的boolean值作为输出
			Predicate<String> pre = (x) ->{System.out.print(x);return false;};
			System.out.println(": "+pre.test("hello World"));
	
			//Consumer<T> - T作为输入，执行某种动作但没有返回值
			Consumer<String> con = (x) -> {System.out.println(x);};
			con.accept("hello world");
	
			//Supplier<T> - 没有任何输入，返回T
			Supplier<String> supp = () -> {return "Supplier";};
			System.out.println(supp.get());
			//BinaryOperator<T> -两个T作为输入，返回一个T作为输出，对于“reduce”操作很有用
			BinaryOperator<String> bina = (x,y) ->{System.out.print(x+" "+y);return "BinaryOperator";};
			System.out.println("  "+bina.apply("hello ","world"));
			
		}
	}
				
						
			
## 方法引用（::）

- 以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用

- 第一种方法引用是构造器引用，它的语法是Class::new，或者更一般的Class< T >::new。请注意构造器没有参数。

		final Car car = Car.create( Car::new );
		final List< Car > cars = Arrays.asList( car );

- 第二种方法引用是静态方法引用，它的语法是Class::static_method。请注意这个方法接受一个Car类型的参数。

		cars.forEach( Car::collide );
		
- 第三种方法引用是特定类的任意对象的方法引用，它的语法是Class::method。请注意，这个方法没有参数。

		cars.forEach( Car::repair );
		
- 最后，第四种方法引用是特定对象的方法引用，它的语法是instance::method。请注意，这个方法接受一个Car类型的参数

		final Car police = Car.create( Car::new );
		cars.forEach( police::follow );
		
		private void sort(){
		    List<Commodity> commodities=new ArrayList<>();
		    //java 8之前
		    commodities.sort(new Comparator<Commodity>() {
		    @Override
		    public int compare(Commodity o1, Commodity o2) {
		            return o1.getPrice()-o2.getPrice();
		        }
		    });
		    //java 8 lambda的写法
		    commodities.sort((Commodity o1,Commodity o2)->o1.getPrice()-o2.getPrice());
		    //java 8 方法应用的写法
		    commodities.sort(Comparator.comparing(Commodity::getPrice));
		}

	
## 重复注解

- java5开始引用注解机制，然而相同注解在同样的地方只能声明一次，java8引入重复注解。

- 重复注解机制本身必须用@Repeatable注解。事实上是编译器技巧的改变
	
	
## 更好的类型推断

- Java 8在类型推测方面有了很大的提高。在很多情况下，编译器可以推测出确定的参数类型，这样就能使代码更整洁。

		public class Value< T > {
		    public static< T > T defaultValue() { 
		        return null; 
		    }
		     
		    public T getOrDefault( T value, T defaultValue ) {
		        return ( value != null ) ? value : defaultValue;
		    }
		}
		
		public class TypeInference {
		    public static void main(String[] args) {
		        final Value< String > value = new Value<>();
		        value.getOrDefault( "22", Value.defaultValue() );
		    }
		}

## 扩展注解的支持

- Java 8扩展了注解的上下文。现在几乎可以为任何东西添加注解：局部变量、泛型类、父类与接口的实现，方法的异常也可以添加注解。

		public class Annotations {
		    @Retention( RetentionPolicy.RUNTIME )
		    @Target( { ElementType.TYPE_USE, ElementType.TYPE_PARAMETER } )
		    public @interface NonEmpty {        
		    }
		         
		    public static class Holder< @NonEmpty T > extends @NonEmpty Object {
		        public void method() throws @NonEmpty Exception {           
		        }
		    }
		         
		    @SuppressWarnings( "unused" )
		    public static void main(String[] args) {
		        final Holder< String > holder = new @NonEmpty Holder< String >();       
		        @NonEmpty Collection< @NonEmpty String > strings = new ArrayList<>();       
		    }
		}
		
	ElementType.TYPE_USE和ElementType.TYPE_PARAMETER是两个新添加的用于描述适当的注解上下文的元素类型。
		
## Optional

- 新增类库，为解决java中常见的空指针异常导致程序无法正常运行

- Optional实际上是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

		//允许为空值，当值为空时也不会报错
		Optional< String > name = Optional.ofNullable( null );
		System.out.println( "name是否有值 " + name.isPresent() );        
		System.out.println( "如果名称为空: " + name.orElseGet( () -> "代替名称" ) ); 
		System.out.println( name.map( s -> "名称为： " + s  ).orElse( "名称为空" ) );
		
		//不允许为空值，当值为空时会报错
		Optional< String > firstName = Optional.of( "name" );
		System.out.println( "firstName是否有值 " + firstName.isPresent() );        
		System.out.println( "如果名称为空: " + firstName.orElseGet( () -> "代替名称" ) ); 
		System.out.println( firstName.map( s -> "名称为： " + s  ).orElse( "名称为空" ) );
		
	Optional<a href="https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html">文档</a>
		
## Stream
java8中stream是对集合（Collection)对象的功能上的增强。提供对集合高效的聚合和过滤操作。支持串行和并行两种模式进行汇聚操作。stream并非集合元素，并不会保存数据，如同迭代器一样，单向的不可往复的，遍历一次即用尽。而与迭代器不同的是，stream可以进行并行操作。

#### stream的构成：
  使用流的过程基本分为：数据来源（source）->数据转换->执行结果。每次执行转换后原有的stream不变，返回一个新的stream，致使操作形成链，变成一个管道。

#### 数据源的来源:
- 集合对象Collection和数组
- Collection.stream()
- Collection.parallelStream()
- Arrays.stream(T array)
- Stream.of()
- ...

#### 流的三种操作：
- Intermediate:一个流可以进行零个或多个转换操作，目的就是打开流，执行某种过滤或map映射等返回一个新流，交给下一个操作使用。此类操作都是惰性化的，就是说仅仅调用这个方法并没有开始流的遍历。map/filter/distinct/sorted/peek/limit/skip/parallel/sequential/unordered
- Terminal：结果操作，一个流只能有一个该操作，当执行该操作，流就相当于被使用光了，不会再产生其他流，并且会生成一个结果。forEach/toArray/reduce/collect/min/max/count/anyMatch/allMatch/noneMatch/findFirst/findAny/limit
- short-circuiting：对于一个无限大的stream，在有限的时间内计算出结果或返回一个新的stream。anyMatch/allMatch/noneMatch/findFirst/findAny/limit
  
#### 常见操作：
  - map:映射，map生成的是1:1的映射，每次输入的元素都会按照规则转换成另外一种规则的元素。
  
        /**
         * 简单收集，收集某属性集合
         */
        public Set<String> collectDate(List<UserVO> userList){
          return userList.stream().map(UserVO::getUserName).collect(Collectors.toSet());
        }
        
  - flatMap:一对多映射，通常用于合并流操作
      
	      Stream<List<Integer>> inputStream = Stream.of(
	       Arrays.asList(1),
	       Arrays.asList(2, 3),
	       Arrays.asList(4, 5, 6)
	       );
	      Stream<Integer> outputStream = inputStream.
	      flatMap((childList) -> childList.stream());
      
  - filter:filter对原始的stream进行某测试，通过测试的元素形成一个新的stream
    
          /**
           * 简单过滤查找列表
           * @param userList
           * @param age
           * @return
           */
          public List<UserVO> simpleFilterFindList(List<UserVO> userList,int age){
            return userList.stream().filter((userVO)->userVO.getAge()==age).collect(Collectors.toList());
          }
          
  - forEach:接受lambda表达式，然后在流的每个元素执行表达式
  - findFirst:该操作是Terminal操作又是short-circuiting操作。总是返回集合的第一个元素的Optional
    
        /**
         * 查找第一个
         * @param userList
         * @return
         */
        public UserVO simpleFilter(List<UserVO> userList,String userName){
          return userList.stream().filter((userVO)->userVO.getUserName().equals(userName)).findFirst().orElse(null);
        }
        
  - reduce：该操作主要是将stream元素组合起来，提供一个起始值，然后依照规则跟前面的stream的各个元素的组合。
  
	      // 字符串连接，concat = "ABCD"
	      String concat = Stream.of("A", "B", "C", "D").reduce("", String::concat); 
	      // 求最小值，minValue = -3.0
	      double minValue = Stream.of(-1.5, 1.0, -3.0, -2.0).reduce(Double.MAX_VALUE, Double::min); 
	      // 求和，sumValue = 10, 有起始值
	      int sumValue = Stream.of(1, 2, 3, 4).reduce(0, Integer::sum);
	      // 求和，sumValue = 10, 无起始值
	      sumValue = Stream.of(1, 2, 3, 4).reduce(Integer::sum).get();
	      // 过滤，字符串连接，concat = "ace"
	      concat = Stream.of("a", "B", "c", "D", "e", "F").
	       filter(x -> x.compareTo("Z") > 0).
	       reduce("", String::concat);
       
  - limit/skip:limit是返回stream的前n个元素，而skip则是对其stream的前n个元素
  
	      /**
	       * limit 是取前n个元素
	       * skip是丢弃前n个元素
	       * @param userList
	       * @return
	       */
	      public List<UserVO> limitAndSkip(List<UserVO> userList){
	        return userList.stream().limit(10).skip(2).collect(Collectors.toList());
	      }
      
  - sorted:对stream的排序，好处是可以先对stream进行其他操作如map,filter,limit等操作再进行排序
  
        /**
         * 排序集合
         * @param userList
         * @return
         */
        public List<UserVO> sortList(List<UserVO> userList){
          return userList.stream().sorted((t1,t2) -> t1.getUserName().compareTo(t2.getUserName())).collect(Collectors.toList());
        }
  
  - min/max/distinct:常见的取最大值最小值和去重操作
  - Match：allMatch/anyMatch/noneMatch,这些都是不需要遍历完所有元素就可以返回结果的，例如allMatch有一个不满足就会返回false。
  
        /**
         * 检查是否所有元素都匹配条件
         * @param userList
         * @return
         */
        public boolean matchAllUser(List<UserVO> userList){
          return userList.stream().allMatch((userVO)->userVO.getAge()>=0);
        }

  - 自定义生成流：Stream.generate
    通过Supplier接口可以自定义流的生成，常用于随机数，常量或者前后保持某种关系的流。必须使用limit控制流的大小。
    
          /**
           * 自定义流
           */
          public void generateStream(){
            Random ran = new Random();
            Supplier<Integer> stream = ran::nextInt;
            Stream.generate(stream).limit(10).forEach((value) -> System.out.println(value));
          }

  - Stream.iterate:操作与reduce类似，接受一个初始值，和一个操作，依次执行下去。也需要使用limit进行控制流的大小。
  
  
  - groupingBy:分组
  
      	/**
         * 将集合分组
         * 根据年龄分组
         * @param userList
         * @return
         */
        public Map<Integer,List<UserVO>> groupUserVO(List<UserVO> userList){
          return userList.stream().collect(Collectors.groupingBy(userVO -> userVO.getAge()));
        }
        
  - partitioningBy:分割
  
      	/**
         * 分割数据集，用年龄分割
         * @param userList
         * @return
         */
        public Map<Boolean,List<UserVO>> splitUserList(List<UserVO> userList){
          return userList.stream().collect(Collectors.partitioningBy(userVO -> userVO.getAge()>15));
        }


#### 总结
- 流不进行存储，不是一种数据结构，也不会修改底层的数据源，只会产生新的流
- Intermediate操作永远是惰性的，很多操作是向后延迟，直到知到多少数据才会开始
- stream可以并行处理，无需编写多线程代码
- stream可以是无限的，但一般需要limit和findFirst等short-curcutiting方法快速得到结果

## Date/Time API (JSR 310)

- Joda-Time——一个可替换标准日期/时间处理且功能非常强大的Java API

- <a href="https://www.javacodegeeks.com/2014/03/whats-new-in-java-8-date-api.html">新的java.time包涵盖了所有处理日期，时间，日期/时间，时区，时刻（instants），过程（during）与时钟（clock）的操作</a>

- Clock类，它通过指定一个时区，然后就可以获取到当前的时刻，日期与时间。Clock可以替换System.currentTimeMillis()与TimeZone.getDefault()。
	
		final Clock clock = Clock.systemUTC();
		System.out.println( clock.instant() );
		System.out.println( clock.millis() );
		
- LocaleDate与LocalTime。LocaleDate只持有ISO-8601格式且无时区信息的日期部分。相应的，LocaleTime只持有ISO-8601格式且无时区信息的时间部分。LocaleDate与LocalTime都可以从Clock中得到

		final LocalDate date = LocalDate.now();
		final LocalDate dateFromClock = LocalDate.now( clock );
		         
		System.out.println( date );
		System.out.println( dateFromClock );
		         
		// Get the local date and local time
		final LocalTime time = LocalTime.now();
		final LocalTime timeFromClock = LocalTime.now( clock );
		         
		System.out.println( time );
		System.out.println( timeFromClock );
		
- LocaleDateTime把LocaleDate与LocaleTime的功能合并起来，它持有的是ISO-8601格式无时区信息的日期与时间。

		final LocalDateTime datetime = LocalDateTime.now();
		final LocalDateTime datetimeFromClock = LocalDateTime.now( clock );
		         
		System.out.println( datetime );
		System.out.println( datetimeFromClock );
		
- 如果你需要特定时区的日期/时间，可以使用ZonedDateTime。它持有ISO-8601格式具具有时区信息的日期与时间

		final ZonedDateTime zonedDatetime = ZonedDateTime.now();
		final ZonedDateTime zonedDatetimeFromClock = ZonedDateTime.now( clock );
		final ZonedDateTime zonedDatetimeFromZone = ZonedDateTime.now( ZoneId.of( "America/Los_Angeles" ) );
		         
		System.out.println( zonedDatetime );
		System.out.println( zonedDatetimeFromClock );
		System.out.println( zonedDatetimeFromZone );
		
		
- Duration类：Duration使计算两个日期间的差变的十分简单。

		final LocalDateTime from = LocalDateTime.of( 2014, Month.APRIL, 16, 0, 0, 0 );
		final LocalDateTime to = LocalDateTime.of( 2015, Month.APRIL, 16, 23, 59, 59 );
		 
		final Duration duration = Duration.between( from, to );
		System.out.println( "Duration in days: " + duration.toDays() );
		System.out.println( "Duration in hours: " + duration.toHours() );
			
## JavaScript引擎Nashorn

- Nashorn，一个新的JavaScript引擎随着Java 8一起公诸于世，它允许在JVM上开发运行某些JavaScript应用。Nashorn就是javax.script.ScriptEngine的另一种实现，并且它们俩遵循相同的规则，允许Java与JavaScript相互调用。

		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName( "JavaScript" );
		         
		System.out.println( engine.getClass().getName() );
		System.out.println( "Result:" + engine.eval( "function f() { return 1; }; f() + 1;" ) );
			
			
## Base64：Java 8中，Base64编码已经成为Java类库的标准。

	import java.nio.charset.StandardCharsets;
	import java.util.Base64;

	public class Base64Test {
		public static void main(String[] args) {
			final String text = "Base64 finally in Java 8!";

			final String encoded = Base64
			.getEncoder()
			.encodeToString( text.getBytes( StandardCharsets.UTF_8 ) );
			System.out.println( encoded );

			final String decoded = new String( 
			Base64.getDecoder().decode( encoded ),
			StandardCharsets.UTF_8 );
			System.out.println( decoded );
		}
	}

Base64类同时还提供了对URL、MIME友好的编码器与解码器（Base64.getUrlEncoder() / Base64.getUrlDecoder(), Base64.getMimeEncoder() / Base64.getMimeDecoder()）。
			
## 并行（parallel）数组

- Java 8增加了大量的新方法来对数组进行并行处理。可以说，最重要的是parallelSort()方法，因为它可以在多核机器上极大提高数组排序的速度。


	import java.util.Arrays;
	import java.util.concurrent.ThreadLocalRandom;

	public class ParallelArrays {
		public static void main( String[] args ) {
			long[] arrayOfLong = new long [ 20000 ];        

			Arrays.parallelSetAll( arrayOfLong, 
			index -> ThreadLocalRandom.current().nextInt( 1000000 ) );
			Arrays.stream( arrayOfLong ).limit( 10 ).forEach( 
			i -> System.out.print( i + " " ) );
			System.out.println();

			Arrays.parallelSort( arrayOfLong );     
			Arrays.stream( arrayOfLong ).limit( 10 ).forEach( 
			i -> System.out.print( i + " " ) );
			System.out.println();
		}
	}
			
上面的代码片段使用了parallelSetAll()方法来对一个有20000个元素的数组进行随机赋值。然后，调用parallelSort方法。这个程序首先打印出前10个元素的值，之后对整个数组排序。
			
			
## 并发（Concurrency）

- 在新增Stream机制与lambda的基础之上，在java.util.concurrent.ConcurrentHashMap中加入了一些新方法来支持聚集操作。同时也在java.util.concurrent.ForkJoinPool类中加入了一些新方法来支持共有资源池（common pool）

- 新增的java.util.concurrent.locks.StampedLock类提供一直基于容量的锁，这种锁有三个模型来控制读写操作（它被认为是不太有名的java.util.concurrent.locks.ReadWriteLock类的替代者）。

	在java.util.concurrent.atomic包中还增加了下面这些类：
	
		DoubleAccumulator
		DoubleAdder
		LongAccumulator
		LongAdder
		
## 类依赖分析器jdeps

- jdeps是一个很有用的命令行工具。它可以显示Java类的包级别或类级别的依赖。它接受一个.class文件，一个目录，或者一个jar文件作为输入。jdeps默认把结果输出到系统输出（控制台）上。

		jdeps org.springframework.core-3.0.5.RELEASE.jar	
			
## Java虚拟机（JVM）的新特性

- PermGen空间被移除了，取而代之的是Metaspace（JEP 122）。JVM选项-XX:PermSize与-XX:MaxPermSize分别被-XX:MetaSpaceSize与-XX:MaxMetaspaceSize所代替。
		
参考：<a href="https://www.javacodegeeks.com/2014/05/java-8-features-tutorial.html">Java 8 Features Tutorial </a>
github：<a href="https://github.com/Xchunguang/java8">示例</a>
官方文档：<a href="https://www.oracle.com/technetwork/java/javase/8-whats-new-2157071.html">JDK 8中的新功能</a>
	