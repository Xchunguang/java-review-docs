# java11������
java11��һ������֧�ֵ�LTS�汾��ͬʱ�����˺ܶ����ԣ�java11���Ƴ��պø���java8��Ѹ��µ����ʱ�䣬����Ԥ��java11�����ڲ��õ��ϸ���˾������������
## ��������

�ٷ��ṩ����Ҫ�������������£�

![java11](https://github.com/Xchunguang/java-review-docs/blob/master/docs/img/java11.png)

## ��Ҫ�ı仯����Ϣ

- Applet��Web startӦ�ó�������Ҫ�Ĳ����ջ����java9���Ѿ������ã���java11��ʽɾ��
- ���û�в����ջ�����Ѵ�JDK 11֧�ֵ������б���ɾ����֧�ֵ���������������֡�
- ��windows��macOS�ϵ�JRE���Զ����½����ٿ���
- ��windows��maxOSϵͳ�а�װ֮ǰ�汾��JDKʱ��ѡ�Ƿ�װһ��JRE����java11�в��ڿ�ѡ��
- ��ǰ�汾�����ṩJRE��ֻ�ṩJDK������ʹ��jlink���ߴ���һ����С���Զ��������ʱ������
- javafx���ٰ�����jdk�У����ڴ�openjfx.io���Ե������ء�
- װ��JDK 7/8/9/10��Java Mission Control,���ٰ�����Oracle JDK��������һ�����������ء�
- ��ǰ�İ汾�����Ӣ�����������ġ������������������������(����),������,��䡣Ȼ��,��JDK 11֮��,�������¹������������������������(����),�������������﷭�벻���ṩ
- ����window�Ĵ����ʽ��`tar.gz`�ĵ�`zip`��ʽ��
- ����macOS�Ĵ����ʽ��`.app`��`.dmg`

## Unicode 10 

��������ƽ̨api֧��Unicode��׼��10.0�汾,�Դ�֧��Unicode 8.0.0��JDK 10����������JDK 11�����Unicode 9.0.0��10.0.0�汾

## HTTP Client (Standard) 

HTTP�ͻ�������Java 11�б�׼������Ϊ�˹�����һ���֣�λ��jdk.incubator.http���е���ǰ������API �ѱ�ɾ������Ϊjava.net.httpģ�顣

ʹ��java�Դ���http client�����������apache��HttpClient���߰�
    
    var request = HttpRequest.newBuilder()
            .uri(URI.create("https://github.com"))
            .GET()
            .build();
    var client = HttpClient.newHttpClient();
    // ͬ��
    HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
    System.out.println(response.body());
    // �첽
    client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
            .thenApply(HttpResponse::body)
            .thenAccept(System.out::println);

## �µ� Collection.toArray(IntFunction)

��java.util.Collection�ӿ��������һ���µ�Ĭ�Ϸ���toArray��IntFunction�����˷����������ϵ�Ԫ�ش��䵽�´�������������ʱ���͵����顣

    Set<String> items = Set.of("item1", "item2", "item3");
    //JDK11֮ǰ
    items.toArray(new String[items.size()]);
    //JDK11֮��
    items.toArray(size -> new String[size]);
    items.toArray(String[]::new);

## ZGC��һ���������ĵ��ӳٶ��������ռ���(ʵ��)

ZGC��Ϊʵ���Թ��ܰ������ڡ�Ҫ�������������Ҫ��-XX��+ UnlockExperimentalVMOptionsѡ����-XX��+ UseZGCѡ����ʹ�á�

ZGC�ĺ����ǲ��������ռ���������ζ����Java�̼߳���ִ�е�ͬʱ������з��صĹ�������ǣ�ѹ�������ô����ַ���������ȣ����⼫��������������ռ���Ӧ�ó�����Ӧʱ��ĸ���Ӱ�졣

#### ZGC�����Ŀ��
- ��ͣʱ�䲻����10 ms
- ��ͣʱ�䲻���Ӷѻ�live-set��С
- �����С�Ӽ����׵���ǧ���ֽڲ��ȵĶ�

#### ZGC�����ʵ��������������
- ֻ��Linux/x64ϵͳ�п���
- ��֧��ʹ��ѹ����oops��/��ѹ������㡣Ĭ������½���XX:+UseCompressedOops��-XX:+UseCompressedClassPointersѡ��������ǽ��������á�
- ��֧����ж�ء�Ĭ������½���-XX:+ClassUnloading�� -XX:+ClassUnloadingWithConcurrentMarkѡ��������ǽ��������á�
- ��֧�ֽ�ZGC��Graal���ʹ�á�

## Epsilon GC �޲����������ռ���

Epsilon GC���µ�ʵ�����޲��������ռ�����Epsilon GC�������ڴ���䣬���Ҳ�ʵ���κ��ڴ���ջ��ơ��������ܲ��Էǳ����ã�����������GC�ĳɱ�/������жԱȡ����������ڲ����з���ض����ڴ�ռ�ú��ڴ�ѹ�����ڼ�������£������ܶԷǳ����ݵ���ҵ�����ã������ڴ���ս���JVM��ֹʱ�����������ڵ�����Ӧ�ó����л�����һ���ӳٸĽ���

�ٷ��ĵ���<a href="https://www.oracle.com/technetwork/java/javase/11-relnote-issues-5012449.html#NewFeature">����</a>

