# java10������

java10��һ��С�������汾���ų�����һ�ٶ�������ԣ����ð汾����һ��LTS����֧�ֵİ汾�����Կ������˽����ԣ���java11ʱ����Ӧ���������С�

## �ֲ����������ƶ�

�����Կ���˵��java10���ܹ�ע�����ԣ��ù������ṩ��һЩ�﷨�ǣ����ƿ������飺

    var list = new ArrayList<String>();
    list.add("hello��world��");
    
������Խ��Խ��js�ˣ�����ͬ����java�Ͼ���ǿ���͵����ԣ��������������뱨��
    
    var list = new ArrayList<String>();
    list = new ArrayList<Integer>(); //���뱨��
    
���ֲ����������ƶ�ֻ���������³�����

- �ֲ������ĳ�ʼ��
- forѭ���ڲ����������
- ��ͳforѭ������������

���Ҿֲ������ƶϲ����������³���

- ��������
- ���캯������
- ������������
- �ֶ�
- ������ʽ

## G1�Ĳ���Full GC

���Ʋ���Full GC������G1�����ĵȴ�ʱ�䣬G1�����ռ�����ּ�ڱ��������ļ���,���ǵ����������޷��㹻��ػ����ڴ�ʱ������������Full GC��

�ɵ�G1��Full GCʵ��ʹ�õ��̱߳��-���-�����㷨�����µ�Full GC���л�����������ʹ��������ͻ�ϼ�����ͬ�����Ĳ��й����̡߳�

## �µĴ������ɱ伯�ϵ�API

java10�ṩ�˼���API�������ɱ伯�ϣ�

#### ���Ѿ����ڵ�ʵ���������ɱ伯�ϣ�

- List.copyOf
- Set.copyOf
- Map.copyOf

#### Stream���е�Collectors������������������Щ����ʹ���е�Ԫ���ռ���һ�����ɸı�ļ���

- toUnmodifiableList
- toUnmodifiableSet
- toUnmodifiableMap 

## Optional.orElseThrow() ����

Optional��������µķ���orElseThrow()����ѡ�滻ԭ�е�get����

## ��֤��(Root Certificates)

Ϊjdk�ṩ��һ��Ĭ�ϵ�CA֤��

## javadoc֧�ֶ����ʽ��

javadoc tool�ṩ��һ���µ�javadoc������ѡ��--add-stylesheet��֧�����ɵ��ĵ��еĶ����ʽ���ʹ��


java10��������<a href="https://www.oracle.com/technetwork/java/javase/10-relnote-issues-4108729.html#NewFeature">�ٷ��ĵ�</a>
