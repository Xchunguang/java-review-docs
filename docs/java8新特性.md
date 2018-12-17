# java8������

![java8](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java8.png)

## Lambda���ʽ


- Lambda����Ѻ�����Ϊһ�������Ĳ�����������Ϊ�������ݽ������У������߰Ѵ��뿴������

- ��򵥵���ʽ�У�һ��lambda�������ö��ŷָ��Ĳ����б��C>�����뺯���������ֱ�ʾ��

		Arrays.asList( 1, 2, 3 ).forEach( e -> System.out.println( e ) );

- Lambda����������ĳ�Ա������ֲ������������Щ��������final�Ļ������ǻᱻ������תΪfinal������Ч�ʸ��ߣ���

		String str = ",";			//�ȼ��ڣ�final String str = ",";
		Arrays.asList( "a", "b", "c" ).forEach( 
		    ( String e ) -> System.out.print( e + str ) );	
		    


- Lambda���ܻ᷵��һ��ֵ������ֵ������Ҳ���ɱ������Ʋ�����ġ����lambda�ĺ�����ֻ��һ�еĻ�����ôû�б�Ҫ��ʽʹ��return���:

		List<Integer> list = Arrays.asList( 4, 2, 1,3);
		list.sort( ( e1, e2 ) -> e1.compareTo( e2 ) );
		list.forEach(e->System.out.println(e));	
					
			
## ����ʽ�ӿ�

- ���ʹ���еĺ����Ѻõ�֧��lambda��java8��ȡ�����Ӻ���ʽ�ӿڵĸ��
- ����ʽ�ӿھ���һ��ֻ��һ����������ͨ�ӿڣ�������ʽ��ת����lambda���ʽ���������ⷽ����Ĭ�Ϸ�������̬�����Լ��̳���Object���һЩ������toString(),equels()�ȣ�
- ��ʹ���У�����ʽ�ӿ����׳�������ӿ��б���������һ����������ô�ӿڽ������Ǻ���ʽ�ӿڣ����±���ʧ�ܡ�Ϊ��Java8����ע��@FunctionalInterface��ע��defaultĬ�Ϸ����;�̬��������Ӱ�캯��ʽ�ӿڡ�

		@FunctionalInterface
		public interface Runnable {
		    public abstract void run();
		}

- java8����Ĭ�Ϸ����;�̬������չ�ӿڵ�������

		public interface Convert {

			//���԰�����̬����
			public static void hasStaticMethod(){
				System.out.println("������̬����");
			}
			
			default void hasDefaultMethod(){
				System.out.println("����Ĭ�Ϸ���");
			}
		}
		
- ����ʽ�ӿ�ʾ��

		@FunctionalInterface
		public interface Convert<F,T> {
		
			T convert(F from);
			
			//���԰�����̬����
			public static void hasStaticMethod(){
				System.out.println("������̬����");
			}
			
			default void hasDefaultMethod(){
				System.out.println("����Ĭ�Ϸ���");
			}
		}
		
		public class ConvertMain {
		public static void main(String[] args) {
	
			Convert<String, Integer> converter = (from) -> Integer.valueOf(from);
			Integer converted = converter.convert("123");
			System.out.println(converted);
			converter.hasDefaultMethod();
			Convert.hasStaticMethod();
			
			// Function<T, R> -T��Ϊ���룬���ص�R��Ϊ���
			Function<String,String> function = (x) -> {System.out.print(x+": ");return "Function";};
			System.out.println(function.apply("hello world"));
	
			//Predicate<T> -T��Ϊ���룬���ص�booleanֵ��Ϊ���
			Predicate<String> pre = (x) ->{System.out.print(x);return false;};
			System.out.println(": "+pre.test("hello World"));
	
			//Consumer<T> - T��Ϊ���룬ִ��ĳ�ֶ�����û�з���ֵ
			Consumer<String> con = (x) -> {System.out.println(x);};
			con.accept("hello world");
	
			//Supplier<T> - û���κ����룬����T
			Supplier<String> supp = () -> {return "Supplier";};
			System.out.println(supp.get());
			//BinaryOperator<T> -����T��Ϊ���룬����һ��T��Ϊ��������ڡ�reduce������������
			BinaryOperator<String> bina = (x,y) ->{System.out.print(x+" "+y);return "BinaryOperator";};
			System.out.println("  "+bina.apply("hello ","world"));
			
		}
	}
				
						
			
## �������ã�::��

- ��ֱ����������Java������ʵ�����ķ�������������lambda����ʹ��

