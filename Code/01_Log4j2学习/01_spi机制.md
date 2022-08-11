# SPI机制
[TOC]

## 一、java的spi介绍

> java的spi在`META-INF/services`下面创建一个全路径文件。然后用ServiceLoader加载。

### 1. 使用

1. 在resource下面创建文件名为：`com.testdt.Pay`的文件，文件内容为：
``` text
com.testdt.AliPay
com.testdt.TxPay
```
2. 在路径`com.testdt`创建`Pay`, `AliPay`, `TxPay`三个类。
3. 加载
``` java
public static void main(String[] args) {
    ServiceLoader<Pay> pays = ServiceLoader.load(Pay.class);
    Iterator<Pay> iterator = pays.iterator();
    while (iterator.hasNext()){
        Pay pay = iterator.next();
        pay.pay();
    }
}
```
> 可以实现解耦，三方服务，自定义自己的组件。只需要提供统一接口，三方实现，然后具体逻辑走三方。
> 但是在jdk中，只能一次性加载，并且线程不安全。



### 2. 源码分析

1. 在ServiceLoader里面有`LazyIterator`继承迭代器。load方法会传入Classloader和Class字节。
```java
public void reload() {
    providers.clear();         
    lookupIterator = new LazyIterato(service, loader);
}
```
2. 当调用`iterator.hasNext()`的时候：
```java
private boolean hasNextService() {
    if (nextName != null) {
        return true;
    }
    if (configs == null) {
        try {
            //private static final String PREFIX = "META-INF/services/";
            // 获取文件全路径
            String fullName = PREFIX + service.getName();
            if (loader == null)
                configs = ClassLoader.getSystemResources(fullName);
            else
                // Enumeration<URL>[] tmp[0] = Classloader.getResources(name);
                configs = loader.getResources(fullName);
        } catch (IOException x) {
            fail(service, "Error locating configuration files", x);
        }
    }
    while ((pending == null) || !pending.hasNext()) {
        if (!configs.hasMoreElements()) {
            return false;
        }
        // BufferedReader r = new BufferedReader(new InputStreamReader(in, "utf-8"));
        // while ((lc = parseLine(service, u, r, lc, names)) >= 0); 装载下一个类路径
        pending = parse(service, configs.nextElement());
    }
    nextName = pending.next();
    return true;
}
```

```java
// 实例化类，懒加载，当使用时才会实例化该类。
private S nextService() {
    if (!hasNextService())
        throw new NoSuchElementException();
    String cn = nextName;
    nextName = null;
    Class<?> c = null;
    c = Class.forName(cn, false, loader);
    S p = service.cast(c.newInstance());
}
```


