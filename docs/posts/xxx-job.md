---
draft: false 
date: 2024-03-04
categories:
  - tech
comments: true
authors: [saymealoud]

---




### 第一章 新版分布式调度XXL-Job+SpringBoot课程介绍



#### 第1集 新版分布式调度XXL-Job+SpringBoot实战专题课程介绍

**简介：分布式调度XXL-Job介绍-适合人员和学后水平**

* 课程介绍
  * 本套课程 是**全新录制，**从0到1讲解新版分布式调度XXL-Job核心基础+**高级知识点。**

  * 不止讲解新版XXL-Job核心知识 ，横向对比多个业界解决方案，源码部署分布式调度XXL-Job，讲解控制台各个模块知识点，使用SpringBoot整合XXL-Job实战，多节点执行器和调度策略实战，命令行参数传递和日志配置

  * 【高级部分】XXL-Job集群部署和架构链路讲解，海量任务调度解决方案分片广播讲解和实战

<!-- more -->

- 为什么要学习分布式调度XXL-Job
  - XXL-JOB是一个分布式任务调度平台，其核心设计目标是开发迅速、学习简单、轻量级、易扩展
  - 多数互联网公司里面用的技术，周期性业务 如**消息推送、支付对账、数据统计**等，离不开分布式调度
  - 在多数互联网公司中，分布式调度XXL-Job占有率很高，是近几年大量流行
  - 可以作为公司内部培训技术分享必备知识，超多案例+实战等
  - 有谁在用
    * 众多互联网大厂，包括并不限于虎牙、京东、荔枝FM、平安、360、网易 等 一二线大厂
    * https://www.xuxueli.com/
  
  



* 课程技术栈和测试环境说明

  * JDk8或者JDK11 + SpringBoot + 源码部署 + IDEA+XXLJob2.3

  * 技术不断更新，版本如何选择 

    * 中间件使用这块，不建议做第一个吃螃蟹的人
    * 不同中间件，厂商不一样，不一定及时更新

    

* 学后水平

  
  * 掌握新版XXL-Job核心知识 ，横向对比多个业界解决方案
  * 源码部署XXL-Job和控制台菜单模块功能点
  * 掌握使用SpringBoot整合XXL-Job实战
  * 掌握多节点执行器和调度策略实战，命令行参数传递和日志配置
  * 【高级】XXL-Job集群部署和架构链路讲解
  * 【高级】海量任务调度解决方案分片广播讲解和实战



#### 第2集 新版分布式调度XXL-Job+SpringBoot实战大纲速览
**简介：新版分布式调度XXL-Job+SpringBoot实战大纲速览**

* 课程学前基础
  * 有Linux+SpringBoot就行，不会的话我们有专题课程学习,联系客服小姐姐即可
  * 框架整合这块：采用SpringBoot框架（采用其他语言或框架也行）
  
* 目录大纲浏览

  




### 第二章 急速认知分布式调度XXL-Job框架和源码安装



#### 第1集 你知道多少种定时任务的实现方式

**简介：你知道多少种定时任务的实现方式**

* 需求分析
  - 在平常的业务场景当中，经常有一些场景需要使用到定时任务
  - 比如说在某个时间点会发送优惠券、发送短信等等的一些业务操作，难道这些操作都需要人工来进行干预吗？
  - 比如一些支付系统，需要在每天的凌晨1点来进行对前一天的清算，难道每次都要一个人来负责这个业务吗？



* 定时任务
  * 通过时间表达式这一方式来进行任务调度的被称为定时任务
  * 分类
    * 单机定时任务
      * 单机的容易实现，但应用于集群环境做分布式部署，就会带来重复执行
      * 解决方案有很多比如加锁、数据库等，但是增加了很多非业务逻辑
    * 分布式调度（分布式定时任务）
      * 把需要处理的计划任务放入到统一的平台，实现集群管理调度与分布式部署的定时任务 叫做分布式定时任务
      * 支持集群部署、高可用、并行调度、分片处理等


