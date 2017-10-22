---
layout: post
section-type: post
title: springboot整合kafka
category: tech
comments: true
tags: [ 'springboot', 'kafka' ]
---

### kafka百科:
>Kafka是一种高吞吐量的分布式发布订阅消息系统，它可以处理消费者规模的网站中的所有动作流数据。 这种动作（网页浏览，搜索和其他用户的行动）是在现代网络上的许多社会功能的一个关键因素。 这些数据通常是由于吞吐量的要求而通过处理日志和日志聚合来解决。 对于像Hadoop的一样的日志数据和离线分析系统，但又要求实时处理的限制，这是一个可行的解决方案。Kafka的目的是通过Hadoop的并行加载机制来统一线上和离线的消息处理，也是为了通过集群来提供实时的消费。

### mac 使用brew安装kafka
``` bash
cd /usr/local
brew install kafka
```
  因为kafka是依赖zookeeper的,所以使用brew安装kafka的同时会帮我们自动安装zookeeper.
  下面是我们使用命令分别开启zookeeper和kafka,然后运行kafka程序里面的bash脚本来启动一个生产者和一个消费者.
  由于我们使用brew安装的kafka,所以省去了配置环境变量这一步了.

```bash
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties &
#启动zookeeper命令,kafka依赖zookeeper
kafka-server-start /usr/local/etc/kafka/server.properties &
#启动kafka命令
kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
#创建一个topic名为”test”
kafka-topics --list --zookeeper localhost:2181
#查看已经创建的topics列表
kafka-console-producer.sh --broker-list localhost:9092 --topic test
        kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning       
#上面两个命令分别开启生产者和消费者,当我们在消费者终端输入消息,生产者当中就会进行消费
```
![kafaka启动](https://github.com/picong/picong.github.io/blob/master/img/kafka/start.png?raw=true)

![kafaka通信](https://github.com/picong/picong.github.io/blob/master/img/kafka/comunication.png?raw=true)

### springboot整合kafka

#### pom添加依赖:
```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
  </dependency>

  <dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
  </dependency>
```
<br><br>
#### 配置文件:
```javascript
spring:
kafka:
  bootstrap-servers: localhost:9092
  consumer:
    group-id: myGroup
    key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
    value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
  producer:
    key-serializer: org.apache.kafka.common.serialization.StringSerializer
    value-serializer: org.apache.kafka.common.serialization.StringSerializer
```
<br><br>
#### 生产者:
```java
@Component
public class Sender {
  @Autowired
  private KafkaTemplate kafkaTemplate;
  private Gson gson = new GsonBuilder().create();

  public void senMessage(){
      Message m = new Message();
      m.setId(System.currentTimeMillis());
      m.setMsg(UUID.randomUUID().toString());
      m.setSendTime(new Date());
      kafkaTemplate.send("test",gson.toJson(m));
  }
}
```
<br><br>
#### 消费者:
```java
@Component
public class Receiver {

    private final static Logger logger = LoggerFactory.getLogger(Receiver.class);

    private Gson gson = new GsonBuilder().create();

    @KafkaListener(topics = "test")
    public void processMessage(String content){
        Message m = gson.fromJson(content,Message.class);
        logger.info("消息发送者发送的信息 m={}",m);
    }
}
```
<br><br>
#### springboot 启动入口:
```java
@SpringBootApplication
public class KafkaApplication {
	public static void main(String[] args) throws InterruptedException {
		ApplicationContext app = SpringApplication.run(KafkaApplication.class, args);

		while(true){
			Sender sender = app.getBean(Sender.class);
			sender.senMessage();
			Thread.sleep(500);
		}

	}
}
```

### 参考文档及源代码
#### 修改过后的🌰[github](https://github.com/picong/springbootstudy/tree/master/kafka "github")
#### 原来的🌰[csdn](http://blog.csdn.net/gebitan505/article/details/70242016 "csdn")
