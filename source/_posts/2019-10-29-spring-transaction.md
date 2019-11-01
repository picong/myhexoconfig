---
layout: post
section-type: post
title: spring-transaction
category: tech
comments: true
tags: [ 'spring', 'transaction' ]
---

### 事务的四个特性(ACID):
> * 原子性(Atomicity): 事务是一个原子操作，由一系列动作组成。事务原子性确保动作要么全部完成，要么完全不起作用。
> * 一致性(Consistency): 一旦事务完成(不管成功还是失败)，系统必须确保它所建模的业务处于一致的状态，而不会是部分完成部分失败。在显示中的数据不应该被破坏。
> * 隔离性(Isolation): 可能有许多事务会同事处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏。

### spring事务相关的核心接口:
* PlatformTransactionManager
* TransactionDefinition
* TransactionStatus
![ ](https://img-blog.csdn.net/20160324011156424)

#### 事务管理器
Spring并不直接管理事务，而是提供了多种事务管理器，他们将事务管理的职责委托给Hibernate或者JTA等持久化机制所提供的相关平台框架的事务来实现。

#### 传播行为(当事务方法被另一个事务方法调用时，必须指定事务应该如何传播)
传播行为|含义
:---|:---
propagation_required|表示当前方法必须运行在事务中。如果当前事务存在，方法将会在该事务中运行。否则，会启动一个新的事务。
propagation_supports|表示当前方法不需要事务上下文，但是如果存在当前事务的话，那么该方法会在这个事务中运行
propagation_mandatory|表示该方法必须在事务中运行，如果当前事务不存在，则会抛出一个异常
propagation_required_new|表示当前方法必须运行在它自己的事务中。一个新的事务将被启动。如果存在当前事务，在该方法执行期间，当前事务会被挂起。如果使用JTATransactionManager的话，则需要访问TransactionManager
propagation_not_supported|表示该方法不应该运行在事务中。如果存在当前事务，在该方法运行期间，当前事务将被挂起。如果使用JTATransactionManager的话，需要访问TransactionManager
propagation_nested|表示如果当前已经存在一个事务，那么该方法将会在嵌套事务中运行。嵌套的事务可以独立于当前事务进行单独地提交或回滚。如果当前事务不存在，那么其行为与propagation_required一样。注意各厂商对这种传播行为的支持是由所差异的。可以参考资源管理器的文档来确认他们是否支持嵌套事务


