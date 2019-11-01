---
layout: post
section-type: post
title: springAOP、springIOC
category: tech
comments: true
tags: [ 'springcloud', 'hystrix' ]
---

### hystrix的特性:
* 服务降级
> 1.优先核心服务，非核心服务不可用或弱可用<br>
> 2.通过HystrixCommand注解指定<br>
> 3.fallbackMethod(回退函数)中具体实现<br>
* 服务熔断
* 依赖隔离
> 1.线程池隔离，Hystrix 自动实现了依赖隔离
* 监控(Hystrix DashBoard)
> 可用@SpringCloudApplication 代替 @SpringBootApplication @EnableDiscoveryClient @EnableCircuitBreaker三个注解













### SpringIOC:
>

#### IOC的底层实现:
1.通过beandefinition来承载bean的属性，通过beandifinitionRegistry来记录name和bean的对应关系。
2.beanFactory,通过BeandefinationRegistry来获取Beandefinition中的bean来进行bean的加工。
>查找依赖
* 通过类型查找 (注入属性时：通过setter注入参数的setter方法的命名不需要关心)
* 通过name查找的 (注入属性时：通过setter注入参数的时候setter方法必须按照标准的settler来写)

#### 容器启动的过程
* xml配置文件配置的bean 创建beanFactory过程中注入容器
* 注解 @Bean 配置的bean 调用invokeBeanFactoryPostProcesssors中注入的

#### springMVC组件执行流程
DispatcherServlet --> HandlerMapping(基于URL查找Handler) --> HandlerAdapter(2.基于Handler查找适配器) --> Handler(3.执行业务处理) --> ViewResolver (4.基于ViewName查找View) --> View (5.执行视图解析)
