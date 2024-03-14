---
draft: false 
date: 2024-03-14
categories:
  - book
comments: true
authors: [saymealoud]

---

#  Spring Boot 编程思想 - mercyblitz

Spring Boot  2013 问世, 微服务架构 Martin Fowler  等人 2014 提出



IOC  DI  颠覆 J2EE 开发模式

  “胶水框架”   



AOP 切面 使得 事物编程简化   Spring  Web  MVC



```html
Spring Framework 接口怎么简化, 设计如何优美,始终无法摆脱被动情况---- 自身并非容器, 不得不随Java EE 容器启动才能加载, Spring Boot  的出现, 改变了 Spring Framework 乃至整个 Spring 技术体系的现状
```



- Create stand-alone Spring applications
- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
- Provide opinionated 'starter' dependencies to simplify your build configuration
- Automatically configure Spring and 3rd party libraries whenever possible
- Provide production-ready features such as metrics, health checks, and externalized configuration
- Absolutely no code generation and no requirement for XML configuration



- 创建独立的 Spring 应用程序 直接嵌入 Tomcat、Jetty 或 Undertow（无需部署 WAR 文件）
-  提供固化的 “starter” 依赖项以简化构建配置 
- 当条件满足时自动装配 Spring 或第三方类库 
- 提供运维 (production-ready)特性，如指标信息(Metrics )、健康检查及外部化配置 
- 绝无生成代码，并且不需要 XML 配置

## Chapter 6

#### 约定大于配置

> "Absolutely no code generation and no requirement for XML configuration"

<p>Spring Boot 重回 main 方法的启动方式,让自己找到了初学 Java 时的记忆,尽管不少人的开发模式习惯以XML文件配置启动,在逐渐转化为注解驱动的过程,以为注解驱动时Spring Boot引入的,说明了Spring Boot 与 Spring Framework 的关系是 “你中有我,我中有你”</p>



<p>从技术的角度来叙述,Spring Framework 是Spring Boot的“基础设施” Spring Boot 的基本特性均来自 Spring Framework.Spring Boot不但无法取代 Spring Framework ,还实现了 Spring Framework 的"自我救赎". </p>



从Spring Framework 2.5 开始, Spring Bean 注册方式由 Annotation 驱动逐步替代 XML 文件驱动, 通过  **@Component**  及 “派生” 注解 (**@Service**) 与 XML 元素 `context:component-scan base-package="..."` 互相配合,将 Spring **@Component** Bean 扫描并注册至 Spring Bean 容器 (BeanFactory), 通过 DI 注解**@Autowired** 获取相应的Spring 组件 Bean. 然而 **@Component** Bean 必须在 `context:component-scan` 规定的`base-package` 集合范围中.



<p>到了 Spring Framework  3.0 时代,新引入的 Annotation @Configuration 是 XML 配置文件的替代物. 而 Bean 的定义不在需要在 XML 文件中声明 <Bean> 元素  可使用 @Bean 来代替.同时,框架提供了更细粒度的  Annotation @Import 来倒入 @Configuration Class ,将其注册为 Spring Bean,并进一步解析其声明的 Spring Bean 注册相关的注解, 如 @Import 或 @Bean.尽管在 Bean 的装配方面有明显提升, 但是仍是以硬编码的方式指定范围  </p>



当 Spring Framework 3.1 发布后, 虽说 3.0 到 3.1 不过是小版本的变化,然而其实际更新动作并不小, 此时的 Spring 注解才完全取代 XML 配置文件,引入 Annotation **@ComponentScan** 来代替 XML 元素 `context:component-scan`  , 初看起来并没有多少提升.应用方可以实现 ImportSelector 接口 (实现 selectImports(AnnotationMetadata)方法),程序动态地决定哪些 Spring  Bean 需要被导入. 可是 ImportSelector  实现类必须暴露成 Spring Bean,否则  selectImports(AnnotationMetadata)方法) 不会被 Spring 容器调用, 为了简化实现成本, Spring Framework  3.1 内建了不少的功能 “模块” 激活的注解, 如 @EnableCache.即便如此,被标注 **@EnableCache** 的类同样需要通过 **@ComponentScan** 或 **@Import** 等方式被 Spring 容器感知, 这就又成了 “先有鸡还是先有蛋”的问题.硬编码的问题得到了缓解,但仍然与自动装配的能力相去甚远.



Spring Framework 到了 4.0 版本增加了条件化的 Spring Bean 装配注解 **@Conditional** , 其 value() 属性可指定 Condition 的实现类, 而 Condition 提供装配条件的实现逻辑,  **@Conditional** 的引入更直观表达了 Spring Bean 装载时所需的前置条件.或许 Spring Boot 官方对自动化装配的后半段描述容易让人忽视-------“ whenever possible ” 正因为 Spring Framework 条件注解 **@Conditional** 的引入,使得条件装配成为可能, Spring Boot 在此基础上, 实现了最显著特性之一-----**条件化自动装配**.