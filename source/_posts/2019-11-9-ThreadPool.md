---
layout: post
section-type: post
title: spring-transaction
category: tech
comments: true
tags: [ 'java', 'thread' ]
---

###  线程池 :



#### ThreadPoolExecutor(构造函数)

```java
public ThreadPoolExecutor(int corePoolSize,
                         int maximumPoolSize,
                         long keepApliveTime,
                         TimeUnit unit,
                         BlockingQueue<Runnable> workQueue,
                         ThreadFactory threadFactory,
                         RejectedExecutionHandler handler) {
    if (corePoolSize < 0 ||
       maximumPoolSize <= 0 ||
       maximumPoolSize < corePoolSize ||
       keepAliveTime < 0)
        throw new IllegalArgumentException();
    if (workQueue == null || threadFactory == null || handler == null)
        throw new NullPointerException();
    this.acc = System.getSecurityManager() == null ? 
        null : 
    AccessController.getContext();
    this.corePoolSize = corePoolSize;
    this.maximumPoolSize = maxmumPoolSize;
    this.workQueue = workQueue;
    this.keepAliveTime = unit.toNanos(keepAliveTime);
    this.threadFactory = threadFactory;
    this.handler = handler;
}
```

* corePoolSize: 线程池的核心线程数目，当一个请求进来时如果当前线程池中线程数量小于这个值，则直接通过ThreadFactory新建一个线程来处理这个请求，如果已有线程数量大于等于这个值则将请求放入阻塞队列中。

* maximumPoolSize: 线程池的最大线程数目，当线程池数量已经等于corePoolSize并且阻塞队列已经满了，则看线程数量是否小于maximumPoolSize:如果小于则创建一个线程来处理请求，否则使用“饱和策略”来拒绝这个请求。对于大于corePoolSize部分的线程，称这部分线程为“idle threads”, 这部分线程会有一个最大空闲时间，如果超过这个空闲时间还没有任务进来则将这些空闲线程回收。

* keepAliveTime和unit：这两个参数只要用来控制idle threads的最大空闲时间，超过这个空闲时间空闲线程将被回收。这里需要注意，ThreadPoolExecutor中有一个属性：**private volatile boolean allowCoreThreadTimeOut;** ,这个用来制定是否允许核心线程空闲超时回收，默认为false，即不允许核心线程超时回收，核心线程将一直等待新任务。如果设置这个参数为true，核心线程空闲超时后也可以被回收。

* workQueue：阻塞队列，超过corePoolSize部分的请求放入这个阻塞队列中等待执行。阻塞队列分为有界队列和无界队列。在创建阻塞队列时如果我们指定了这个队列的“capacity”则这个队列就是有界的，否则是无界的。这里有一点需要注意：使用线程池之前请明确是否真的需要无界阻塞队列，如果阻塞队列是无界的，回导致大量的请求堆积，进而造成内存溢出系统崩溃。

* threadFactory：是一个线程池工厂，主要用来为线程池创建线程，我们可以定制一个ThreadFactory来达到统一命名我们线程池中的线程的目的。

* hadler：饱和策略，用来拒绝多余的请求。饱和策略有：CallerRunsPolicy：请求脱离线程池运行(调用者caller线程池来运行这个任务)；AbortPolicy：抛出RejectedException异常；运行DiscardPolicy：丢弃这个任务，也即什么都不做；DiscardOldestPolicy：将阻塞队列中等待时间最久的任务删除(即队列头部的任务)，将新的任务加入队尾。

  ![threadPoolExecutor](https://upload-images.jianshu.io/upload_images/11963487-333bd6568944b3aa.jpg?imageMogr2/auto-orient/strip|imageView2/2/format/webp)