- 常见定时任务
  - 单机：Java自带的java.util.Timer类配置比较麻烦，时间延后问题
  - 单机：ScheduledExecutorService
    - 是基于线程池来进行设计的定时任务类，在这里每个调度的任务都会分配到线程池里的一个线程去执行该任务，并发执行，互不影响
  - 单机：SpringBoot框架自带
    - SpringBoot使用注解方式开启定时任务
    - 启动类里面 @EnableScheduling开启定时任务，自动扫描
    - 定时任务业务类 加注解 @Component被容器扫描
    - 定时执行的方法加上注解 @Scheduled(fixedRate=2000) 定期执行一次



* 为什么需要任务调度平台

  - 因为在传统的Java当中，传统的定时任务实现方案，像上述的Timer、Quartz等
  - 它们都有不少缺点
    - 不支持集群、不支持统计、没有管理的平台、也没有报警、监控等等
    - 在分布式架构当中，有一些场景是需要用到分布式的任务调度的，在同一个服务器中的多个实例任务之间存在着互斥，需要进行统一的调度，所以任务调度需要支持高可用、监控、故障告警等一系列措施。
    - 需要统一管理和追踪各个的服务节点之间任务调度的结果，并保存记录任务信息等等


  - 如何解决上述问题？
    - 分布式调度平台























#### 第2集 常见的分布式调度平台快速认知

**简介：常见的分布式调度平台快速认知**

- 常见分布式定时任务


  - Quartz

    - Quartz关注点在于定时任务而并非是数据，并没有一套根据数据化处理而定的流程
    - 虽然可以实现数据库作业的高可用，但是缺少了分布式的并行调度功能，相对弱点
    - 不支持任务分片、没UI界面管理，并行调度、失败策略等也缺少

     

  - TBSchedule

    - 这个是阿里早期开源的分布式任务调度系统，使用的是timer而不是线程池执行任务调度，使用timer在处理异常的时候是有缺陷的，但TBSchedule的作业类型比较单一，文档也缺失得比较严重
    - 目前阿里内部使用的是ScheduleX
      - 阿里云也有商业化版本: https://help.aliyun.com/product/147760.html

    ![image-20220519175053475](img/image-20220519175053475.png)

  - Elastic-job

    - 当当开发的分布式任务调度系统，功能强大，采用的是zookeeper实现分布式协调，具有高可用与分片。
    - 2020年6月，ElasticJob的四个子项目已经正式迁入Apache仓库
    - 由 2 个相互独立的子项目 ElasticJob-Lite 和 ElasticJob-Cloud 组成
      - ElasticJob-Lite 定位为轻量级无中心化解决方案，使用jar的形式提供分布式任务的协调服务；
      - ElasticJob-Cloud 使用 Mesos 的解决方案，额外提供资源治理、应用分发以及进程隔离等服务
    - 地址：https://shardingsphere.apache.org/elasticjob/index_zh.html

   

  - XXL-JOB
    - 大众点评的员工徐雪里在15年发布的分布式任务调度平台，是轻量级的分布式任务调度框架，目标是开发迅速、简单、清理、易扩展; 老版本是依赖quartz的定时任务触发，在v2.1.0版本开始 移除quartz依赖
    - 地址：https://www.xuxueli.com/xxl-job/

  

- 常规对比图

| 对比项       | XXL-JOB                                | elastic-job                                                  |
| ------------ | -------------------------------------- | ------------------------------------------------------------ |
| 并行调度     | 调度系统多线程并行                     | 任务分片的方式并行                                           |
| 弹性扩容     | 使用Quartz基于数据库分布式功能         | 通过zookeeper保证                                            |
| 高可用       | 通过DB锁保证                           | 通过zookeeper保证                                            |
| 阻塞策略     | 单机串行/丢弃后续的调度/覆盖之前的调度 | 执行超过zookeeper的session timeout时间的话，会被清除，重新进行分片 |
| 动态分片策略 | 以执行器为维度进行分片、支持动态的扩容 | 平均分配/作业名hash分配/自定义策略                           |
| 失败处理策略 | 失败告警/失败重试                      | 执行完毕后主动获取未分配分片任务 服务器下线后主动寻找可以用的服务器执行任务 |
| 监控         | 支持                                   | 支持                                                         |
| 日志         | 支持                                   | 支持                                                         |

 