- ��һ�ַ��������ǹ��������ã������﷨��Class::new�����߸�һ���Class< T >::new����ע�⹹����û�в�����

		final Car car = Car.create( Car::new );
		final List< Car > cars = Arrays.asList( car );

- �ڶ��ַ��������Ǿ�̬�������ã������﷨��Class::static_method����ע�������������һ��Car���͵Ĳ�����

		cars.forEach( Car::collide );
		
- �����ַ����������ض�����������ķ������ã������﷨��Class::method����ע�⣬�������û�в�����

		cars.forEach( Car::repair );
		
- ��󣬵����ַ����������ض�����ķ������ã������﷨��instance::method����ע�⣬�����������һ��Car���͵Ĳ���

		final Car police = Car.create( Car::new );
		cars.forEach( police::follow );
		
		private void sort(){
		    List<Commodity> commodities=new ArrayList<>();
		    //java 8֮ǰ
		    commodities.sort(new Comparator<Commodity>() {
		    @Override
		    public int compare(Commodity o1, Commodity o2) {
		            return o1.getPrice()-o2.getPrice();
		        }
		    });
		    //java 8 lambda��д��
		    commodities.sort((Commodity o1,Commodity o2)->o1.getPrice()-o2.getPrice());
		    //java 8 ����Ӧ�õ�д��
		    commodities.sort(Comparator.comparing(Commodity::getPrice));
		}

	
## �ظ�ע��

- java5��ʼ����ע����ƣ�Ȼ����ͬע����ͬ���ĵط�ֻ������һ�Σ�java8�����ظ�ע�⡣

- �ظ�ע����Ʊ��������@Repeatableע�⡣��ʵ���Ǳ��������ɵĸı�
	
	
## ���õ������ƶ�

- Java 8�������Ʋⷽ�����˺ܴ����ߡ��ںܶ�����£������������Ʋ��ȷ���Ĳ������ͣ���������ʹ��������ࡣ

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

## ��չע���֧��

- Java 8��չ��ע��������ġ����ڼ�������Ϊ�κζ������ע�⣺�ֲ������������ࡢ������ӿڵ�ʵ�֣��������쳣Ҳ�������ע�⡣

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
		
	ElementType.TYPE_USE��ElementType.TYPE_PARAMETER����������ӵ����������ʵ���ע�������ĵ�Ԫ�����͡�
		
## Optional

- ������⣬Ϊ���java�г����Ŀ�ָ���쳣���³����޷���������

- Optionalʵ�����Ǹ������������Ա�������T��ֵ�����߽�������null��Optional�ṩ�ܶ����õķ������������ǾͲ�����ʽ���п�ֵ��⡣

		//����Ϊ��ֵ����ֵΪ��ʱҲ���ᱨ��
		Optional< String > name = Optional.ofNullable( null );
		System.out.println( "name�Ƿ���ֵ " + name.isPresent() );        
		System.out.println( "�������Ϊ��: " + name.orElseGet( () -> "��������" ) ); 
		System.out.println( name.map( s -> "����Ϊ�� " + s  ).orElse( "����Ϊ��" ) );
		
		//������Ϊ��ֵ����ֵΪ��ʱ�ᱨ��
		Optional< String > firstName = Optional.of( "name" );
		System.out.println( "firstName�Ƿ���ֵ " + firstName.isPresent() );        
		System.out.println( "�������Ϊ��: " + firstName.orElseGet( () -> "��������" ) ); 
		System.out.println( firstName.map( s -> "����Ϊ�� " + s  ).orElse( "����Ϊ��" ) );
		
	Optional<a href="https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html">�ĵ�</a>
		
## Stream
java8��stream�ǶԼ��ϣ�Collection)����Ĺ����ϵ���ǿ���ṩ�Լ��ϸ�Ч�ľۺϺ͹��˲�����֧�ִ��кͲ�������ģʽ���л�۲�����stream���Ǽ���Ԫ�أ������ᱣ�����ݣ���ͬ������һ��������Ĳ��������ģ�����һ�μ��þ��������������ͬ���ǣ�stream���Խ��в��в�����

#### stream�Ĺ��ɣ�
  ʹ�����Ĺ��̻�����Ϊ��������Դ��source��->����ת��->ִ�н����ÿ��ִ��ת����ԭ�е�stream���䣬����һ���µ�stream����ʹ�����γ��������һ���ܵ���

#### ����Դ����Դ:
- ���϶���Collection������
- Collection.stream()
- Collection.parallelStream()
- Arrays.stream(T array)
- Stream.of()
- ...

