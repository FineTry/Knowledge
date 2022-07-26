# 09 JVM参数优化

[TOC]

## 一、JVM参数

### 1. JVM常用参数

1. **-verbose:gc** 
   * 展示gc详细信息
2. **-Xms20M** 
   * 堆初始化大小20M
3. **-Xmx20M**
   * 堆扩展最大20M
4. **-Xmn10M**
   * 新生代10M
5. **-XX:+PrintGCDetails** 
   * 打印GC详细信息
6. **-XX:SurvivorRatio=8**
   * 新生代中可以分为伊甸园区（Eden区），From Survivor 区 （S0区）和 To Survivor 区 （S1区）。 占用的空间分别默认为 8：1：1
   * 此参数调整比例，如果8就是8:2，那么按照新生代10M计算，Eduen区为8M，S0 -> 1M，S1 -> 1M。

7. **-XX:MaxTenuringThreshold=15**
   * 在新生代中，第一次发生MinorGC回将Eden存活的对象移入S0，年龄+1。第二次发生MinorGC，会回收Eden和S0区。将两个区存活的对象年龄+1，移入S1。当满足**15**岁，就会进入老年代。