- 如何选择哪一个分布式任务调度平台
  - XXL-Job和Elastic-Job都具有广泛的用户基础和完善的技术文档，都可以满足定时任务的基本功能需求
  - xxl-job侧重在业务实现简单和管理方便，容易学习，失败与路由策略丰富, 推荐使用在用户基数相对较少，服务器的数量在一定的范围内的场景下使用
  - elastic-job关注的点在数据，添加了弹性扩容和数据分片的思路，更方便利用分布式服务器的资源, 但是学习难度较大，推荐在数据量庞大，服务器数量多的时候使用













#### 第3集 带你走近分布式调度XXL-Job核心特性

**简介：带你走近分布式调度XXL-Job核心特性**

* 什么是XXL-Job

  * XXL-JOB

    - 大众点评的员工徐雪里在15年发布的分布式任务调度平台，是轻量级的分布式任务调度框架，目标是开发迅速、简单、清理、易扩展; 老版本是依赖quartz的定时任务触发，在v2.1.0版本开始 移除quartz依赖
    - 官网地址：https://www.xuxueli.com/xxl-job/
    - GitHub地址：https://github.com/xuxueli/xxl-job/

  * xxl-job的设计思想

    - 将调度行为抽象形成“调度中心”公共平台，而平台自身并不承担业务逻辑，“调度中心”负责发起调度请求。
    - 将任务抽象成分散的JobHandler，交由“执行器”统一管理
    - “执行器”负责接收调度请求并执行对应的JobHandler中业务逻辑。
    - 因此，“调度”和“任务”两部分可以相互解耦，提高系统整体稳定性和扩展性

    

* 架构图(图片来源是xxl-job官网)

  - 调度中心
    - 负责管理调度的信息，按照调度的配置来发出调度请求
    - 支持可视化、简单的动态管理调度信息，包括新建、删除、更新等，这些操作都会实时生效，同时也支持监控调度结果以及执行日志。
  - 执行器
    - 负责接收请求并且执行任务的逻辑。任务模块专注于任务的执行操作等等，使得开发和维护更加的简单与高效


- XXL-Job具有哪些特性
  - 调度中心HA（中心式）：调度采用了中心式进行设计，“调度中心”支持集群部署，可保证调度中心HA
  - 执行器HA（分布式）：任务分布式的执行，任务执行器支持集群部署，可保证任务执行HA
  - 触发策略：有Cron触发、固定间隔触发、固定延时触发、API事件触发、人工触发、父子任务触发
  - 路由策略：执行器在集群部署的时候提供了丰富的路由策略，如：第一个、最后一个、轮询、随机、一致性HASH、最不经常使用LFU、最久未使用LRU、故障转移等等
  - 故障转移：如果执行器集群的一台机器发生故障，会自动切换到一台正常的执行器发送任务调度
  - Rolling实时日志的监控：支持rolling方式查看输入的完整执行日志
  - 脚本任务：支持GLUE模式开发和运行脚本任务，包括Shell、python、node.js、php等等类型脚本















#### 第4集 分布式调度XXl-Job源码部署和数据库表建立《上》

**简介：新版分布式调度XXl-Job源码部署和数据库表建立《上》**


* 下载地址（版本和课程保证一致）

  * https://github.com/xuxueli/xxl-job/releases
  * 版本V-2.3

* 源码（本章本集资料）

  * 环境：IDEA旗舰版+Maven+JDK8或JDK11+Mysql5.7（本身就是SpringBoot项目）
  * 目录介绍
    * doc：xxl-job的文档资料，包括了数据库的脚本（后面要用到）
    * xxl-job-core：公共jar包依赖
    * xxl-job-admin：调度中心，项目源码，是Springboot项目，可以直接启动
    * xxl-job-executor-samples：执行器，是Sample实例项目，里面的Springboot工程可以直接启动，也可以在该项目的基础上进行开发，也可以将现有的项目改造成为执行器项目

  

