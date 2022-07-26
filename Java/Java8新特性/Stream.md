# Stream详解

> Java 8 引入了全新的 Stream API，可以使用声明的方式来处理数据，极大地方便了集合操作，让我们可以使用更少的代码来实现更为复杂的逻辑，本文主要对一些常用的Stream API进行介绍。

[TOC]

## 一、Stream 聚合操作

### 1. Stream 流的创建

> Stream对象分为两种，一种串行的流对象，一种并行的流对象。

```java
// permissionList指所有权限列表
// 为集合创建串行流对象
Stream<UmsPermission> stream = permissionList.stream();
// 为集合创建并行流对象
tream<UmsPermission> parallelStream = permissionList.parallelStream();
```

### 2. filter

对Stream中的元素进行过滤操作，当设置条件返回true时返回相应元素。

```java
// 获取权限类型为目录的权限
List<UmsPermission> dirList = permissionList.stream()    
    .filter(permission -> permission.getType() == 0)   
    .collect(Collectors.toList());
```

### 3. map

对Stream中的元素进行转换处理后获取。比如可以将UmsPermission对象转换成Long对象。我们经常会有这样的需求：需要把某些对象的id提取出来，然后根据这些id去查询其他对象，这时可以使用此方法。

```java
// 获取所有权限的id组成的集合
List<Long> idList = permissionList.stream()    
	.map(permission -> permission.getId())    
	.collect(Collectors.toList());
```

### 4. limit

从Stream中获取指定数量的元素。

```java
// 获取前5个权限对象组成的集合
List<UmsPermission> firstFiveList = permissionList.stream()    
    .limit(5)    
    .collect(Collectors.toList());
```

### 5. count

仅获取Stream中元素的个数。

```java
// count操作：获取所有目录权限的个数
long dirPermissionCount = permissionList.stream()    
    .filter(permission -> permission.getType() == 0)    
    .count();
```

### 6. sorted

对Stream中元素按指定规则进行排序。

1. **方式1：**

```java
// 将所有权限按先目录后菜单再按钮的顺序排序
List<UmsPermission> sortedList = permissionList.stream()    			
                .sorted((permission1,permission2)->{return permission1.getType().compareTo(permission2.getType());})    
                .collect(Collectors.toList());
```

2. **方式1简化版：**

```
List<UmsPermission> sortedList = permissionList.stream()
	.sorted(Comparator.comparing(UmsPermission::getType))
	.collect(Collectors.toList());
```

### 7. skip

跳过指定个数的Stream中元素，获取后面的元素。

```java
// 跳过前5个元素，返回后面的
List<UmsPermission> skipList = permissionList.stream()    
    .skip(5)    
    .collect(Collectors.toList());
```

### 8. 用collect方法将List转成map

有时候我们需要反复对List中的对象根据id进行查询，我们可以先把该List转换为以id为key的map结构，然后再通过map.get(id)来获取对象，这样比较方便。

```java
// 将权限列表以id为key，以权限对象为值转换成map
Map<Long, UmsPermission> permissionMap = permissionList.stream()    		  
                .collect(Collectors.toMap(permission -> permission.getId(), permission -> permission));
```

### 9. 和函数式接口Predicate配合

> Predicate的test方法返回一个boolean，可以根据函数式方法体做判断。

```java
public static void filterTest(List<String> languages, Predicate<String> condition) {
	languages.stream().filter(x -> condition.test(x)).forEach(x -> System.out.println(x + " "));
}

public static void main(String[] args) {
    List<String> languages = Arrays.asList("Java","Python","scala","Shell","R");
    System.out.println("Language starts with J: ");
    filterTest(languages,x -> x.startsWith("J"));
    System.out.println("\nLanguage ends with a: ");
    filterTest(languages,x -> x.endsWith("a"));
}
```

### 10. 用lambda表达式实现map与reduce

```java
public void mapReduceTest() {
    List<Double> cost = Arrays.asList(10.0, 20.0,30.0);
    double allCost = cost.stream().map(x -> x+x*0.05).reduce((sum,x) -> sum + x).get();
    System.out.println(allCost);
}
```

* **reduce**使用：

* > reduce可以计算，所有返回单值得，最大，最小，平均，和....

  ```java
  // 方式1
  int accResult = Stream.of(1, 2, 3, 4)
  	.reduce(0, (acc, item) -> acc+item);
  // 方式2
  Optional<Integer> reduce = Stream.of(1, 2, 3, 4)
      .reduce((acc, item) -> acc + item);
  ```

### 11. 求最大，最小，平均，和....

```java
// 方式1，采用IntStream...
public static void main(String[] args) {
    toInt();
    toLong();
    toDouble();
}

private static void toInt() {
    IntSummaryStatistics statistics = Stream.of(1L, 2L, 3L, 4L).mapToInt(Long::intValue).summaryStatistics();
    System.out.println("最大值：" + statistics.getMax());
    System.out.println("最小值：" + statistics.getMin());
    System.out.println("平均值：" + statistics.getAverage());
    System.out.println("求和：" + statistics.getSum());
    System.out.println("计数：" + statistics.getCount());
}

private static void toLong() {
    LongSummaryStatistics statistics = Stream.of(1L, 2L, 3L, 4000000000000000000L).mapToLong(Long::longValue).summaryStatistics();
}

private static void toDouble() {
    DoubleSummaryStatistics statistics = Stream.of(1, 2, 3.0, 5.2).mapToDouble(Number::doubleValue).summaryStatistics();
}
// 方式2，采用reduce进行操作
Optional<Integer> reduce = Stream.of(1, 2, 3, 4)
    .reduce((acc, item) -> acc < item ? acc : item);	// 最大：4
```

### 12. AnyMuch和AllMuch

```java
// anymuch是有一个满足返回true，allmuch是都满足才返回true。
boolean b = Stream.of(1, 2, 3, 4).anyMatch(i -> i == 1);   // true
boolean a = Stream.of(1, 2, 3, 4).allMatch(i -> i == 1);   // false
```





## 二、应用

> UmsPermissionNode继承自UmsPermission对象，之增加了一个children属性，用于存储下级权限。

### 1. 定义获取树形结构的方法

> 我们先过滤出pid为0的顶级权限，然后给每个顶级权限设置其子级权限，covert方法的主要用途就是从所有权限中找出相应权限的子级权限。

```java
@Override
public List<UmsPermissionNode> treeList() {
	List<UmsPermission> permissionList = permissionMapper.selectByExample(new UmsPermissionExample());
List<UmsPermissionNode> result = permissionList.stream()
	.filter(permission -> permission.getPid().equals(0L))
	.map(permission -> covert(permission, permissionList)).collect(Collectors.toList());
return result;
}
```

### 2. 为每个权限设置子级权限

> 这里我们使用filter操作来过滤出每个权限的子级权限，由于子级权限下面可能还会有子级权限，这里我们使用递归来解决。但是递归操作什么时候停止，这里把递归调用方法放到了map操作中去，当没有子级权限时filter下的map操作便不会再执行，从而停止递归。

```java
/**
* 将权限转换为带有子级的权限对象
* 当找不到子级权限的时候map操作不会再递归调用covert
*/
private UmsPermissionNode covert(UmsPermission permission, List<UmsPermission> permissionList) {
    UmsPermissionNode node = new UmsPermissionNode();
    BeanUtils.copyProperties(permission, node);
    List<UmsPermissionNode> children = permissionList.stream()
           .filter(subPermission -> subPermission.getPid().equals(permission.getId()))
           .map(subPermission -> covert(subPermission, permissionList)).collect(Collectors.toList());
    node.setChildren(children);
    return node;
}
```