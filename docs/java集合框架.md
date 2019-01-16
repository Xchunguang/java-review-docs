# Java���Ͽ��
Java�м��ϵĸ�����Ǵ洢�Ͳ��������һ���顣Java�ṩ���������͵�������һ����Collection���洢Ԫ�صļ��ϡ���һ����Map���洢��ֵ�ԡ�

## Collection

Collection�Ǵ洢���󼯺ϵĶ���ӿڣ���ʾһ�����ļ��ϡ�JDK��û���ṩCollection�ӿڵ��κ�ʵ���࣬�����ṩ�˼����ӽӿڣ�����List��Set��Queue�����ṩ����Ӧ�Ķ��ʵ���ࡣ������ܳ���ʵ���ࡣ

#### Setʵ����

Set������Collection��ȫһ���Ľӿڣ�ֻ����Ϊ��ͬ��Set���������ظ���Ԫ�ء�Set�Ǵ洢�����Ψһ�Ķ��󡣶�SetҲ����Ϊ����Set���ӽӿ�SortedSet��ΪSet�����򼯺�

- TreeSet

  ����TreeMap��NavigableSetʵ�֡�Ԫ�ذ�������Ȼ˳�����򣬻����ڴ���ʱ�ṩ�Ƚ���������ȡ����ʹ�õĹ��캯����TreeSet�ڲ�ʹ��TreeMapʵ��
  
  ��ʵ�ֱ�֤������������(add��remove��contains)��ʱ�临�Ӷ�Ϊlog(n)
  
  TreeSet�����̰߳�ȫ�ģ�����в�ֻһ���̶߳������TreeSet��������ⲿͬ����֤���н����ȷ�����⻹����ʹ��`Collections.synchronizedSortedSet`����һ���̰߳�ȫ��Set��
  
      SortedSet s = Collections.synchronizedSortedSet(new TreeSet(...));
      
  �ò��������ڴ����׶���ɣ���ֹ���ⲻͬ���ķ��ʡ�
  
  
- HashSet

  ��ʵ�����ڲ�ʵ���Ƿ�װHashMap�������ʵ�֣�keyֵ�洢Ԫ�أ���value�洢һ����̬����PRESENT�����򣬲��Ҳ���֤˳����ʱ������Ʊ��ֲ��䡣��ʵ������nullֵ
  
  HashSetĬ�Ϲ��캯���ĳ�ʼ����Ϊ16����������Ϊ0.75��HashSet�ĵ���ʱ����HashSet��Ԫ�ص���Ŀ���ڲ�HashMap�����Ĵ�С�����ȣ���������Ҫ����������HashSet��Ҫ���ù�����������������˹�С�ļ������ӡ�

  HashSetҲ�����̰߳�ȫ�ģ�Ҳ����ͨ��`Collections.synchronizedSet`��HashSet����̰߳�ȫ��Set��

- LinkedHashSet

  LinkedHashSet��HashSet�����࣬��Ϊ�����HashSet����ʵ����HashSet��ͬ�ľ������ڲ�ά����һ���ᴩ��������Ŀ��˫����������������Ԫ�ص����򣬼�Ԫ�ز��뼯�ϵ�˳��ע���������ͬԪ�ز��뼯�ϲ���Ӱ��ԭ����Ԫ�ص�˳��
  
  LinkedHashSet���Խ����еļ��ϱ������ļ��ϣ�����ԭʼ�ļ�����ͨ��ʲô��ʽʵ�֣�
  
      void foo(Set s) {
         Set copy = new LinkedHashSet(s);
         ...
      }
      
  LinkedHashSet����ά������Ķ�����ã����ܿ����Ե���HashSet�����ܣ�������HashSet��ͬ���ǵ���LinkedHashSet��ʱ��ֻ�����е�Ԫ�ص���Ŀ�йأ������ڲ�HashMap�������޹ء�
  
  LinkedHashSet������Ӱ�������ܵĲ�������ʼ�����ͼ������ӡ����ǵĶ�����HashSet��ȫ��ͬ������ע�⣬Ϊ����ѡ����ߵĳ�ʼ����ֵ�Ĵ��۲���HashSet���أ���Ϊ����ĵ���������������Ӱ�졣

#### Listʵ����

- ArrayList

  �ڲ�ʹ�ÿɵ�����С������ʵ�ֵ�List�б�����List�ӿڵ����в���������ArrayList���԰���nullֵ��
  
  ����ʵ����List�ӿ��⣬ArrayList�ṩ�˿��Բ����ڲ��洢�б������Ĵ�С�ķ�������������൱��Vector����ArrayList�����̰߳�ȫ��
  
  ArrayList��Ĭ��������10�����ҿ�������Ԫ�ص����ӽ����Զ����ݡ�
  
  ArrayList�����̰߳�ȫ�ġ�������ʹ��`List list = Collections.synchronizedList(new ArrayList(...));`�����̰߳�ȫ��list
  