* 数据库（本章本集资料或者源码doc/db目录下）

  * xxl_job的数据库里有如下几个表
    - xxl_job_group：执行器信息表，用于维护任务执行器的信息
    - xxl_job_info：调度扩展信息表，主要是用于保存xxl-job的调度任务的扩展信息，比如说像任务分组、任务名、机器的地址等等
    - xxl_job_lock：任务调度锁表
    - xxl_job_log：日志表，主要是用在保存xxl-job任务调度历史信息，像调度结果、执行结果、调度入参等等
    - xxl_job_log_report：日志报表，会存储xxl-job任务调度的日志报表，会在调度中心里的报表功能里使用到
    - xxl_job_logglue：任务的GLUE日志，用于保存GLUE日志的更新历史变化，支持GLUE版本的回溯功能
    - xxl_job_registry：执行器的注册表，用在维护在线的执行器与调度中心的地址信息
    - xxl_job_user：系统的用户表




















#### 第5集 新版分布式调度XXl-Job源码部署和数据库表建立《下》

**简介：新版分布式调度XXl-Job源码部署和数据库表建立《下》**

* 部署XXL-Job步骤

  * xxl-job-admin目录配置文件 application.properties

    * 配置数据库连接

    ```
    ### xxl-job, datasource
    spring.datasource.url=jdbc:mysql://127.0.0.1:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai
    spring.datasource.username=root
    spring.datasource.password=xdclass.net
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    ```

    * 配置 xxl.job.accessToken（后续要配置客户端接入配置token）

    ```
    xxl.job.accessToken=xdclass.net
    ```

    

  * 构建编译：mvn install

  * 启动项目

* 注意

  * 网络部署属于CS架构，需要双方网络互通


  * Mac电脑启动报错


* 访问地址
  * 地址：ip:8080/xxl-job-admin
  * 默认账号密码：admin / 123456

















#### 第6集 分布式调度XXL-Job UI菜单模块介绍

**简介：分布式调度XXL-Job UI界面介绍**

- 运行报表
  - 以图形化来展示了整体的任务执行情况
    - 任务数量：能够看到调度中心运行的任务数量
    - 调度次数：调度中心所触发的调度次数
    - 执行器数量：在整个调度中心中，在线的执行器数量有多少


- 任务管理（配置执行任务）
  - 示例执行器：所用到的执行器
  - 任务描述：概述该任务是做什么的
  - 路由策略：
    - 第一个：选择第一个机器
    - 最后一个：选择最后一个机器
    - 轮询：依次选择执行
    - 随机：随机选择在线的机器
    - 一致性HASH：每个任务按照Hash算法固定选择某一台机器，并且所有的任务均匀散列在不同的机器上
    - 最不经常使用：使用频率最低的机器优先被使用
    - 最近最久未使用：最久未使用的机器优先被选举
    - 故障转移：按照顺序依次进行心跳检测，第一个心跳检测成功的机器选定为目标的执行器并且会发起任务调度
    - 忙碌转移：按照顺序来依次进行空闲检测，第一个空闲检测成功的机器会被选定为目标群机器，并且会发起任务调度
    - 分片广播：广播触发对于集群中的所有机器执行任务，同时会系统会自动传递分片的参数
  - Cron：执行规则 
  - 调度过期策略：调度中心错过调度时间的补偿处理策略，包括：忽略、立即补偿触发一次等
  - JobHandler：定义执行器的名字
  - 阻塞处理策略：
    - 单机串行：新的调度任务在进入到执行器之后，该调度任务进入FIFO队列，并以串行的方式去进行
    - 丢弃后续调度：新的调度任务在进入到执行器之后，如果存在相同的且正在运行的调度任务，本次的调度任务请求就会被丢弃掉，并且标记为失败
    - 覆盖之前的调度：新的调度任务在进入到执行器之后，如果存在相同的且正在运行的调度任务，就会终止掉当前正在运行的调度任务，并且清空队列，运行新的调度任务。
  - 子任务ID：输入子任务的任务id，可填写多个
  - 任务超时时间：添加任务超时的时候，单位s，设置时间大于0的时候就会生效
  - 失败重试次数：设置失败重试的次数，设置时间大于0的时候就会生效
  - 负责人：填写该任务调度的负责人
  - 报警邮件：出现报警，则发送邮件


