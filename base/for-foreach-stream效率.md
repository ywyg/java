我们在开发时，有的人可能会认为stream的效率相比于for或者foreach应该更高一些，当时单纯的以为是不可靠的，所以针对这个问题，设计了以下这个小实验，结论表明：

当使用数据量小于100000（近似值），List 是数组类型时：

* foreach的效率最高
* for次之
* stream最差

当使用数据量小于100000（近似值），List 是节点类型时：

* foreach的效率最高
* stream次之
* for最差

当数据量更大的时候

* stream效率会慢慢提升，之到超过foreach
* for最差

```java
package exception;

import java.util.*;
import java.util.stream.Collectors;

/**
 * @author saijie.gao
 * @date 2021/11/19
 */
public class WhoIsFastStreamOrFor {


    public static long streamString(List<FastaModule> modules) {
        List<String> list = new ArrayList<>();
        Long startTime = System.nanoTime();
        list = modules.stream().map(FastaModule::getName).collect(Collectors.toList());
        Long execTime = System.nanoTime() - startTime;
        return execTime;
    }

    public static long forString(List<FastaModule> modules) {
        List<String> list = new ArrayList<>();
        Long startTime = System.nanoTime();
        for (FastaModule fastaModule : modules) {
            list.add(fastaModule.getName());
        }
        Long execTime = System.nanoTime() - startTime;
        return execTime;
    }

    public static long streamInteger(List<FastaModule> modules) {
        List<Integer> list = new ArrayList<>();
        Long startTime = System.nanoTime();
        list = modules.stream().map(FastaModule::getAge).collect(Collectors.toList());
        Long execTime = System.nanoTime() - startTime;
        return execTime;
    }

    public static long forInteger(List<FastaModule> modules) {
        List<Integer> list = new ArrayList<>();
        Long startTime = System.nanoTime();
        for (int i = 0; i < modules.size(); i++) {
            list.add(modules.get(i).getAge());
        }
        Long execTime = System.nanoTime() - startTime;
        return execTime;
    }

    public static long streamAddress(List<FastaModule> modules) {
        List<Address> list = new ArrayList<>();
        Long startTime = System.nanoTime();
        list = modules.stream().map(FastaModule::getAddress).collect(Collectors.toList());
        Long execTime = System.nanoTime() - startTime;
        return execTime;
    }

    public static long forAddress(List<FastaModule> modules) {
        List<Address> list = new ArrayList<>();
        Long startTime = System.nanoTime();
        for (FastaModule fastaModule : modules) {
            list.add(fastaModule.getAddress());
        }
        Long execTime = System.nanoTime() - startTime;
        return execTime;
    }


    public static List<FastaModule> makeData(int size) {
        List<FastaModule> list = new ArrayList<>();
        Random random = new Random();
        for (int i = 0; i < size; i++) {
            String name = UUID.randomUUID().toString().substring(0, 5);
            int age = random.nextInt(100);
            String schoolAddress = name.substring(0, 3);
            String homeAddress = name.substring(1, 3);
            FastaModule fastaModule = new FastaModule(name, age, new Address(schoolAddress, homeAddress));
            list.add(fastaModule);
        }
        return list;
    }


    public static void main(String[] args) {
        List<FastaModule> modules = makeData(10000);
        long streamStringTime = streamString(modules);
        long forStringTime = forString(modules);
        if (streamStringTime < forStringTime) {
            System.out.println("streamStringTime fast,than :" + (forStringTime - streamStringTime));
        } else {
            System.out.println("forStringTime fast,than :" + (streamStringTime - forStringTime));
        }

        long streamIntegerTime = streamInteger(modules);
        long forIntegerTime = forInteger(modules);
        if (streamIntegerTime < forIntegerTime) {
            System.out.println("streamIntegerTime fast,than :" + (forIntegerTime - streamIntegerTime));
        } else {
            System.out.println("forIntegerTime fast,than :" + (streamIntegerTime - forIntegerTime));
        }

        long streamAddressTime = streamAddress(modules);
        long forAddressTime = forAddress(modules);
        if (streamAddressTime < forAddressTime) {
            System.out.println("streamAddressTime fast,than :" + (forAddressTime - streamAddressTime));
        } else {
            System.out.println("forAddressTime fast,than :" + (streamAddressTime - forAddressTime));
        }
    }
}

```