#### �������ֲ�����
- Intermediate:һ�������Խ����������ת��������Ŀ�ľ��Ǵ�����ִ��ĳ�ֹ��˻�mapӳ��ȷ���һ��������������һ������ʹ�á�����������Ƕ��Ի��ģ�����˵�����������������û�п�ʼ���ı�����map/filter/distinct/sorted/peek/limit/skip/parallel/sequential/unordered
- Terminal�����������һ����ֻ����һ���ò�������ִ�иò����������൱�ڱ�ʹ�ù��ˣ������ٲ��������������һ�����һ�������forEach/toArray/reduce/collect/min/max/count/anyMatch/allMatch/noneMatch/findFirst/findAny/limit
- short-circuiting������һ�����޴��stream�������޵�ʱ���ڼ��������򷵻�һ���µ�stream��anyMatch/allMatch/noneMatch/findFirst/findAny/limit
  
#### ����������
  - map:ӳ�䣬map���ɵ���1:1��ӳ�䣬ÿ�������Ԫ�ض��ᰴ�չ���ת��������һ�ֹ����Ԫ�ء�
  
        /**
         * ���ռ����ռ�ĳ���Լ���
         */
        public Set<String> collectDate(List<UserVO> userList){
          return userList.stream().map(UserVO::getUserName).collect(Collectors.toSet());
        }
        
  - flatMap:һ�Զ�ӳ�䣬ͨ�����ںϲ�������
      
	      Stream<List<Integer>> inputStream = Stream.of(
	       Arrays.asList(1),
	       Arrays.asList(2, 3),
	       Arrays.asList(4, 5, 6)
	       );
	      Stream<Integer> outputStream = inputStream.
	      flatMap((childList) -> childList.stream());
      
  - filter:filter��ԭʼ��stream����ĳ���ԣ�ͨ�����Ե�Ԫ���γ�һ���µ�stream
    
          /**
           * �򵥹��˲����б�
           * @param userList
           * @param age
           * @return
           */
          public List<UserVO> simpleFilterFindList(List<UserVO> userList,int age){
            return userList.stream().filter((userVO)->userVO.getAge()==age).collect(Collectors.toList());
          }
          
  - forEach:����lambda���ʽ��Ȼ��������ÿ��Ԫ��ִ�б��ʽ
  - findFirst:�ò�����Terminal��������short-circuiting���������Ƿ��ؼ��ϵĵ�һ��Ԫ�ص�Optional
    
        /**
         * ���ҵ�һ��
         * @param userList
         * @return
         */
        public UserVO simpleFilter(List<UserVO> userList,String userName){
          return userList.stream().filter((userVO)->userVO.getUserName().equals(userName)).findFirst().orElse(null);
        }
        
  - reduce���ò�����Ҫ�ǽ�streamԪ������������ṩһ����ʼֵ��Ȼ�����չ����ǰ���stream�ĸ���Ԫ�ص���ϡ�
  
	      // �ַ������ӣ�concat = "ABCD"
	      String concat = Stream.of("A", "B", "C", "D").reduce("", String::concat); 
	      // ����Сֵ��minValue = -3.0
	      double minValue = Stream.of(-1.5, 1.0, -3.0, -2.0).reduce(Double.MAX_VALUE, Double::min); 
	      // ��ͣ�sumValue = 10, ����ʼֵ
	      int sumValue = Stream.of(1, 2, 3, 4).reduce(0, Integer::sum);
	      // ��ͣ�sumValue = 10, ����ʼֵ
	      sumValue = Stream.of(1, 2, 3, 4).reduce(Integer::sum).get();
	      // ���ˣ��ַ������ӣ�concat = "ace"
	      concat = Stream.of("a", "B", "c", "D", "e", "F").
	       filter(x -> x.compareTo("Z") > 0).
	       reduce("", String::concat);
       
  - limit/skip:limit�Ƿ���stream��ǰn��Ԫ�أ���skip���Ƕ���stream��ǰn��Ԫ��
  
	      /**
	       * limit ��ȡǰn��Ԫ��
	       * skip�Ƕ���ǰn��Ԫ��
	       * @param userList
	       * @return
	       */
	      public List<UserVO> limitAndSkip(List<UserVO> userList){
	        return userList.stream().limit(10).skip(2).collect(Collectors.toList());
	      }
      
  - sorted:��stream�����򣬺ô��ǿ����ȶ�stream��������������map,filter,limit�Ȳ����ٽ�������
  
        /**
         * ���򼯺�
         * @param userList
         * @return
         */
        public List<UserVO> sortList(List<UserVO> userList){
          return userList.stream().sorted((t1,t2) -> t1.getUserName().compareTo(t2.getUserName())).collect(Collectors.toList());
        }
  
  - min/max/distinct:������ȡ���ֵ��Сֵ��ȥ�ز���
  - Match��allMatch/anyMatch/noneMatch,��Щ���ǲ���Ҫ����������Ԫ�ؾͿ��Է��ؽ���ģ�����allMatch��һ��������ͻ᷵��false��
  
        /**
         * ����Ƿ�����Ԫ�ض�ƥ������
         * @param userList
         * @return
         */
        public boolean matchAllUser(List<UserVO> userList){
          return userList.stream().allMatch((userVO)->userVO.getAge()>=0);
        }

  - �Զ�����������Stream.generate
    ͨ��Supplier�ӿڿ����Զ����������ɣ����������������������ǰ�󱣳�ĳ�ֹ�ϵ����������ʹ��limit�������Ĵ�С��
    
          /**
           * �Զ�����
           */
          public void generateStream(){
            Random ran = new Random();
            Supplier<Integer> stream = ran::nextInt;
            Stream.generate(stream).limit(10).forEach((value) -> System.out.println(value));
          }

  - Stream.iterate:������reduce���ƣ�����һ����ʼֵ����һ������������ִ����ȥ��Ҳ��Ҫʹ��limit���п������Ĵ�С��
  
  
  - groupingBy:����
  
      	/**
         * �����Ϸ���
         * �����������
         * @param userList
         * @return
         */
        public Map<Integer,List<UserVO>> groupUserVO(List<UserVO> userList){
          return userList.stream().collect(Collectors.groupingBy(userVO -> userVO.getAge()));
        }
        
  - partitioningBy:�ָ�
  
      	/**
         * �ָ����ݼ���������ָ�
         * @param userList
         * @return
         */
        public Map<Boolean,List<UserVO>> splitUserList(List<UserVO> userList){
          return userList.stream().collect(Collectors.partitioningBy(userVO -> userVO.getAge()>15));
        }


