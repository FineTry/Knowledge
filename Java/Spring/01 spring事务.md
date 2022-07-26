# Spring事务机制

[TOC]

## 一、spring两种事务

spring两种事务：

1. 编程性事务（允许在代码里精确定义事务边界）
2. 声明式事务。

>  简单地说，**编程式事务侵入到了业务代码里面**，但是提供了更加详细的事务管理；而**声明式事务由于基于AOP**，所以既能起到事务管理的作用，又可以**不影响业务代码的具体实现。**



### 1. 编程性事务

> Spring提供两种方式的编程式事务管理，分别是：使用TransactionTemplate和直接使用PlatformTransactionManager。



```java
TransactionTemplate transactionTemplate = new TransactionTemplate();
transactionTemplate.execute(ts -> {
	try {
		// 业务代码
	} catch (Exception e) {
        // 回滚
		ts.setRollbackOnly();
	}
});
```



### 2. 声明式事务

> 声明式事务管理有三种实现方式：**基于TransactionProxyFactoryBean的方式**、**基于AspectJ的XML方式**、**基于注解的方式（@Transaction）**





## 二、spring事务回滚机制

* **默认spring事务只在发生未被捕获的 RuntimeExcetpion时才回滚。**



1. 下面代码不会回滚。

```java
f(userSave){          
    try {         
        userDao.save(user);          
        userCapabilityQuotaDao.save(capabilityQuota);         
     } catch (Exception e) {          
        logger.info("能力开通接口，开户异常，异常信息："+e);         
     }         
 }  
```

2. 下面会回滚

```java
if(userSave){         
     try {          
        userDao.save(user);          
        userCapabilityQuotaDao.save(capabilityQuota);         
       } catch (Exception e) {         
        logger.info("能力开通接口，开户异常，异常信息："+e);          
        throw new RuntimeException();   // 抛出异常，自动回滚
        // 手动回滚
        // TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();        
     }          
}  
```

> spring aop 异常捕获原理：被拦截的方法需显式抛出异常，并不经任何处理，这样aop代理才能捕获到方法的异常，才能进行回滚，**默认情况下aop只捕获RuntimeExcetpion的异常。**

* 解决方法：
  * `throw new RuntimeException();` 抛出异常
  * `TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();` 手动回滚