* 调度日志
  - 这里是查看调度的日志，根据日志来查看任务具体的执行情况是怎样的
* 执行器管理
  - 这里是配置执行器，等待执行器启动的时候都会被调度中心监听加入到地址列表
* 用户管理
  - 可以对用户的一些操作








  
### 第三章 新版SpringBoot整合XXL-Job分布式调度任务实操



#### 第1集 新版Springboot整合XXL-Job项目搭建

**简介：新版Springboot整合XXL-Job项目搭建**

- 新版Springboot介绍
  - GitHub地址：https://github.com/spring-projects/spring-boot
  - 官方文档：https://spring.io/guides/gs/spring-boot/
  - 视频地址：https://item.taobao.com/item.htm?id=618384570391
  - 在线创建：https://start.spring.io/
- 相关的软件环境
  - jdk8/11+maven3.5
  - 编辑器IDEA
- 创建一个Springboot项目
  - 导入本集课程代码即可，pom版本保持一致



- 添加XXL-Job依赖

```
		 <dependency>
            <groupId>com.xuxueli</groupId>
            <artifactId>xxl-job-core</artifactId>
            <version>2.3.0</version>
        </dependency>
```

* 编写简单的任务调度例子

  * 新增logback.xml

  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <configuration debug="false" scan="true" scanPeriod="1 seconds">
  
      <contextName>logback</contextName>
      <property name="log.path" value="./data/logs/xxl-job/app.log"/>
  
      <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
          <encoder>
              <pattern>%d{HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
          </encoder>
      </appender>
  
      <appender name="file" class="ch.qos.logback.core.rolling.RollingFileAppender">
          <file>${log.path}</file>
          <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
              <fileNamePattern>${log.path}.%d{yyyy-MM-dd}.zip</fileNamePattern>
          </rollingPolicy>
          <encoder>
              <pattern>%date %level [%thread] %logger{36} [%file : %line] %msg%n
              </pattern>
          </encoder>
      </appender>
  
      <root level="info">
          <appender-ref ref="console"/>
          <appender-ref ref="file"/>
      </root>
  
  </configuration>
  ```

  

  * 配置文件

  ```
  #----------xxl-job配置--------------
  logging.config=classpath:logback.xml
  #调度中心部署地址,多个配置逗号分隔 "http://address01,http://address02"
  xxl.job.admin.addresses=http://127.0.0.1:8080/xxl-job-admin
  
  #执行器token，非空时启用 xxl-job, access token 
  xxl.job.accessToken=xdclass.net
  
  # 执行器app名称,和控制台那边配置一样的名称，不然注册不上去
  xxl.job.executor.appname=xdclass-shop
  
  # [选填]执行器注册：优先使用该配置作为注册地址，为空时使用内嵌服务 ”IP:PORT“ 作为注册地址。
  #从而更灵活的支持容器类型执行器动态IP和动态映射端口问题。
  xxl.job.executor.address=
  
  #[选填]执行器IP ：默认为空表示自动获取IP（即springboot容器的ip和端口，可以自动获取，也可以指定），多网卡时可手动设置指定IP，该IP不会绑定Host仅作为通讯实用；地址信息用于 "执行器注册" 和 "调度中心请求并触发任务"，
  xxl.job.executor.ip=
  
  # [选填]执行器端口号：小于等于0则自动获取；默认端口为9999，单机部署多个执行器时，注意要配置不同执行器端口；
  xxl.job.executor.port=9999
  
  #执行器日志文件存储路径，需要对该路径拥有读写权限；为空则使用默认路径
  xxl.job.executor.logpath=./data/logs/xxl-job/executor
  
  #执行器日志保存天数
  xxl.job.executor.logretentiondays=30
  ```

  

  * XxlJobConfig

  ```
  @Configuration
  public class XxlJobConfig {
      private Logger log = LoggerFactory.getLogger(XxlJobConfig.class);
      
      @Value("${xxl.job.admin.addresses}")
      private String adminAddresses;
  
      @Value("${xxl.job.executor.appname}")
      private String appName;
  
      @Value("${xxl.job.executor.ip}")
      private String ip;
  
      @Value("${xxl.job.executor.port}")
      private int port;
  
      @Value("${xxl.job.accessToken}")
      private String accessToken;
  
      @Value("${xxl.job.executor.logpath}")
      private String logPath;
  
      @Value("${xxl.job.executor.logretentiondays}")
      private int logRetentionDays;
  
      //旧版的有bug
      //@Bean(initMethod = "start", destroyMethod = "destroy")
      @Bean
      public XxlJobSpringExecutor xxlJobExecutor() {
          log.info(">>>>>>>>>>> xxl-job config init.");
          XxlJobSpringExecutor xxlJobSpringExecutor = new XxlJobSpringExecutor();
          xxlJobSpringExecutor.setAdminAddresses(adminAddresses);
          xxlJobSpringExecutor.setAppname(appName);
          xxlJobSpringExecutor.setIp(ip);
          xxlJobSpringExecutor.setPort(port);
          xxlJobSpringExecutor.setAccessToken(accessToken);
          xxlJobSpringExecutor.setLogPath(logPath);
          xxlJobSpringExecutor.setLogRetentionDays(logRetentionDays);
  
          return xxlJobSpringExecutor;
      }
  }
  ```

















#### 第2集 创建你的第一个XXL-Job分布式调度任务

**简介：创建你的第一个XXL-Job分布式调度任务**

- 注解介绍

  - 在 Spring Bean 实例中，开发 Job 方法方式格式要求为

  ```
  public ReturnT<String> execute(String param)
  ```

  - 为 Job 方法添加注解 （注解value值是调度中心新建任务的 JobHandler 属性的值）

  ```
  @XxlJob(value="自定义jobHandler名称", init = "handler初始化方法", destroy = "handler 销毁方法")
  ```

- 编写代码

```
@Component

public class MyJobHandler {
    private Logger log = LoggerFactory.getLogger(MyJobHandler.class);
    
    @XxlJob(value = "demoJobHandler",init = "init",destroy = "destroy")
    public ReturnT<String> execute(String param){

        log.info("小滴课堂 execute 任务方法触发成功");

        return ReturnT.SUCCESS;
    }

    private void init(){


        log.info("小滴课堂 MyJobHandler init >>>>>");
    }

    private void destroy(){
        log.info("小滴课堂 MyJobHandler destroy >>>>>");
    }

}
```

- 执行器管理
  - 到xxl-job调度中心里的执行器管理->新增
    - Appname：是每一个执行器的唯一表示AppName，执行器会以周期性为appname进行注册，为任务调度的时候使用
    - 名称：执行器的名称，因为appname有限制字母与数字等等组成，可读性不强，这个名称就是为了提高执行器的可读性
    - 注册方式：调度中心获取执行器地址的方式
      - 自动注册：执行器自动进行执行器的注册，通过底层的注册表可以动态的发现执行器机器的地址
      - 手动录入：人工手动录入执行器的地址信息，多地址使用逗号进行分割，供调度中心使用
    - 机器地址：“注册方式”为手动录入的时候才能使用，支持人工维护执行器的地址
    - 点击保存后可能要等30S左右才回显示机器的地址


- 任务管理
  - 任务管理->选择所需要管理的执行器->新增执行器






















#### 第3集 执行和分析第一个XXL-Job分布式调度任务

**简介：执行和分析第一个XXL-Job分布式调度任务**

- 任务管理-启动任务

  - 任务管理的状态已经从STOP变更成RUNNING


- 调度日志

  - 在调度日志里能够查看调度结果与日志的信息




























#### 第4集 Executor执行器多节点部署和调度策略讲解实战

**简介：执行器多节点部署和调度策略讲解实战**

* 本机部署多节点
  * 程序端口修改
  * 执行器端口修改



* 策略
  * 第一个、最后一个、轮训




















**更多架构课程请访问 xdclass.net**

### 第四章 【进阶】XXL-Job分布式调度多案例实战



#### 第1集 分布式调度参数传递和调度日志配置

**简介：分布式调度参数传递和调度日志配置讲解**

* UI界面参数传递

```
String jobParam = XxlJobHelper.getJobParam();
```

* 执行日志打印
  * 需要通过 "XxlJobHelper.log" 打印执行日志




* 执行结果
  * 默认任务结果为 "成功" 状态，不需要主动设置
  * 自主设置任务结果（会出现在【执行备注】里面）
    * 如想设置任务结果为失败，可以通过 "XxlJobHelper.handleFail
    * 如想设置任务结果为成功，handleSuccess" 












#### 第2集 【高级】XXL-Job集群部署和高可用案例配置

**简介：【高级】XXL-Job集群部署和高可用案例讲解**

* HA/集群

  * 为了避免单点故障，任务调度系统通常需要通过集群实现系统高可用
  * 由于任务调度系统的特殊性，“调度”和“任务”两个模块需要均支持集群部署，由于职责不同，因此各自集群侧重点也有有所不同。

  

  - 调度中心集群

    - 目标为避免调度模块单点故障，集群节点需要通过锁或命名服务保证单个任务的单次触发，只在其中一个节点上生效，以防止任务的重复触发。

  - 执行器集群

    - 目标为避免任务模块单点故障，进一步可以通过自定义路由策略实现Failover等高级功能，从而在执行器某台机器节点故障时自动转移不会影响到任务的正常触发执行

    

* 高可用集群架构


* 执行器配置调度中心集群

```
#调度中心部署地址,多个配置逗号分隔 "http://address01,http://address02"
xxl.job.admin.addresses=http://127.0.0.1:8080/xxl-job-admin,http://127.0.0.1:8079/xxl-job-admin
```









#### 第3集 【高级】XXL-Job海量数据处理-分片任务实战《上》

**简介：XXL-Job海量数据处理-分片任务实战《上》**

* 需求

  * 有一个任务需要处理100W条数据，每条数据的业务逻辑处理要0.1s
  * 对于普通任务来说，只有一个线程来处理 可能需要10万秒才能处理完，业务则严重受影响
  * 案例：双十一大促，给1000万用户发营销短信

  

* 什么是分片任务

  * 执行器集群部署，如果任务的路由策略选择【分片广播】，一次任务调度将会【广播触发】对应集群中所有执行器执行一次任务，同时系统自动传递分片参数，执行器可根据分片参数开发分片任务
  * 需要处理的海量数据，以执行器为划分，每个执行器分配一定的任务数，并行执行
  * XXL-Job支持动态扩容执行器集群，从而动态增加分片数量，到达更快处理任务
  * 分片的值是调度中心分配的

  ```
  // 当前分片数，从0开始，即执行器的序号
  int shardIndex = XxlJobHelper.getShardIndex();
  //总分片数，执行器集群总机器数量
  int shardTotal = XxlJobHelper.getShardTotal();
  ```


* 解决思路

  * 如果将100W数据均匀分给集群里的10台机器同时处理，
  * 每台机器耗时，1万秒即可，耗时会大大缩短，也能充分利用集群资源
  * 在xxl-job里，可以配置执行器集群有10个机器，那么分片总数是10，分片序号0~9 分别对应那10台机器。

  * 分片方式
    * id % 分片总数 余数是0 的，在第1个执行器上执行
    * id % 分片总数 余数是1 的，在第2个执行器上执行
    * id % 分片总数 余数是2 的，在第3个执行器上执行
    * ...
    * id % 分片总数 余数是9 的，在第10个执行器上执行

  

  

  * 路由策略
    * 选择为【分片广播】


  * 其他解决方式
    * 也可以启动多个job，使用同个jobHandler,通过命令行参数控制











#### 第4集 【高级】XXL-Job海量数据处理-分片任务实战《下》

**简介：XXL-Job海量数据处理-分片任务实战《上》**

* 编码实战

  * 需求：100个用户，分片处理
  * 新增job测试案例

  ```
  	 /**
       * 2、分片广播任务
       */
      @XxlJob("shardingJobHandler")
      public void shardingJobHandler() throws Exception {
  
          // 当前分片数，从0开始，即执行器的序号
          int shardIndex = XxlJobHelper.getShardIndex();
          //总分片数，执行器集群总机器数量
          int shardTotal = XxlJobHelper.getShardTotal();
  
  
          XxlJobHelper.log("分片参数：当前分片序号 = {}, 总分片数 = {}", shardIndex, shardTotal);
  
          // 业务逻辑
          for (int i = 0; i < shardTotal; i++) {
              if (i == shardIndex) {
                  log.info("第 {} 片, 命中分片开始处理", i);
  
              } else {
                  log.info("第 {} 片, 忽略", i);
              }
          }
  
      }
  ```

  	* 根据id进行分片取模（部署3个执行器）

  ```
  
      /**
       * 2、分片广播任务
       */
      @XxlJob("shardingJobHandler")
      public void shardingJobHandler() throws Exception {
  
          // 当前分片数，从0开始，即执行器的序号
          int shardIndex = XxlJobHelper.getShardIndex();
          //总分片数，执行器集群总机器数量
          int shardTotal = XxlJobHelper.getShardTotal();
  
          XxlJobHelper.log("分片参数：当前分片序号 = {}, 总分片数 = {}", shardIndex, shardTotal);
  
  
          List<Integer> allUserIds = getAllUserIds();
          allUserIds.forEach(obj -> {
              if (obj % shardTotal == shardIndex) {
                  log.info("第 {} 片, 命中分片开始处理用户id={}",shardIndex,obj);
              }
          });
      }
  
      private List<Integer> getAllUserIds() {
  
          List<Integer> ids = new ArrayList<>();
          for (int i = 0; i < 100; i++) {
              ids.add(i);
          }
          return ids;
      }
  
  
  ```

  















#### 第5集 留给大家的作业-海量任务定时调度-处理维护三连问

**简介： 留给大家的作业-海量任务定时调度-处理维护三连问**

- 需求背景

  * 为了促进平台用户活跃度，鼓励用户每天登录网站
  * 短链平台，每天可以免费创建2次短链，每天限制次数
    * 用户量：千万级别、甚至亿级别
    * 每个用户【每天】可以免费进行2次使用平台功能【免费流量包】
    * 如果用户超过2次还想使用平台功能，则可以付费购买流量包，增加每天次数【付费流量包】



  * 正式案例
    * 百度云短链平台：https://dwz.cn/console/product

  

  * 如何维护好用户每天的免费权益次数呢 ？？？

  


  - 怎么维护更新好流量包每天的次数呢


  

- 留给老王的作业，海量数据项目大课里面的核心内容

  - 分片广播任务
  - 惰性更新+定时更新



* 业务模型适合场景

  * 拼多多用户签到，总签到次数达到一定数值后，免费领取商品，每天限制次数

  * 信用卡每天领取积分，每天限制次数

  * 电商平台抽奖，每天免费抽奖两次，每天限制次数

    

  

  

  

















#### 第6集 学完分布式调度XXL-Job后你的下一步

**简介： 学完分布式调度XXL-Job后你的下一步**


















### 第五章 分布式调度XXL-Job课程总结和学习建议



#### 第1集  分布式调度XXL-Job课程总结

**简介： 分布式调度XXL-Job课程总结**

* 知识点回顾
  * 需求背景
  * 同类解决方案
  * XXL-Job部署架构
  * SpringBoot项目整合
  * 多节点部署和调度策略
  * 控制台日志配置
  * 高可用案例
  * 分片任务实战