#### �ܽ�
- �������д洢������һ�����ݽṹ��Ҳ�����޸ĵײ������Դ��ֻ������µ���
- Intermediate������Զ�Ƕ��Եģ��ܶ����������ӳ٣�ֱ��֪���������ݲŻῪʼ
- stream���Բ��д��������д���̴߳���
- stream���������޵ģ���һ����Ҫlimit��findFirst��short-curcutiting�������ٵõ����

## Date/Time API (JSR 310)

- Joda-Time����һ�����滻��׼����/ʱ�䴦���ҹ��ܷǳ�ǿ���Java API

- <a href="https://www.javacodegeeks.com/2014/03/whats-new-in-java-8-date-api.html">�µ�java.time�����������д������ڣ�ʱ�䣬����/ʱ�䣬ʱ����ʱ�̣�instants�������̣�during����ʱ�ӣ�clock���Ĳ���</a>

- Clock�࣬��ͨ��ָ��һ��ʱ����Ȼ��Ϳ��Ի�ȡ����ǰ��ʱ�̣�������ʱ�䡣Clock�����滻System.currentTimeMillis()��TimeZone.getDefault()��
	
		final Clock clock = Clock.systemUTC();
		System.out.println( clock.instant() );
		System.out.println( clock.millis() );
		
- LocaleDate��LocalTime��LocaleDateֻ����ISO-8601��ʽ����ʱ����Ϣ�����ڲ��֡���Ӧ�ģ�LocaleTimeֻ����ISO-8601��ʽ����ʱ����Ϣ��ʱ�䲿�֡�LocaleDate��LocalTime�����Դ�Clock�еõ�

		final LocalDate date = LocalDate.now();
		final LocalDate dateFromClock = LocalDate.now( clock );
		         
		System.out.println( date );
		System.out.println( dateFromClock );
		         
		// Get the local date and local time
		final LocalTime time = LocalTime.now();
		final LocalTime timeFromClock = LocalTime.now( clock );
		         
		System.out.println( time );
		System.out.println( timeFromClock );
		
- LocaleDateTime��LocaleDate��LocaleTime�Ĺ��ܺϲ������������е���ISO-8601��ʽ��ʱ����Ϣ��������ʱ�䡣

		final LocalDateTime datetime = LocalDateTime.now();
		final LocalDateTime datetimeFromClock = LocalDateTime.now( clock );
		         
		System.out.println( datetime );
		System.out.println( datetimeFromClock );
		
