---
draft: false 
date: 2024-03-22
categories:
  - life
comments: true
authors: [saymealoud]

---

消息真的发送出去了吗?



- 消息发送后,发送端不知道Rabbit MQ 是否真的收到了消息
- 若 Rabbit MQ 异常,消息丢失后,订单处理流程停止,业务异常
- 需要使用Rabbit MQ 发送端确认机制, 确认消息发送



消息真的被路由了吗?

- 消息发送后,发送端不知道消息是否被正确路由, 若路由异常,消息会被丢弃
- 消息丢弃后,订单处理流程停止,业务异常
- 需要使用 Rabbit MQ 消息返回机制, 确认消息被正确路由



队列饱满怎么办?

- 默认情况下,消息进入队列,会永远存在,直到被消费
- 大量堆积的消息会给Rabbit MQ 产生很大的压力
- 需要使用 Rabbit MQ 消息过期时间,  防止消息大量积压



如何转移过期消息

- 消息被设置了过期时间,过期后会直接被丢弃
- 直接被丢弃的消息,无法对系统运行异常发出警报 
- 需要使用 Rabbit MQ **死信队列** ,收集过期消息,以供分析



目前需要处理:

1. 发送端确认
2. 消息返回
3. 消费端限流
4. 消费端确认
5. 消息过期机制
6. 死信队列



实际开发经验

1. 线程池    溢出  线程爆炸
2. POJO    类 单一职责



发送方

- 需要使用 Rabbit MQ **发送端确认机制,**确认消息成功发送到 Rabbit MQ 并被处理
- 需要使用Rabbit MQ  **消息返回机制**, 若没发现目标队列, 中间件会通知发送方



消费方

- 需要使用 Rabbit MQ **消费端确认机制**,  确认消息没有发生处理异常
- 需要使用 Rabbit MQ  **消费端限流机制**, 限制消息推送速度, 保障接收端服务稳定



Rabbit MQ 自身

- 大量堆积的消息会给 Rabbit MQ  产生很大的压力, 需要使用 Rabbit MQ **消息过期时间**,防止消息大量积压
- 过期后会直接丢弃, 无法对系统运行异常发出警报, 需要使用 Rabbit MQ  **死信队列** , 收集过期消息,以供分析



消息真的发出去了吗?

- 消息发送后, 发送端不知道 Rabbit MQ 是否真的收到了消息
- 若 Rabbit MQ异常 , 消息丢失后, 订单处理流程停止, 业务异常
- 需要使用 Rabbit MQ **发送端确认机制**, 确认消息发送



什么是发送端确认机制

- 消息发送后, 若中间件收到消息,会给发送端一个应答
- 生产者,接收应答,用来确认这条消息是否正常发送到中间件



三种确认机制

- 单条同步确认
  - 配置 Channel ,开启确认模式: channel.confirmSelect()
  - 每发送一条消息,调用 channel.waitForConfirms()方法,等待确认
- 多条同步确认 (不推荐)
  - 配置 Channel ,开启确认模式: channel.confirmSelect()
  - 发送多条消息,调用 channel.waitForConfirms()方法,等待确认 
- 异步确认 (不推荐)
  - 配置 Channel ,开启确认模式: channel.confirmSelect()
  - 在channel 上添加监听: addConfirmListener, 发送消息后,会回调此方法,通知是否发送成功
  - 异步确认有可能是单条, 也有可能是多条,取决于 MQ



消息真的被路由了吗

- 消息发送后,发送端不知道消息是否被正确路由,若路由异常,消息会被丢弃
- 消息丢弃后,订单处理流程停止,业务异常
- 需要使用 Rabbit MQ **消息返回机制**, 确认消息被正确路由



消息返回机制的原理

- 消息发送后,中间件会对消息进行路由
- 若没有发现目标队列, 中间件会通知发送方
- Return Listener 会被调用



消息返回的开启方法

- 在 Rabbit MQ 基础配置中有一个关键配置项 : Mandatory 
- Mandatory 若为false, Rabbit MQ 将直接丢弃无法路由的消息
- Mandatory 若为 true , Rabbit MQ 才会处理无法路由的消息



消费端确认机制

- 默认情况下,消费端接收消息时,消息会被自动确认 (ACK)
- 消费端处理消息异常时,发送端与消息中间件无法得知消息处理情况
- 需要使用 Rabbit MQ **消费端确认机制**, 确认消息被正确处理