- LinkedList

  List��Deque�ӿڵ�˫����ʵ�֡�ʵ��������List�ķ���������������Ԫ�أ�����null��
  
  LinkedList�Ĳ�ѯЧ����Խϵ�
  
- Vector

  Vector���������̰߳�ȫ��ArrayList�����ǣ�Vector�Ĵ�С���Ը�����Ҫ�������С������Ӧ�ڴ���Vector֮����Ӻ�ɾ����Ŀ��
  
- ArrayList��Vector������
	- ����������ʵ�ֵ�
	- Vector�Ƕ��̰߳�ȫ�ģ���ArrayList���ǣ�Vector�ڷ������õ��ܶ�synchronize�������Σ�����Vector��Ч�����޷���ArrayList�������
	- �������ǲ������������ռ�Ĵ洢Ԫ�أ����ǵ��ռ䲻���ʱ������������ӷ�ʽ�ǲ�һ���ģ�Vector����ԭ���ռ��һ��ArrayList����ԭ���ռ��50%��
	- Vector���������������ӣ���ArrayList������

- ArrayList��LinkedList
	- ArrayList��ʵ���˻��ڶ�̬��������ݽṹ��LinkedList�ǻ�����������ݽṹ
	- ����������ʵ�get��Set��ArrayList��������LinkedList����ΪLinkedListҪ�ƶ�ָ��
	- ����������ɾ������add��remove��LinkedList�Ƚ�ռ���ƣ���ΪArraylistҪ�ƶ�����

## Map
Map�ӿڴ洢һ���ֵ�����أ��ṩkey��value��ӳ�䡣

#### Mapʵ����

- TreeMap 

  ���ں������NavigableMapʵ�֡�Map�����������Ȼ˳��������򣬻���ͨ����Map����ʱ�ṩ�ıȽ����������򣬾���ȡ����ʹ�õĹ��캯����
  
  ���������һ����ƽ������������������ÿ���ڵ��ֵ�������ڻ�����������������е����нڵ��ֵ������С�ڻ�����������������нڵ��ֵ����ȷ�������������ʱ���Կ��ٵ������в��ҺͶ�λ������ڵ㣬����Treemap���ԣ��������ĵײ���ú���������漯���е�Entry������ζ��TreeMap���Ԫ�أ�ȡ��Ԫ�ص����ܶ���HashMap�͡�
  
  TreeMapʵ��ΪcontainsKey��get��put��remove�����ṩ���б�֤��log��n��ʱ��ɱ���
  
  TreeMap�����̰߳�ȫ�ģ���������ͨ����`SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...));`����һ���̰߳�ȫ��Map
  
- HashMap

  HashMap ��һ��ɢ�б����洢�������Ǽ�ֵ��(key-value)ӳ�䡣
  
  ����ʵ����Map�ӿڣ����ݼ���HashCodeֵ�洢���ݣ����кܿ�ķ����ٶȣ��������һ����¼�ļ�Ϊnull����֧���߳�ͬ����

  HashMap������൱��Hashtable���������ǲ�ͬ���Ĳ��������ֵ��
  
  HashMap����֤Ԫ�ص�˳���Ҳ��ܱ�֤����ʱ�������˳�򲻱䡣
  
  ��HashMap�ĵ�����Ҫ��HashMapʵ���ġ��������������С���� - ֵӳ����������ɱ�����ʱ�䡣��ˣ�����������ܺ���Ҫ����Ҫ����ʼ�������õ�̫�ߣ���������̫�ͣ���
  
  HashMapĬ�ϼ�������Ϊ0.75���ϸߵ�ֵ����ٿռ俪�����������Ӳ��ҳɱ�����ӳ��HashMap��Ĵ���������У�����get��put�������������ʼ����ʱ��Ӧ����ӳ���е�Ԥ����Ŀ�����为�����ӣ��Ա���С������ɢ�в����������������ʼ�������������Ŀ�����Լ������ӣ��򲻻ᷢ�����¼��ز���
  
  HashMap�ڲ�����һ��Entry����������д洢�ģ�Entry�����������Էֱ�Ϊ��key��value��next��Next������һ��Entry��������putһ��Ԫ�ص�ʱ��HashMap�����ҵ����������Ӧ�±꣬Ȼ�������������±���Ԫ�أ��򽫴�Entry�Ž����±ꡣ������±���Ԫ�أ��������е�Ԫ�غ��緢������ͬ��Key���滻�����û��key��Ԫ�ؼ��뵽��ͷ��
  
- HashMap��HashTable������
	- HashTable�̳���Dictiionary��HashMap�̳���AbstractMap
	- HashTable�е�put������������ͬ���ģ�key��value����Ϊnull������������key��hashcode���������key==null���ͻ��׳���ָ���쳣
	- HashMap�е�put�����������Ƿ�ͬ���ģ���������key==null	��������û�ж�value�����κε��ã�����value����Ϊnull

