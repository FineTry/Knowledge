# Effective Java

> 重构java代码，使用优秀的编码替换老旧的编码。

[TOC]

## 一、创建和销毁对象

### 1. 避免创建不必要的对象，和避免使用Finalizer和Cleaner机制。



### 2. try-with-resources 优先try-finally

1. try-finally有一定的缺陷：close可能执行失败，close抛出异常会压制前一个异常

```java
// 在实现了AutoCloseable 的类上，可以直接使用try-with-resources ，他会自动关闭
try (BufferedReader br = new BufferedReader(new FileReader(""));
    BufferedReader br1 = new BufferedReader(new FileReader(""));) {
        // ......
    }catch (Exception e){
        e.printStackTrace();
}
```



### 3. 通过私有构造器强化不可实例化的能力

1. 如果一个类只有静态方法或者静态域，那么最好私有化构造方法。

```java
// Noninstantiable utility class 
public class UtilityClass { 
// Suppess default constructo for noninstantiability
	private void UtilityClass{}
    // ...
}
```



### 4. 消除过期的对象

1. 如果一个栈先增长然后再收缩，从栈中弹出来的对象将不会被当作垃圾回收，即使使用栈的程序不再引用这些对象，它们也不会被回收，这是因为栈内部维护着对这些对象的**过期引用**。

   在pop的时候：

   ```java
   // 对象指向空
   Object esult = elements[--si ze]; 
   elements[size] =null;// Eliminate obsolete refer ence
   etu esult;
   ```

   