- �������Ҫ�ض�ʱ��������/ʱ�䣬����ʹ��ZonedDateTime��������ISO-8601��ʽ�߾���ʱ����Ϣ��������ʱ��

		final ZonedDateTime zonedDatetime = ZonedDateTime.now();
		final ZonedDateTime zonedDatetimeFromClock = ZonedDateTime.now( clock );
		final ZonedDateTime zonedDatetimeFromZone = ZonedDateTime.now( ZoneId.of( "America/Los_Angeles" ) );
		         
		System.out.println( zonedDatetime );
		System.out.println( zonedDatetimeFromClock );
		System.out.println( zonedDatetimeFromZone );
		
		
- Duration�ࣺDurationʹ�����������ڼ�Ĳ���ʮ�ּ򵥡�

		final LocalDateTime from = LocalDateTime.of( 2014, Month.APRIL, 16, 0, 0, 0 );
		final LocalDateTime to = LocalDateTime.of( 2015, Month.APRIL, 16, 23, 59, 59 );
		 
		final Duration duration = Duration.between( from, to );
		System.out.println( "Duration in days: " + duration.toDays() );
		System.out.println( "Duration in hours: " + duration.toHours() );
			
## JavaScript����Nashorn

- Nashorn��һ���µ�JavaScript��������Java 8һ������������������JVM�Ͽ�������ĳЩJavaScriptӦ�á�Nashorn����javax.script.ScriptEngine����һ��ʵ�֣�������������ѭ��ͬ�Ĺ�������Java��JavaScript�໥���á�

		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName( "JavaScript" );
		         
		System.out.println( engine.getClass().getName() );
		System.out.println( "Result:" + engine.eval( "function f() { return 1; }; f() + 1;" ) );
			
			
## Base64��Java 8�У�Base64�����Ѿ���ΪJava���ı�׼��

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

Base64��ͬʱ���ṩ�˶�URL��MIME�Ѻõı��������������Base64.getUrlEncoder() / Base64.getUrlDecoder(), Base64.getMimeEncoder() / Base64.getMimeDecoder()����
			
## ���У�parallel������

- Java 8�����˴������·�������������в��д�������˵������Ҫ����parallelSort()��������Ϊ�������ڶ�˻����ϼ����������������ٶȡ�


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
			
����Ĵ���Ƭ��ʹ����parallelSetAll()��������һ����20000��Ԫ�ص�������������ֵ��Ȼ�󣬵���parallelSort����������������ȴ�ӡ��ǰ10��Ԫ�ص�ֵ��֮���������������
			
			
## ������Concurrency��

- ������Stream������lambda�Ļ���֮�ϣ���java.util.concurrent.ConcurrentHashMap�м�����һЩ�·�����֧�־ۼ�������ͬʱҲ��java.util.concurrent.ForkJoinPool���м�����һЩ�·�����֧�ֹ�����Դ�أ�common pool��

- ������java.util.concurrent.locks.StampedLock���ṩһֱ��������������������������ģ�������ƶ�д������������Ϊ�ǲ�̫������java.util.concurrent.locks.ReadWriteLock�������ߣ���

	��java.util.concurrent.atomic���л�������������Щ�ࣺ
	
		DoubleAccumulator
		DoubleAdder
		LongAccumulator
		LongAdder
		
## ������������jdeps

- jdeps��һ�������õ������й��ߡ���������ʾJava��İ�������༶���������������һ��.class�ļ���һ��Ŀ¼������һ��jar�ļ���Ϊ���롣jdepsĬ�ϰѽ�������ϵͳ���������̨���ϡ�

		jdeps org.springframework.core-3.0.5.RELEASE.jar	
			
## Java�������JVM����������

- PermGen�ռ䱻�Ƴ��ˣ�ȡ����֮����Metaspace��JEP 122����JVMѡ��-XX:PermSize��-XX:MaxPermSize�ֱ�-XX:MetaSpaceSize��-XX:MaxMetaspaceSize�����档
		
�ο���<a href="https://www.javacodegeeks.com/2014/05/java-8-features-tutorial.html">Java 8 Features Tutorial </a>
github��<a href="https://github.com/Xchunguang/java8">ʾ��</a>
�ٷ��ĵ���<a href="https://www.oracle.com/technetwork/java/javase/8-whats-new-2157071.html">JDK 8�е��¹���</a>
	