消费端 ACK 类型

- 自动 ACK : 消费端收到消息后, 会自动签收消息
- 手动 ACK: 消费端收到消息后, 不会自动签收消息, 需要我们在业务代码中显式签收消息
  - 单条手动 ACK : multiple = false (**推荐**)
  - 多条手动 ACK : multiple = true



重回队列

- 若设置了重回队列, 消息被 NACK后, 会返回队列末尾,等待进一步处理
- 一般不建议开启重回队列,因为第一次处理异常的消息,再次处理,基本上也是异常



消费端限流机制



- 业务高峰期,可能出现发送端与接收端性能不一致,大量消息被同时推送给接收端,造成接收端服务崩溃
- 需要使用 Rabbit MQ **消费端限流机制**, 限制消息推送速度,保障接收端服务稳定



实际场景

- 业务高峰期,有个微服务崩溃了,崩溃期间队列积压了大量消息,微服务上线后,收到大量并发消息
- 将同样多的消息推给能力不同的副本,会导致部分副本异常



Rabbit MQ   QoS

- 针对上面问题,  Rabbit MQ 开发了 QoS (服务质量保证)功能
- QoS 功能保证了在一定数目的消息未被确认前,不消费新的信息
- Qos 功能的前提是不使用自动确认



QoS 原理

- QoS 原理是当消费者有一定数量的消息未被 ACK 确认时, Rabbit MQ 不给消费端推送新的消息
- Rabbit MQ 使用 QoS 机制实现了消费端限流



消费端限流机制参数设置

- prefetchCount:  针对消费端最多推送多少未确认消息
- global: true : 针对整个消费端限流 false : 针对当前 channel 
- prefetchSize: 0 (单个消息大小限制, 一般为 0)
- prefetchSize 与 global 两项, Rabbit MQ 暂时未实现



消息过期机制



队列饱满怎么办?

- 默认情况下,消息进入队列,会永远存在,直到被消费
- 大量堆积的消息回给 Rabbit MQ 产生很大的压力
- 需要使用 Rabbit MQ **消息过期时间**,防止消息大量积压



Rabbit MQ的 过期时间 (TTL)

- Rabbit MQ 的过期时间称为  TTL 
- Rabbit MQ 过期时间分为 消息 TTL 和 队列 TTL
- 消息TTL 设置了消息的单条过期
- 队列 TTL 设置了队列中所有消息的过期时间  (如果之前已经定义了队列那就要删除,不然会设置 TTL 启动会报错)



如何找到自己合适的 TTL?

- TTL  设置主要考虑技术架构与业务
- TTL 应该明显长于服务的平均重启时间
- 建议 TTL 长于业务高峰期时间



死信队列

如何转移过期消息?

- 消息设置了过期时间,过期后会直接丢弃
- 直接丢弃的消息,无法对系统运行异常发出警报
- 需要使用 Rabbit MQ 私信队列, 收集过期消息,以供分析



什么是死信队列

- 死信队列: 队列配置了 DLX 属性(Dead-Letter-Exchange)
- 当一个消息变成死信(dead message )后, 能重新被发布到另一个 Exchange,这个 Exchange 也是一个普通的交换机
- 死信被死信交换机路由后, 一般进入一个固定队列



怎么变成死信

- 消息被拒绝 (reject/nack) 并且 requeue = false
- 消息过期 (TTL 到期)
- 队列达到最大长度



死信队列设置方法

- 设置转发、接收死信的交换机和队列:
  - Exchange: dlx.exchange
  - Queue: dlx.queue
  - RoutingKey: #
- 在需要设置死信的队列加入参数:
  - X-dead-letter-exchange =dlx.exchange



summary



慎用Rabbit MQ 高级特性

- 不要无限追求高级,用 Rabbit MQ的高级特性
- 重回队列、发送端确认是不常用的特性,谨慎使用
- 管控台是 调试 Rabbit MQ的利器
- Rabbit MQ高级特性多数涉及交换机、队列的属性配置,可以在管控台确认配置是否生效
- Rabbit MQ 高级特性很多都可以在管控台进行实验



总结

