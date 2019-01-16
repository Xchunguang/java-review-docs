# Java集合框架
Java中集合的概念就是存储和操作对象的一个组。Java提供了两种类型的容器。一种是Collection，存储元素的集合。另一种是Map，存储键值对。

## Collection

Collection是存储对象集合的顶层接口，表示一组对象的集合。JDK并没有提供Collection接口的任何实现类，而是提供了几个子接口，例如List、Set和Queue。并提供了相应的多个实现类。下面介绍常用实现类。

#### Set实现类

Set具有与Collection完全一样的接口，只是行为不同，Set不允许保存重复的元素。Set是存储无序的唯一的对象。而Set也可以为有序，Set的子接口SortedSet即为Set的有序集合

- TreeSet

  基于TreeMap的NavigableSet实现。元素按照其自然顺序排序，或者在创建时提供比较器，具体取决于使用的构造函数。TreeSet内部使用TreeMap实现
  
  该实现保证基础操作方法(add、remove和contains)的时间复杂度为log(n)
  
  TreeSet不是线程安全的，如果有不只一个线程对象操作TreeSet，则必须外部同步保证运行结果正确，另外还可以使用`Collections.synchronizedSortedSet`创建一个线程安全的Set：
  
      SortedSet s = Collections.synchronizedSortedSet(new TreeSet(...));
      
  该操作必须在创建阶段完成，防止意外不同步的访问。
  
  
- HashSet

  该实现类内部实现是封装HashMap对象进行实现，key值存储元素，而value存储一个静态对象PRESENT。无序，并且不保证顺序随时间的推移保持不变。该实现允许null值
  
  HashSet默认构造函数的初始容量为16，加载因子为0.75。HashSet的迭代时间与HashSet中元素的数目和内部HashMap容量的大小成正比，因此如果需要经常迭代的HashSet不要设置过大的容量或者设置了过小的加载因子。

  HashSet也不是线程安全的，也可以通过`Collections.synchronizedSet`将HashSet变成线程安全的Set。

- LinkedHashSet

  LinkedHashSet是HashSet的子类，作为有序的HashSet。该实现与HashSet不同的就是它内部维护了一个贯穿其所有条目的双向链表，该链表定义了元素的排序，即元素插入集合的顺序，注意如果有相同元素插入集合不会影响原集合元素的顺序。
  
  LinkedHashSet可以将现有的集合变成有序的集合，无论原始的集合是通过什么方式实现：
  
      void foo(Set s) {
         Set copy = new LinkedHashSet(s);
         ...
      }
      
  LinkedHashSet由于维护链表的额外费用，性能可能略低于HashSet的性能，但是与HashSet不同的是迭代LinkedHashSet的时间只与其中的元素的数目有关，而跟内部HashMap的容量无关。
  
  LinkedHashSet有两个影响其性能的参数：初始容量和加载因子。它们的定义与HashSet完全相同。但请注意，为此类选择过高的初始容量值的代价不如HashSet严重，因为此类的迭代次数不受容量影响。

#### List实现类

- ArrayList

  内部使用可调整大小的数组实现的List列表。包含List接口的所有操作，并且ArrayList可以包含null值。
  
  除了实现了List接口外，ArrayList提供了可以操作内部存储列表的数组的大小的方法，此类基本相当于Vector，但ArrayList不是线程安全的
  
  ArrayList的默认容量是10，并且可以随着元素的增加进行自动扩容。
  
  ArrayList不是线程安全的。但可以使用`List list = Collections.synchronizedList(new ArrayList(...));`创建线程安全的list
  
- LinkedList

  List和Deque接口的双链表实现。实现了所有List的方法，并允许所有元素（包括null）
  
  LinkedList的查询效率相对较低
  
- Vector

  Vector几乎就是线程安全的ArrayList，但是，Vector的大小可以根据需要增大或缩小，以适应在创建Vector之后添加和删除项目。
  
- ArrayList和Vector的区别
	- 都是用数组实现的
	- Vector是多线程安全的，而ArrayList不是，Vector在方法中用到很多synchronize进行修饰，所以Vector在效率上无法与ArrayList进行相比
	- 两个都是采用线性连续空间的存储元素，但是当空间不足的时候，两个类的增加方式是不一样的，Vector增加原来空间的一倍ArrayList增加原来空间的50%。
	- Vector可以设置增长因子，而ArrayList不可以

- ArrayList和LinkedList
	- ArrayList是实现了基于动态数组的数据结构，LinkedList是基于链表的数据结构
	- 对于随机访问的get和Set，ArrayList性能优于LinkedList，因为LinkedList要移动指针
	- 对于新增和删除操作add和remove，LinkedList比较占优势，因为Arraylist要移动数据

## Map
Map接口存储一组键值对象呢，提供key到value的映射。

#### Map实现类

- TreeMap 

  基于红黑树的NavigableMap实现。Map根据其键的自然顺序进行排序，或者通过在Map创建时提供的比较器进行排序，具体取决于使用的构造函数。
  
  红黑树：是一种自平衡的排序二叉树，树中每个节点的值，都大于或等于在他的左子树中的所有节点的值，并且小于或等于他右子树的所有节点的值，这确保红黑树在运行时可以快速的在树中查找和定位的所需节点，对于Treemap而言，由于他的底层采用红黑树来保存集合中的Entry，这意味着TreeMap添加元素，取出元素的性能都比HashMap低。
  
  TreeMap实现为containsKey，get，put和remove操作提供了有保证的log（n）时间成本。
  
  TreeMap不是线程安全的，不过可以通过：`SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...));`创建一个线程安全的Map
  
- HashMap

  HashMap 是一个散列表，它存储的内容是键值对(key-value)映射。
  
  该类实现了Map接口，根据键的HashCode值存储数据，具有很快的访问速度，最多允许一条记录的键为null，不支持线程同步。

  HashMap类大致相当于Hashtable，除了它是不同步的并且允许空值。
  
  HashMap不保证元素的顺序，且不能保证随着时间的推移顺序不变。
  
  对HashMap的迭代需要与HashMap实例的“容量”加上其大小（键 - 值映射的数量）成比例的时间。因此，如果迭代性能很重要，则不要将初始容量设置得太高（或负载因子太低）。
  
  HashMap默认加载因子为0.75，较高的值会减少空间开销，但会增加查找成本（反映在HashMap类的大多数操作中，包括get和put）。在设置其初始容量时，应考虑映射中的预期条目数及其负载因子，以便最小化重新散列操作的数量。如果初始容量大于最大条目数除以加载因子，则不会发生重新加载操作
  
  HashMap内部是由一个Entry类型数组进行存储的，Entry中有三个属性分别为：key，value，next。Next代表下一个Entry，当我们put一个元素的时候，HashMap首先找到这个键所对应下标，然后遍历，如果此下标无元素，则将此Entry放进此下标。如果此下标有元素，遍历所有的元素后，如发现有相同的Key则替换，如果没有key则将元素加入到链头。
  
- HashMap的HashTable的区别
	- HashTable继承自Dictiionary而HashMap继承自AbstractMap
	- HashTable中的put方法：方法是同步的，key、value不能为null，方法调用了key的hashcode方法，如果key==null，就会抛出空指针异常
	- HashMap中的put方法：方法是非同步的，方法允许key==null	，方法并没有对value进行任何调用，所以value可以为null