- 为了确保消息发送,使用了发送端确认机制
- 为了确保消息正确路由,使用了消息返回机制
- 为了保证消息正常梳理,使用了消费端确认机制
- 为了保证消费端稳定,使用消费端限流机制
- 为了中间件问题,使用过期时间机制
- 为了处理异常消息,使用死信机制



引入 Spring Boot 的重要性 

1. 易用的 Rabbit MQ 连接方式
2. 方便的 Rabbit MQ配置方式
3. 提供了完善的消息收发方式



Spring AMQP 特性

- 异步消息监听容器
  - 原始实现: 自己实现线程池、回调方法,并注册回调方法
  - Spring Boot: 自动实现可配置的线程池,并自动注册回调方法,只需实现回调方法
- 原生提供 RabbitTemplate , 方便收发消息
  - 相比 basicPublish, 功能更强大,能自动实现消息转换等功能
- 原生提供 RabbitAdmin ,方便队列、交换机声明
  - 声明式提供队列、交换机、绑定关系的注册方法
  - 设置不需要显式的注册代码
- Spring Boot Config  原生支持 Rabbit MQ 
  - 充分发挥 Spring Boot 约定大于配置的特性
  - 可以隐式建立 Connection、Channel



RabbitAdmin

- 管理 Rabbit MQ

  - declareExchange : 创建交换机
  - deleteExchange: 删除交换机
  - declareQueue: 创建队列
  - deleteQueue : 删除队列
  - purgeQueue: 清空队列
  - declareBinding: 新建绑定关系
  - removeBinding:  删除绑定关系
  - getQueueProperties: 查询队列属性

- 创建方法

  ```java
  ConnectionFactory connectionFactory = new CachingConnectionFactory();
  RabbitAdmin rabbitAdmin = new RabbitAdmin(connectionFactory);
  ```

  

RabbitAdmin 声明式配置

- 将Exchange、Queue、Binding 声明为 Bean
- 再将 RabbitAdmin 声明为  Bean
- Exchange、Queue、Binding 即可自动创建

 

RabbitAdmin  声明式配置的优点

- 将声明和创建工作分开,解耦多人工作
-  不需要显式声明,减少代码量,减少bug



RabbitTemplate

- RabbitTemplate 与 RestTemplate 类似,使用了模版方法设计模式
- RabbitTemplate 提供了丰富的功能,方便消息收发
- RabbitTemplate 可以显式传入配置也可以隐式声明配置





SimpleMessageListenerContainer 高效监听消息

- 设置同时监听多个队列、自动启动、自动配置Rabbit MQ
- 设置消费者数量 (最大数量、最小数量、批量消费)
- 设置消息确认模式、是否重回队列、异常捕获
- 设置是否独占、其他消费者属性等
- 设置具体的监听器、消息转换器等
- 支持动态设置,运行中修改监听器配置



MessageListenerAdapter 消息监听适配器

- 适配器设计模式
- 解决业务逻辑代码无法修改的问题



MessageConverter

- 收发消息,使用 Byte[] 数组作为消息体

- 编写业务逻辑时,需要使用 Java 对象

- MessageConverter 用来在接收消息时自动转换消息

  

Jackson2JsonMessageConverter

- 最常用的 MessageConverter ,用来转换 Json 格式信息
- 配合 ClassMapper 可以直接转换为 POJO 对象 



RabbitListener 

- RabbitListener 是 SpringBoot 架构中监听消息的“终极方案”
- RabbitListener 使用注解声明,对业务代码无侵入
- RabbitListener 可以在 Springboot  配置文件中进行配置



@RabbitListener 注解

- @RabbitListener 是一个组合注解,可以嵌套以下注解
- @Exchange : 自动声明Exchange
- @Queue : 自动声明队列
- @QueueBinding: 自动声明绑定关系



Rabbit 容量不足

- 受制于服务器、虚机、容器的规模,单节Rabbit MQ容量受限
- 在业务量庞大时,单节点MQ可能会因为内存不足导致 OOM

Rabbit MQ 数据无副本

- 单节点 Rabbit MQ 没有备份数据
- 若单节点故障, 消息数据丢失



Rabbit MQ 可用性低

- 单节点 Rabbit MQ 不可避免会出现故障
- 单节点故障后, Rabbit MQ 服务不可用,系统业务崩溃



单节点:

1. 规模不大 OOM
2. 数据安全
3. 可用性



