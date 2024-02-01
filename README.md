# [芋道源码 —— 纯源码解析博客](https://www.iocoder.cn/ "芋道源码 —— 纯源码解析博客")

愿半生编码，如一生老友！

- [文章](https://www.iocoder.cn/)
- [知识星球](https://www.iocoder.cn/zhishixingqiu)
- [Github](https://github.com/YunaiV)
- [微信公众号](http://www.iocoder.cn/images/common/wechat_mp_2017_07_31_bak.jpg)
- [工作内推](https://www.iocoder.cn/categories/%E5%B7%A5%E4%BD%9C%E5%86%85%E6%8E%A8/)
- [友链](https://www.iocoder.cn/link_url)
- [大厂面试必备](https://www.iocoder.cn/Interview/good-collection/?top)
- [Java 超神之路](http://www.iocoder.cn/zhishixingqiu?top)

# [芋道 Spring Boot 链路追踪 SkyWalking 入门](https://www.iocoder.cn/Spring-Boot/SkyWalking/ "芋道 Spring Boot 链路追踪 SkyWalking 入门")

总阅读量:43012次

<table><tbody><tr><td><a style="color:#009688;" href="https://gitee.com/zhijiantianya/ruoyi-vue-pro" rel="external nofollow noopener noreferrer" target="_blank">⭐⭐⭐ Spring Boot 项目实战</a></td><td><a style="color:#009688;" href="https://gitee.com/zhijiantianya/yudao-cloud" rel="external nofollow noopener noreferrer" target="_blank">⭐⭐⭐ Spring Cloud 项目实战</a></td></tr><tr><td><a style="color:#009688;" href="https://www.iocoder.cn/Dubbo/good-collection/?title">《Dubbo 实现原理与源码解析 —— 精品合集》</a></td><td><a style="color:#009688;" href="https://www.iocoder.cn/Netty/Netty-collection/?title">《Netty 实现原理与源码解析 —— 精品合集》</a></td></tr><tr><td><a style="color:#009688;" href="https://www.iocoder.cn/Spring/good-collection/?title">《Spring 实现原理与源码解析 —— 精品合集》</a></td><td><a style="color:#009688;" href="https://www.iocoder.cn/MyBatis/good-collection/?title">《MyBatis 实现原理与源码解析 —— 精品合集》</a></td></tr><tr><td><a style="color:#009688;" href="https://www.iocoder.cn/Spring-MVC/good-collection/?title">《Spring MVC 实现原理与源码解析 —— 精品合集》</a></td><td><a style="color:#009688;" href="https://www.iocoder.cn/Entity/good-collection/?title">《数据库实体设计合集》</a></td></tr><tr><td><a style="color:#009688;" href="https://www.iocoder.cn/Spring-Boot/good-collection/?title">《Spring Boot 实现原理与源码解析 —— 精品合集》</a></td><td><a style="color:#009688;" href="https://www.iocoder.cn/Interview/good-collection/?title">《Java 面试题 + Java 学习指南》</a></td></tr></tbody></table>

摘要: 原创出处 http://www.iocoder.cn/Spring-Boot/SkyWalking/ 「芋道源码」欢迎转载，保留摘要，谢谢！

- [1\. 概述](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [2\. SpringMVC 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [3\. 忽略部分 URL 的追踪](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [4\. MySQL 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [5\. Redis 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [6\. MongoDB 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [7\. Elasticsearch 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [8\. RocketMQ 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [9\. Kafka 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [10\. RabbitMQ 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [11\. ActiveMQ 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [12\. 日志框架示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [13\. 自定义追踪方法](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [14\. OpenTracing 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [15\. Spring 异步任务示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [16\. Dubbo 示例](http://www.iocoder.cn/Spring-Boot/SkyWalking/)
- [666\. 彩蛋](http://www.iocoder.cn/Spring-Boot/SkyWalking/)

-

-

> 本文在提供完整代码示例，可见 [https://github.com/YunaiV/SpringBoot-Labs](https://github.com/YunaiV/SpringBoot-Labs) 的 [lab-39](https://github.com/YunaiV/SpringBoot-Labs/tree/master/lab-39) 目录。
> 
> 原创不易，给点个 [Star](https://github.com/YunaiV/SpringBoot-Labs/stargazers) 嘿，一起冲鸭！

# 1\. 概述

如果胖友还没了解过分布式链路追踪 [SkyWalking](http://skywalking.apache.org/)，建议先阅读下艿艿写的 [《芋道 SkyWalking 极简入门》](http://www.iocoder.cn/SkyWalking/install/?self) 文章。虽然这篇文章标题是安装部署，实际可以理解成《一文带你快速入门 SkyWalking》，哈哈哈。

可能会有胖友会有疑惑，Spring Boot 不是一个单体应用么，为什么可以使用 SkyWalking 进行分布式链路追踪呢？其实这里有一个**误区**！即使是一个 Spring Boot 单体应用，我们一般可能会和以下服务打交道：

- 关系数据库，例如说 MySQL、Oracle、SQLServer、PostgreSQL 等等。
- 文档数据库，例如说 MongoDB 等等。
- 缓存数据库，例如说 Redis、Memcached 等等。
- 外部三方服务，例如说微信公众号、微信支付、支付宝支付、短信平台等等。

那么即使是一个 Spring Boot 单体应用，就已经涉及到分布在**不同进程**中的服务。此时，就已经满足使用 SkyWalking 进行分布式链路追踪的条件，**同时也是非常有必要的**。例如说，我们线上某个 API 接口访问非常慢，可以通过 SkyWalking 来排查，是因为 MySQL 查询比较慢呢，还是调用的第三方服务比较慢。

在本文中，我们会比[《芋道 SkyWalking 极简入门》](http://www.iocoder.cn/SkyWalking/install/?self)提供更多在 Spring Boot 中使用的示例。例如说：

- 对 SpringMVC 的 API 接口的链路追踪
- 对 JDBC 访问 MySQL 的链路追踪
- 对 Jedis 访问 Redis 的链路追踪
- 对 RocketMQ 的消息的发送和消费的链路追踪
- 等等等等

# 2\. SpringMVC 示例

> 示例代码对应仓库：[lab-39-springmvc](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/)。

本小节，我们来搭建一个 SkyWalking 对 SpringMVC 的 API 接口的链路追踪。该链路通过如下插件实现收集：

- [`spring/mvc-annotation-3.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/spring-plugins/mvc-annotation-3.x-plugin)
- [`spring/mvc-annotation-4.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/spring-plugins/mvc-annotation-4.x-plugin)
- [`spring/mvc-annotation-5.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/spring-plugins/mvc-annotation-5.x-plugin)

## 2.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">packaging</span>&gt;</span>jar<span class="tag">&lt;/<span class="name">packaging</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-springmvc<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 2.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/src/main/resources/application.yml) 中，添加服务器端口配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br></pre></td></tr></tbody></table>

- 设置服务器的端口为 8079 ，避免和 SkyWalking UI 占用的 8080 冲突。

## 2.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/tree/master/lab-39/lab-39-springmvc/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/echo"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"echo"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/hello"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">hello</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"hello"</span>;</span><br><span class="line">    }</span><br><span class="line">    </span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 2.4 Application

创建 [`Application.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/Application.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">Application</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(Application.class, args);</span><br><span class="line">    }</span><br><span class="line">    </span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 2.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/01.png)

然后，执行 `Application#main(String[] args)` 方法，启动该 Spring Boot 应用。如果说控制台打印如下日志，说明 SkyWalking Agent 基本加载成功：

<table><tbody><tr><td class="code"><pre><span class="line"># 加载 SkyWalking Agent</span><br><span class="line">DEBUG 2020-01-04 09:42:46:091 main AgentPackagePath : The beacon class location is jar:file:/Users/yunai/skywalking/apache-skywalking-apm-bin-es7/agent/skywalking-agent.jar!/org/apache/skywalking/apm/agent/core/boot/AgentPackagePath.class. </span><br><span class="line">INFO <span class="number">2020</span>-<span class="number">01</span>-<span class="number">04</span> <span class="number">09</span>:<span class="number">42</span>:<span class="number">46</span>:<span class="number">094</span> main SnifferConfigInitializer : Config file found in /Users/yunai/skywalking/apache-skywalking-apm-bin-es7/agent/config/agent.config.</span><br></pre></td></tr></tbody></table>

## 2.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/echo](http://127.0.0.1:8079/demo/echo) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/02.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 SpringMVC 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/03.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/04.png)

# 3\. 忽略部分 URL 的追踪

> 示例代码对应仓库：[lab-39-springmvc](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/) 。

在[「2. SpringMVC 示例」](https://www.iocoder.cn/Spring-Boot/SkyWalking/#)小节中，我们已经实现了对 SpringMVC 提供的 HTTP API 接口的链路追踪。但是，我们可能希望忽略部分特殊 URL 的追踪，例如说，健康检查的 HTTP API。

所以，我们可以使用 SkyWalking 提供 [`trace-ignore-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/optional-plugins/trace-ignore-plugin) 插件，可以实现忽略部分 URL 的追踪。

本小节，我们无需单独搭建项目，而是直接使用 [lab-39-springmvc](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/) 项目即可。

## 3.1 复制插件

[`trace-ignore-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/optional-plugins/trace-ignore-plugin) 插件，在 `optional-plugins` 目录下，是**可选**插件，所以我们需要复制到 `plugins` 目录下。命令行操作如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment"># 查看当前所在目录</span></span><br><span class="line">$ <span class="built_in">pwd</span></span><br><span class="line">/Users/yunai/skywalking/apache-skywalking-apm-bin-es7/agent</span><br><span class="line"></span><br><span class="line"><span class="comment"># 复制插件到 plugins 目录</span></span><br><span class="line">$ cp optional-plugins/apm-trace-ignore-plugin-6.6.0.jar plugins/</span><br></pre></td></tr></tbody></table>

## 3.2 配置文件

[`trace-ignore-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/optional-plugins/trace-ignore-plugin) 插件，读取 `apm-trace-ignore-plugin.config` 配置文件，默认情况下不存在，所以我们需要进行创建并配置。命令行操作如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment"># 查看当前所在目录</span></span><br><span class="line">$ <span class="built_in">pwd</span></span><br><span class="line">/Users/yunai/skywalking/apache-skywalking-apm-bin-es7/agent</span><br><span class="line"></span><br><span class="line"><span class="comment"># 创建 apm-trace-ignore-plugin.config 配置文件，并进行设置</span></span><br><span class="line">$ vi config/apm-trace-ignore-plugin.config</span><br></pre></td></tr></tbody></table>

配置文件内容如下：

<table><tbody><tr><td class="code"><pre><span class="line"># If the operation name of the first span is matching, this segment should be ignored</span><br><span class="line">#  ant path match style</span><br><span class="line">#  /path/?   Match any single character</span><br><span class="line">#  /path/*   Match any number of characters</span><br><span class="line">#  /path/**  Match any number of characters and support multilevel directories</span><br><span class="line">#  Multiple path comma separation, like trace.ignore_path=/eureka/**,/consul/**</span><br><span class="line">trace.ignore_path=${SW_AGENT_TRACE_IGNORE_PATH:/eureka/**}</span><br></pre></td></tr></tbody></table>

- `trace.ignore_path` 配置项，设置忽略的 URL 路径，基于 [Ant 风格路径表达式](https://www.jianshu.com/p/189847a7d1c7)。
- 这里，我们配置了读取环境变量 `SW_AGENT_TRACE_IGNORE_PATH` ，这样方便我们自定义。

## 3.3 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/11.png)

然后，执行 `Application#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 3.4 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/hello](http://127.0.0.1:8079/demo/hello) 地址，请求下 Spring Boot 应用提供的 API。

2、然后，查看 SkyWaking Agent 日志，可以看到该 URL 的链路追踪被忽略日志。操作命令如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment"># 查看当前所在目录</span></span><br><span class="line">$ <span class="built_in">pwd</span></span><br><span class="line">/Users/yunai/skywalking/apache-skywalking-apm-bin-es7/agent</span><br><span class="line"></span><br><span class="line"><span class="comment"># 查看 SkyWaking Agent 日志，然后搜 TraceIgnoreExtendService 关键字</span></span><br><span class="line">$ vi logs/skywalking-api.log +</span><br><span class="line"><span class="comment"># 日志示例</span></span><br><span class="line">DEBUG 2020-01-04 13:07:41:720 http-nio-8079-exec-4 TraceIgnoreExtendService : operationName : /demo/hello Ignore tracking</span><br></pre></td></tr></tbody></table>

3、之后，在 SkyWalking UI 的查看链路追踪的界面，也看不到该 URL 对应的链路数据。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/12.png)

# 4\. MySQL 示例

> 示例代码对应仓库：[lab-39-mysql](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mysql/)。

本小节，我们来搭建一个 SkyWalking 对 MySQL 操作的链路追踪。该链路通过如下插件实现收集：

- [`jdbc-commons`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/jdbc-commons/)
- [`mysql-common`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/mysql-common)
- [`mysql-5.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/mysql-5.x-plugin)
- [`mysql-6.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/mysql-6.x-plugin)
- [`mysql-8.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/mysql-8.x-plugin)

我们将使用 Spring JdbcTemplate 进行 MySQL 的操作。对 Spring JdbcTemplate 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot JdbcTemplate 入门》](http://www.iocoder.cn/Spring-Boot/JdbcTemplate/?self)文章。

## 4.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mysql/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">packaging</span>&gt;</span>jar<span class="tag">&lt;/<span class="name">packaging</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-mysql<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对数据库连接池的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-jdbc<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span> <span class="comment">&lt;!-- 本示例，我们使用 MySQL --&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>mysql<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>mysql-connector-java<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>5.1.46<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 4.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mysql/src/main/resources/application.yml) 中，添加 MySQL 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line">  <span class="comment"># datasource 数据源配置内容</span></span><br><span class="line"><span class="attr">  datasource:</span></span><br><span class="line"><span class="attr">    url:</span> <span class="attr">jdbc:mysql://127.0.0.1:3306/lab-39-mysql?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8</span></span><br><span class="line"><span class="attr">    driver-class-name:</span> <span class="string">com.mysql.jdbc.Driver</span></span><br><span class="line"><span class="attr">    username:</span> <span class="string">root</span></span><br><span class="line"><span class="attr">    password:</span></span><br></pre></td></tr></tbody></table>

这里，胖友记得在测试的数据库中，创建 `t_user` 表，并插入一条 `id = 1` 的记录。SQL 脚本如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="keyword">CREATE</span> <span class="keyword">TABLE</span> <span class="string">`t_user`</span> (</span><br><span class="line">  <span class="string">`id`</span> <span class="built_in">int</span>(<span class="number">8</span>) <span class="keyword">NOT</span> <span class="literal">NULL</span> AUTO_INCREMENT <span class="keyword">COMMENT</span> <span class="string">'主键自增'</span>,</span><br><span class="line">  <span class="string">`username`</span> <span class="built_in">varchar</span>(<span class="number">50</span>) <span class="keyword">NOT</span> <span class="literal">NULL</span> <span class="keyword">COMMENT</span> <span class="string">'用户名'</span>,</span><br><span class="line">  <span class="string">`password`</span> <span class="built_in">varchar</span>(<span class="number">50</span>) <span class="keyword">NOT</span> <span class="literal">NULL</span> <span class="keyword">COMMENT</span> <span class="string">'密码'</span>,</span><br><span class="line">  PRIMARY <span class="keyword">KEY</span> (<span class="string">`id`</span>)</span><br><span class="line">) <span class="keyword">ENGINE</span>=<span class="keyword">InnoDB</span> AUTO_INCREMENT=<span class="number">1</span> <span class="keyword">DEFAULT</span> <span class="keyword">CHARSET</span>=utf8 <span class="keyword">COMMENT</span>=<span class="string">'用户表'</span>;</span><br><span class="line"></span><br><span class="line"><span class="keyword">INSERT</span> <span class="keyword">INTO</span> <span class="string">`t_user`</span>(<span class="string">`id`</span>, <span class="string">`username`</span>, <span class="string">`password`</span>) <span class="keyword">VALUES</span> (<span class="number">1</span>, <span class="string">'yudaoyuanma'</span>, <span class="string">'nicai'</span>);</span><br></pre></td></tr></tbody></table>

## 4.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mysql/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mysql/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> JdbcTemplate template;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/mysql"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">mysql</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">this</span>.selectById(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"mysql"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> Object <span class="title">selectById</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        <span class="keyword">return</span> template.queryForObject(<span class="string">"SELECT id, username, password FROM t_user WHERE id = ?"</span>,</span><br><span class="line">                <span class="keyword">new</span> BeanPropertyRowMapper&lt;&gt;(Object.class), <span class="comment">// 结果转换成对应的对象。Object 理论来说是 UserDO.class ，这里偷懒了。</span></span><br><span class="line">                id);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/mysql` 接口中，会执行一次 MySQL 的查询。

## 4.4 MySQLApplication

创建 [`MySQLApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mysql/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/MySQLApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MySQLApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(Application.class, args);</span><br><span class="line">    }</span><br><span class="line">    </span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 4.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/21.png)

然后，执行 `MySQLApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 4.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/mysql](http://127.0.0.1:8079/demo/mysql) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/22.png)

接着，点击「Database Dashboard」选项，再点击「Database」选项，可以以数据库为维度的监控数据。如下图所示：![SkyWalking UI 界面 —— 仪表盘（Database）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/23.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 MySQL 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/24.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/25.png)

点击红圈的 MySQL 操作的链路数据，可以看到数据的 SQL 语句。如下图所示：![SkyWalking UI 界面 —— 追踪（MySQL）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/26.png)

这里，我们暂时无法看到 SQL 的数据参数，可以通过修改 `config/agent.config` 配置文件，将 `plugin.mysql.trace_sql_parameters` 配置项，设置为 `true` 。例如：

<table><tbody><tr><td class="code"><pre><span class="line"># mysql plugin configuration</span><br><span class="line">plugin.mysql.trace_sql_parameters=${SW_MYSQL_TRACE_SQL_PARAMETERS:true}</span><br></pre></td></tr></tbody></table>

- 当然，也可以通过 `SW_MYSQL_TRACE_SQL_PARAMETERS` 环境变量。

# 5\. Redis 示例

> 示例代码对应仓库：[lab-39-redis](https://github.com/YunaiV/SpringBooft-Labs/blob/master/lab-39/lab-39-redis/)。

本小节，我们来搭建一个 SkyWalking 对 Redis 操作的链路追踪。该链路通过如下插件实现收集：

- [`jedis-2.x-plugin`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/jedis-2.x-plugin/)
- [`redisson-3.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/redisson-3.x-plugin)
- [`lettuce-5.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/lettuce-5.x-plugin)

我们将使用 Spring Data Redis + Jedis 进行 Redis 的操作。对 Spring Data Redis 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot Redis 入门》](http://www.iocoder.cn/Spring-Boot/Redis/?self)文章。

## 5.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-redis/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-redis<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 Spring Data Redis 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-data-redis<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">exclusions</span>&gt;</span></span><br><span class="line">                <span class="comment">&lt;!-- 去掉对 Lettuce 的依赖，因为 Spring Boot 优先使用 Lettuce 作为 Redis 客户端 --&gt;</span></span><br><span class="line">                <span class="tag">&lt;<span class="name">exclusion</span>&gt;</span></span><br><span class="line">                    <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>io.lettuce<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">                    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lettuce-core<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">                <span class="tag">&lt;/<span class="name">exclusion</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;/<span class="name">exclusions</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 引入 Jedis 的依赖，这样 Spring Boot 实现对 Jedis 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>redis.clients<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>jedis<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 5.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-redis/src/main/resources/application.yml) 中，添加 Redis 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line">  <span class="comment"># 对应 RedisProperties 类</span></span><br><span class="line"><span class="attr">  redis:</span></span><br><span class="line"><span class="attr">    host:</span> <span class="number">127.0</span><span class="number">.0</span><span class="number">.1</span></span><br><span class="line"><span class="attr">    port:</span> <span class="number">6379</span></span><br><span class="line"><span class="attr">    password:</span> <span class="comment"># Redis 服务器密码，默认为空。生产中，一定要设置 Redis 密码！</span></span><br><span class="line"><span class="attr">    database:</span> <span class="number">0</span> <span class="comment"># Redis 数据库号，默认为 0 。</span></span><br><span class="line"><span class="attr">    timeout:</span> <span class="number">0</span> <span class="comment"># Redis 连接超时时间，单位：毫秒。</span></span><br><span class="line">    <span class="comment"># 对应 RedisProperties.Jedis 内部类</span></span><br><span class="line"><span class="attr">    jedis:</span></span><br><span class="line"><span class="attr">      pool:</span></span><br><span class="line"><span class="attr">        max-active:</span> <span class="number">8</span> <span class="comment"># 连接池最大连接数，默认为 8 。使用负数表示没有限制。</span></span><br><span class="line"><span class="attr">        max-idle:</span> <span class="number">8</span> <span class="comment"># 默认连接数最小空闲的连接数，默认为 8 。使用负数表示没有限制。</span></span><br><span class="line"><span class="attr">        min-idle:</span> <span class="number">0</span> <span class="comment"># 默认连接池最小空闲的连接数，默认为 0 。允许设置 0 和 正数。</span></span><br><span class="line"><span class="attr">        max-wait:</span> <span class="bullet">-1</span> <span class="comment"># 连接池最大阻塞等待时间，单位：毫秒。默认为 -1 ，表示不限制。</span></span><br></pre></td></tr></tbody></table>

## 5.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-redis/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-redis/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> StringRedisTemplate redisTemplate;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/redis"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">redis</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">this</span>.get(<span class="string">"demo"</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"redis"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">void</span> <span class="title">get</span><span class="params">(String key)</span> </span>{</span><br><span class="line">        redisTemplate.opsForValue().get(key);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/redis` 接口中，会执行一次 Redis GET 操作。

## 5.4 RedisApplication

创建 [`RedisApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-redis/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/RedisApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">RedisApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(RedisApplication.class, args);</span><br><span class="line">    }</span><br><span class="line">    </span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 5.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/31.png)

然后，执行 `RedisApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 5.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/redis](http://127.0.0.1:8079/demo/redis) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/32.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 Redis 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/33.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/34.png)

点击红圈的 Redis 操作的链路数据，可以看到 Redis 具体操作。如下图所示：![SkyWalking UI 界面 —— 追踪（Redis）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/35.png)

不过没有看到 Redis 操作的具体参数。因为 Spring Data Redis 会把提前具体参数转换成二进制数组，导致 `jedis-2.x-plugin` 插件的 [JedisMethodInterceptor](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/jedis-2.x-plugin/src/main/java/org/apache/skywalking/apm/plugin/jedis/v2/JedisMethodInterceptor.java) 不进行收集。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment">// JedisMethodInterceptor.java</span></span><br><span class="line"></span><br><span class="line"><span class="comment">// String 类型，则进行收集</span></span><br><span class="line"><span class="keyword">if</span> (allArguments.length &gt; <span class="number">0</span> &amp;&amp; allArguments[<span class="number">0</span>] <span class="keyword">instanceof</span> String) {</span><br><span class="line">    Tags.DB_STATEMENT.set(span, method.getName() + <span class="string">" "</span> + allArguments[<span class="number">0</span>]);</span><br><span class="line"><span class="comment">// byte[] 类型，则不进行收集</span></span><br><span class="line">} <span class="keyword">else</span> <span class="keyword">if</span> (allArguments.length &gt; <span class="number">0</span> &amp;&amp; allArguments[<span class="number">0</span>] <span class="keyword">instanceof</span> <span class="keyword">byte</span>[]) {</span><br><span class="line">    Tags.DB_STATEMENT.set(span, method.getName());</span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

# 6\. MongoDB 示例

> 示例代码对应仓库：[lab-39-mongodb](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mongodb/)。

本小节，我们来搭建一个 SkyWalking 对 MongoDB 操作的链路追踪。该链路通过如下插件实现收集：

- [`mongodb-2.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/mongodb-2.x-plugin)
- [`mongodb-3.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/mongodb-3.x-plugin)

我们将使用 Spring Data MongoDB 进行 MongoDB 的操作。对 Spring Data MongoDB 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot MongoDB 入门》](http://www.iocoder.cn/Spring-Boot/MongoDB/?self)文章。

## 6.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mongodb/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-mongodb<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 自动化配置 Spring Data Mongodb --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-data-mongodb<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 6.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mongodb/src/main/resources/application.yml) 中，添加 Redis 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line"><span class="attr">  data:</span></span><br><span class="line">    <span class="comment"># MongoDB 配置项，对应 MongoProperties 类</span></span><br><span class="line"><span class="attr">    mongodb:</span></span><br><span class="line"><span class="attr">      host:</span> <span class="number">127.0</span><span class="number">.0</span><span class="number">.1</span></span><br><span class="line"><span class="attr">      port:</span> <span class="number">27017</span></span><br><span class="line"><span class="attr">      database:</span> <span class="string">yourdatabase</span></span><br><span class="line"><span class="attr">      username:</span> <span class="string">test01</span></span><br><span class="line"><span class="attr">      password:</span> <span class="string">password01</span></span><br><span class="line">      <span class="comment"># 上述属性，也可以只配置 uri</span></span><br></pre></td></tr></tbody></table>

## 6.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mongodb/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mongodb/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> MongoTemplate mongoTemplate;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/mongodb"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">mysql</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">this</span>.findById(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"mongodb"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> UserDO <span class="title">findById</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        <span class="keyword">return</span> mongoTemplate.findOne(<span class="keyword">new</span> Query(Criteria.where(<span class="string">"_id"</span>).is(id)), UserDO.class);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/mongodb` 接口中，会执行一次 MongoDB 查询操作。
- [UserDO](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mongodb/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/dataobject/UserDO.java) 实体类，直接点击查看。

## 6.4 MongoDBApplication

创建 [`MongoDBApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-mongodb/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/MongoDBApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment">// Application.java</span></span><br><span class="line"></span><br><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">MongoDBApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(MongoDBApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 6.5 插件配置

默认情况下，SkyWalking Agent MongoDB 插件，**不记录操作参数**，需要通过配置。命令行操作如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment"># 查看当前所在目录</span></span><br><span class="line">$ <span class="built_in">pwd</span></span><br><span class="line">/Users/yunai/skywalking/apache-skywalking-apm-bin-es7/agent</span><br><span class="line"></span><br><span class="line"><span class="comment"># 需改 agent.config 配置文件，并进行设置</span></span><br><span class="line">$ vi config/agent.config</span><br></pre></td></tr></tbody></table>

**额外**新增如下配置内容如下：

<table><tbody><tr><td class="code"><pre><span class="line"># mongodb plugin configuration</span><br><span class="line">plugin.mongodb.trace_param=${SW_MONGODB_TRACE_PARAM:false}</span><br></pre></td></tr></tbody></table>

- 这样，默认情况下还是保持不记录操作参数，可通过 `SW_MONGODB_TRACE_PARAM` 环境变量开启。

## 6.6 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/41.png)

然后，执行 `MongoDBApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 6.7 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/mongodb](http://127.0.0.1:8079/demo/mongodb) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/42.png)

接着，点击「Database Dashboard」选项，再点击「Database」选项，可以以数据库为维度的监控数据。如下图所示：![SkyWalking UI 界面 —— 仪表盘（Database）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/43.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 MongoDB 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/44.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/45.png)

点击红圈的 MongoDB 操作的链路数据，可以看到操作语句。如下图所示：![SkyWalking UI 界面 —— 追踪（MySQL）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/46.png)

# 7\. Elasticsearch 示例

> 示例代码对应仓库：[lab-39-elasticsearch-jest](https://github.com/YunaiV/SpringBoot-Labs/tree/master/lab-39/lab-39-elasticsearch-jest/)。

本小节，我们来搭建一个 SkyWalking 对 Elasticsearch 操作的链路追踪。该链路通过如下插件实现收集：

- [`elasticsearch-5.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/elasticsearch-5.x-plugin)
- [`elasticsearch-6.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/elasticsearch-6.x-plugin)

不过略微翻车了。原因是：

- SkyWalking `6.6.0` 版本的 `elasticsearch-5.x-plugin` 插件，暂时只支持 [`transport-client`](https://github.com/elastic/elasticsearch/tree/master/client/transport) `5.2.x`\-`5.6.x` 版本。什么意思呢？如果使用 Elasticsearch TCP 协议的客户端，不能使用 `6.X` 开始的版本。
- SkyWalking `6.6.0` 版本的 `elasticsearch-6.x-plugin` 插件，暂时只支持 Elasticsearch Rest 协议的客户端。

听起来好像也没啥问题？目前项目中，一般不会直接使用 Elasticsearch 原生的 TCP 协议和 Rest 协议的客户端，而是采用 [Spring Data Elasticsearch](https://spring.io/projects/spring-data-elasticsearch) 库。该库的主流版本是基于 Elasticsearch TCP 协议的客户端，并且使用 `transport-client` 的 `6.X` 开始的版本。因此，在我们使用 Spring Data Elasticsearch **主流版本**的情况下，SkyWalking 暂时无法实现对 Elasticsearch 的链路追踪。具体艿艿测试的代码示例，可见 [lab-39-elasticsearch](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch/) 仓库。😈 等后面 SkyWalking 提供了支持，艿艿再来更新一波。

不过，目前 Elasticsearch 官方较为推荐采用它提供的 Rest 协议。所以我们也可以采用 [Spring Data Jest](https://github.com/VanRoy/spring-data-jest) 库。而 Spring Data Jest 是基于 [Jest](https://github.com/searchbox-io/Jest) 之上，对 [Spring Data](https://spring.io/projects/spring-data) 的实现。Jest 是基于 Elasticsearch Rest 协议的第三方客户端，其内部采用 [HttpClient](https://hc.apache.org/) 发起 HTTP 请求。因此，我们可以采用 SkyWalking 的 [`httpClient-4.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/httpClient-4.x-plugin) 插件，间接实现对 Elasticsearch 的链路追踪。咳咳咳 😈

因此，我们最终使用 Spring Data Jest 进行 Elasticsearch 的操作。对 Elasticsearch 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot Elasticsearch 入门》](http://www.iocoder.cn/Spring-Boot/Elasticsearch/?self)文章。

## 7.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.1.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-elasticsearch-jest<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 自动化配置 Spring Data Jest --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>com.github.vanroy<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-data-jest<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>3.3.0.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

因为 Spring Data Jest `3.3.0.RELEASE` 版本，最高只能支持 Elasticsearch `6.8.4` 版本，所以 😭 艿艿又不得不搭建一个 Elasticsearch `6.8.4` 版本的服务。心疼自己 5 秒钟，各种倒腾。

## 7.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/src/main/resources/application.yml) 中，添加 Redis 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line"><span class="attr">  data:</span></span><br><span class="line">    <span class="comment"># Jest 配置项</span></span><br><span class="line"><span class="attr">    jest:</span></span><br><span class="line"><span class="attr">      uri:</span> <span class="attr">http://127.0.0.1:9400</span></span><br></pre></td></tr></tbody></table>

## 7.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> ESUserRepository userRepository;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/elasticsearch"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">mysql</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">this</span>.save(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">this</span>.findById(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"elasticsearch"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">void</span> <span class="title">save</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        ESUserDO user = <span class="keyword">new</span> ESUserDO();</span><br><span class="line">        user.setId(id);</span><br><span class="line">        user.setUsername(<span class="string">"username："</span> + id);</span><br><span class="line">        user.setPassword(<span class="string">"password："</span> + id);</span><br><span class="line">        user.setCreateTime(<span class="keyword">new</span> Date());</span><br><span class="line">        userRepository.save(user);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> ESUserDO <span class="title">findById</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        <span class="keyword">return</span> userRepository.findById(id).orElse(<span class="keyword">null</span>);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/elasticsearch` 接口中，会执行一次 Elasticsearch 插入和查询操作。
- [ESUserDO](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/dataobject/ESUserDO.java) 实体类，直接点击查看。
- [ESUserRepository](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/repository/ESUserRepository.java) ES 数据访问类，直接点击查看。

## 7.4 ElasticsearchJestApplication

创建 [`ElasticsearchJestApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/ElasticsearchJestApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span>(exclude = {ElasticsearchAutoConfiguration.class, ElasticsearchDataAutoConfiguration.class})</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">ElasticsearchJestApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(ElasticsearchJestApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 7.5 简单测试

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/51.png)

然后，执行 `ElasticsearchJestApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/elasticsearch](http://127.0.0.1:8079/demo/elasticsearch) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/52.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到**特殊的**小方块，就是 Elasticsearch 服务。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/53.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/54.png)

点击红圈的使用 HttpClient 发起 HTTP 请求，操作 Elasticsearch 的链路数据。如下图所示：![SkyWalking UI 界面 —— 追踪（HttpClient）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/55.png)

不过没有看到 HTTP 请求的具体参数。如果想要的话，胖友需要自行去修改 `httpClient-4.x-plugin` 插件。不过要注意，因为 HTTP 请求的具体参数可能很长，所以最好做下最大长度的截取。

# 8\. RocketMQ 示例

> 示例代码对应仓库：[lab-39-rocketmq](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/)。

本小节，我们来搭建一个 SkyWalking 对 RocketMQ 消息的发送和消费的链路追踪。该链路通过如下插件实现收集：

- [`rocketMQ-3.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/rocketMQ-3.x-plugin)
- [`rocketMQ-4.x-plugin`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/rocketMQ-4.x-plugin/)

我们将使用 [RocketMQ-Spring](https://github.com/apache/rocketmq-spring) 进行 RocketMQ 的操作。对 RocketMQ 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot 消息队列 RocketMQ 入门》](http://www.iocoder.cn/Spring-Boot/RocketMQ/?self)文章。

考虑到让示例更简单，我们的示例项目包含 RocketMQ 的生产者 Producer 和消费者 Consumer。

## 8.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-elasticsearch-jest/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.1.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-rocketmq<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 RocketMQ 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.rocketmq<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>rocketmq-spring-boot-starter<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.0.4<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 8.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/src/main/resources/application.yaml) 中，添加 RocketMQ 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="comment"># rocketmq 配置项，对应 RocketMQProperties 配置类</span></span><br><span class="line"><span class="attr">rocketmq:</span></span><br><span class="line"><span class="attr">  name-server:</span> <span class="number">127.0</span><span class="number">.0</span><span class="number">.1</span><span class="string">:9876</span> <span class="comment"># RocketMQ Namesrv</span></span><br><span class="line">  <span class="comment"># Producer 配置项</span></span><br><span class="line"><span class="attr">  producer:</span></span><br><span class="line"><span class="attr">    group:</span> <span class="string">demo-producer-group</span> <span class="comment"># 生产者分组</span></span><br><span class="line"><span class="attr">    send-message-timeout:</span> <span class="number">3000</span> <span class="comment"># 发送消息超时时间，单位：毫秒。默认为 3000 。</span></span><br></pre></td></tr></tbody></table>

## 8.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/rocketmqdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/rocketmqdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> DemoProducer producer;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/rocketmq"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">this</span>.sendMessage(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"rocketmq"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">void</span> <span class="title">sendMessage</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        producer.syncSend(id);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/rocketmq` 接口中，会执行一次 RocketMQ 发送消息的操作。
- [DemoMessage](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/rocketmqdemo/message/DemoMessage.java) 消息类，直接点击查看。
- [DemoProducer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/rocketmqdemo/producer/DemoProducer.java) 生产者类，直接点击查看。
- [DemoConsumer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/rocketmqdemo/consumer/DemoConsumer.java) 消费者类，直接点击查看。

## 8.4 RocketMQApplication

创建 [`RocketMQApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rocketmq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/rocketmqdemo/RocketMQApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">RocketMQApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(RocketMQApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 8.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/61.png)

然后，执行 `RocketMQApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 8.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/rocketmq](http://127.0.0.1:8079/demo/rocketmq) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/62.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 RocketMQ 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/63.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/64.png)

**注意，可以看到 RocketMQ 消息的发送，到该消息的异步的消费，都被串联在一个整体链路中**。这样对我们日常排查问题，会非常便捷舒服。

点击 RocketMQ Producer 发送消息的链路数据，可以看到 Producer 发送消息的 Topic。如下图所示：![SkyWalking UI 界面 —— 追踪（RocketMQ Producer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/65.png)

点击 RocketMQ Consumer 消费消息的链路数据，可以看到 Consumer 消费消息的 Topic。如下图所示：![SkyWalking UI 界面 —— 追踪（RocketMQ Consumer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/66.png)

## 8.7 阿里云的消息队列 ONS 服务

可能部分胖友采用的是阿里云的消息队列 ONS 服务，使用的是 [`ons-client`](https://mvnrepository.com/artifact/com.aliyun.openservices/ons-client) 库进行 RocketMQ 消息的发送和消费。那么此时，我们就无法使用 SkyWalking 进行 RocketMQ 的链路追踪了。怎么解决呢？

**① 方案一**，参考 SkyWalking [`rocketMQ-4.x-plugin`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/rocketMQ-4.x-plugin/) 插件的源码，修改成支持 `ons-client` 库的链路追踪。`ons-client` 库的代码，和 [`rocketmq-client`](https://mvnrepository.com/artifact/org.apache.rocketmq/rocketmq-client) 的代码是非常接近的，胖友只要稍微改改就能支持。

**② 方案二**，阿里云的消息队列 ONS 服务，已经支持 `rocketmq-client` 进行 RocketMQ 消息的发送和消费。这样，我们就可以继续使用 SkyWalking `rocketMQ-4.x-plugin` 插件来进行链路追踪。

不过要注意，阿里云消息 ONS 服务**大于**开源的 RocketMQ 中间件，所以用到 ONS 服务的**独有**的特性的功能，还是需要使用 `ons-client` 库。

目前艿艿线上所采用的是**方案一**，新建了一个 `ons-1.x-plugin` 插件，实现了对 ONS 服务的链路追踪。

# 9\. Kafka 示例

> 示例代码对应仓库：[lab-39-kafka](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/)。

本小节，我们来搭建一个 SkyWalking 对 Kafka 消息的发送和消费的链路追踪。该链路通过如下插件实现收集：

- [`kafka-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/kafka-plugin)

我们将使用 [Spring-Kafka](https://spring.io/projects/spring-kafka) 进行 Kafka 的操作。对 Kafka 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot 消息队列 Kafka 入门》](http://www.iocoder.cn/Spring-Boot/Kafka/?self)文章。

考虑到让示例更简单，我们的示例项目包含 Kafka 的生产者 Producer 和消费者 Consumer。

## 9.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.1.11.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-kafka<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 引入 Spring-Kafka 依赖 --&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 已经内置 kafka-clients 依赖，所以无需重复引入 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.kafka<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-kafka<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.11.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

> 友情提示： SkyWalking `kafka-plugin` 对高版本的 [`kafka-clients`](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients) 的链路追踪兼容性还存在一点问题，无法追踪到 Kafka Producer 发送消息。
> 
> 所以，这里我们将 `spring-kafka` 的版本从 `2.3.3.RELEASE` 修改成 `2.2.11.RELEASE`，以使用 `kafka-clients` 的低版版本 `2.0.1`。

## 9.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/pom.xml) 中，添加 Kafka 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line">  <span class="comment"># Kafka 配置项，对应 KafkaProperties 配置类</span></span><br><span class="line"><span class="attr">  kafka:</span></span><br><span class="line"><span class="attr">    bootstrap-servers:</span> <span class="number">127.0</span><span class="number">.0</span><span class="number">.1</span><span class="string">:9092</span> <span class="comment"># 指定 Kafka Broker 地址，可以设置多个，以逗号分隔</span></span><br><span class="line">    <span class="comment"># Kafka Producer 配置项</span></span><br><span class="line"><span class="attr">    producer:</span></span><br><span class="line"><span class="attr">      acks:</span> <span class="number">1</span> <span class="comment"># 0-不应答。1-leader 应答。all-所有 leader 和 follower 应答。</span></span><br><span class="line"><span class="attr">      retries:</span> <span class="number">3</span> <span class="comment"># 发送失败时，重试发送的次数</span></span><br><span class="line"><span class="attr">      key-serializer:</span> <span class="string">org.apache.kafka.common.serialization.StringSerializer</span> <span class="comment"># 消息的 key 的序列化</span></span><br><span class="line"><span class="attr">      value-serializer:</span> <span class="string">org.springframework.kafka.support.serializer.JsonSerializer</span> <span class="comment"># 消息的 value 的序列化</span></span><br><span class="line">    <span class="comment"># Kafka Consumer 配置项</span></span><br><span class="line"><span class="attr">    consumer:</span></span><br><span class="line"><span class="attr">      auto-offset-reset:</span> <span class="string">earliest</span> <span class="comment"># 设置消费者分组最初的消费进度为 earliest 。可参考博客 https://blog.csdn.net/lishuangzhe7047/article/details/74530417 理解</span></span><br><span class="line"><span class="attr">      key-deserializer:</span> <span class="string">org.apache.kafka.common.serialization.StringDeserializer</span></span><br><span class="line"><span class="attr">      value-deserializer:</span> <span class="string">org.springframework.kafka.support.serializer.JsonDeserializer</span></span><br><span class="line"><span class="attr">      properties:</span></span><br><span class="line"><span class="attr">        spring:</span></span><br><span class="line"><span class="attr">          json:</span></span><br><span class="line"><span class="attr">            trusted:</span></span><br><span class="line"><span class="attr">              packages:</span> <span class="string">cn.iocoder.springboot.lab39.skywalkingdemo.message</span> <span class="comment"># 消息 POJO 可信目录，解决 JSON 无法反序列化的问题</span></span><br><span class="line">    <span class="comment"># Kafka Consumer Listener 监听器配置</span></span><br><span class="line"><span class="attr">    listener:</span></span><br><span class="line"><span class="attr">      missing-topics-fatal:</span> <span class="literal">false</span> <span class="comment"># 消费监听接口监听的主题不存在时，默认会报错。所以通过设置为 false ，解决报错</span></span><br></pre></td></tr></tbody></table>

## 9.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> DemoProducer producer;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/kafka"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> <span class="keyword">throws</span> ExecutionException, InterruptedException </span>{</span><br><span class="line">        <span class="keyword">this</span>.sendMessage(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"kafka"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">void</span> <span class="title">sendMessage</span><span class="params">(Integer id)</span> <span class="keyword">throws</span> ExecutionException, InterruptedException </span>{</span><br><span class="line">        producer.syncSend(id);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/kafka` 接口中，会执行一次 Kafka 发送消息的操作。
- [DemoMessage](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/message/DemoMessage.java) 消息类，直接点击查看。
- [DemoProducer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/producer/DemoProducer.java) 生产者类，直接点击查看。
- [DemoConsumer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/consumer/DemoConsumer.java) 消费者类，直接点击查看。

## 9.4 KafkaApplication

创建 [`KafkaApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/KafkaApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">KafkaApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(KafkaApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 9.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/71.png)

然后，执行 `KafkaApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 9.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/kafka](http://127.0.0.1:8079/demo/kafka) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/72.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 Kafka 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/73.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/74.png)

**注意，可以看到 Kafka 消息的发送，到该消息的异步的消费，都被串联在一个整体链路中**。这样对我们日常排查问题，会非常便捷舒服。

点击 Kafka Producer 发送消息的链路数据，可以看到 Kafka 发送消息的 Topic。如下图所示：![SkyWalking UI 界面 —— 追踪（Kafka Producer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/75.png)

点击 Kafka Consumer 消费消息的链路数据，可以看到 Consumer 消费消息的 Topic。如下图所示：![SkyWalking UI 界面 —— 追踪（Kafka Consumer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/76.png)

# 10\. RabbitMQ 示例

> 示例代码对应仓库：[lab-39-rabbitmq-demo](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/)。

本小节，我们来搭建一个 SkyWalking 对 Rabbitmq 消息的发送和消费的链路追踪。该链路通过如下插件实现收集：

- [`rabbitmq-5.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/rabbitmq-5.x-plugin)

我们将使用 [Spring-AMQP](https://spring.io/projects/spring-amqp) 进行 RabbitMQ 的操作。对 RabbitMQ 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot 消息队列 RabbitMQ 入门》](http://www.iocoder.cn/Spring-Boot/RabbitMQ/?self)文章。

考虑到让示例更简单，我们的示例项目包含 RabbitMQ 的生产者 Producer 和消费者 Consumer。

## 10.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.1.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-rabbitmq-demo<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 RabbitMQ 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-amqp<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 10.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-kafka/pom.xml) 中，添加 RabbitMQ 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line">  <span class="comment"># RabbitMQ 配置项，对应 RabbitProperties 配置类</span></span><br><span class="line"><span class="attr">  rabbitmq:</span></span><br><span class="line"><span class="attr">    host:</span> <span class="number">127.0</span><span class="number">.0</span><span class="number">.1</span> <span class="comment"># RabbitMQ 服务的地址</span></span><br><span class="line"><span class="attr">    port:</span> <span class="number">5672</span> <span class="comment"># RabbitMQ 服务的端口</span></span><br><span class="line"><span class="attr">    username:</span> <span class="string">guest</span> <span class="comment"># RabbitMQ 服务的账号</span></span><br><span class="line"><span class="attr">    password:</span> <span class="string">guest</span> <span class="comment"># RabbitMQ 服务的密码</span></span><br></pre></td></tr></tbody></table>

## 10.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> DemoProducer producer;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/rabbitmq"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">this</span>.sendMessage(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"rabbitmq"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">void</span> <span class="title">sendMessage</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        producer.syncSend(id);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/rabbitmq` 接口中，会执行一次 RabbitMQ 发送消息的操作。
- [RabbitConfig](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/config/RabbitConfig.java) 配置类，直接点击查看。
- [DemoMessage](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/message/DemoMessage.java) 消息类，直接点击查看。
- [DemoProducer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/producer/DemoProducer.java) 生产者类，直接点击查看。
- [DemoConsumer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/consumer/DemoConsumer.java) 消费者类，直接点击查看。

## 10.4 RabbitMQApplication

创建 [`RabbitMQApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-rabbitmq-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/RabbitMQApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">RabbitMQApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(RabbitMQApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 10.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/81.png)

然后，执行 `KafkaApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 10.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/rabbitmq](http://127.0.0.1:8079/demo/rabbitmq) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/82.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 RabbitMQ 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/83.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/84.png)

**注意，可以看到 RabbitMQ 消息的发送，到该消息的异步的消费，都被串联在一个整体链路中**。这样对我们日常排查问题，会非常便捷舒服。

点击 RabbitMQ Producer 发送消息的链路数据，可以看到 RabbitMQ 发送消息的 RoutingKey、Exchange。如下图所示：![SkyWalking UI 界面 —— 追踪（RabbitMQ Producer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/85.png)

点击 RabbitMQ Consumer 消费消息的链路数据，可以看到 Consumer 消费消息的 RoutingKey、Exchange、Queue。如下图所示：![SkyWalking UI 界面 —— 追踪（RabbitMQ Consumer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/86.png)

# 11\. ActiveMQ 示例

> 示例代码对应仓库：[lab-39-activemq](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/)。

本小节，我们来搭建一个 SkyWalking 对 ActiveMQ 消息的发送和消费的链路追踪。该链路通过如下插件实现收集：

- [`activemq-5.x-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/activemq-5.x-plugin)

我们将使用 [Spring-JMS](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/integration.html#jms) 进行 ActiveMQ 的操作。对 ActiveMQ 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot 消息队列 ActiveMQ 入门》](http://www.iocoder.cn/Spring-Boot/ActiveMQ/?self)文章。

考虑到让示例更简单，我们的示例项目包含 ActiveMQ 的生产者 Producer 和消费者 Consumer。

## 11.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.1.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-activemq<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 ActiveMQ 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-activemq<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 11.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/src/main/resources/application.yaml) 中，添加 ActiveMQ 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line">  <span class="comment"># ActiveMQ 配置项，对应 ActiveMQProperties 配置类</span></span><br><span class="line"><span class="attr">  activemq:</span></span><br><span class="line"><span class="attr">    broker-url:</span> <span class="attr">tcp://127.0.0.1:61616</span> <span class="comment"># Activemq Broker 的地址</span></span><br><span class="line"><span class="attr">    user:</span> <span class="string">admin</span> <span class="comment"># 账号</span></span><br><span class="line"><span class="attr">    password:</span> <span class="string">admin</span> <span class="comment"># 密码</span></span><br><span class="line"><span class="attr">    packages:</span></span><br><span class="line"><span class="attr">      trust-all:</span> <span class="literal">true</span> <span class="comment"># 可信任的反序列化包</span></span><br></pre></td></tr></tbody></table>

## 11.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-ActiveMQ-demo/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> DemoProducer producer;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/activemq"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="keyword">this</span>.sendMessage(<span class="number">1</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"activemq"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">void</span> <span class="title">sendMessage</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        producer.syncSend(id);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/activemq` 接口中，会执行一次 ActiveMQ 发送消息的操作。
- [DemoMessage](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/message/DemoMessage.java) 消息类，直接点击查看。
- [DemoProducer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/producer/DemoProducer.java) 生产者类，直接点击查看。
- [DemoConsumer](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/consumer/DemoConsumer.java) 消费者类，直接点击查看。

## 11.4 ActiveMQApplication

创建 [`ActiveMQApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-activemq/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/ActiveMQApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">ActiveMQApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(ActiveMQApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 11.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/91.png)

然后，执行 `ActiveMQApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 11.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/activemq](http://127.0.0.1:8079/demo/activemq) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/92.png)

3、之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到 ActiveMQ 小方块。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/93.png)

4、再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/94.png)

**注意，可以看到 ActiveMQ 消息的发送，到该消息的异步的消费，都被串联在一个整体链路中**。这样对我们日常排查问题，会非常便捷舒服。

点击 ActiveMQ Producer 发送消息的链路数据，可以看到 ActiveMQ 发送消息的 Queue。如下图所示：![SkyWalking UI 界面 —— 追踪（ActiveMQ Producer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/95.png)

点击 ActiveMQ Consumer 消费消息的链路数据，可以看到 Consumer 消费消息的 Queue。如下图所示：![SkyWalking UI 界面 —— 追踪（ActiveMQ Consumer）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/96.png)

# 12\. 日志框架示例

> 示例代码对应仓库：[lab-39-logback](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-logback/)。

在使用 SkyWalking 排查问题的时候，我们可能希望能够跟链路的**日志**进行关联，那么我们可以将链路编号( SkyWalking TraceId )记录到日志中，从而进行关联。

> 友情提示：艿艿自己的项目里，在一些业务数据希望跟 SkyWalking 链路进行关联时，会考虑新增一个 `traceId` 字段，存储 SkyWalking TraceId。例如说：
> 
> - 发送聊天消息时，消息记录上会存储链路编号。
> - 创建交易订单时，订单记录上会存储链路编号。
> 
> 这样，在排查该数据记录时，我们就可以拿着 `traceId` 字段，去查响应的链路信息和日志信息。

SkyWalking 提供了多种日志框架的支持，通过不同的插件：

- [`apm-toolkit-log4j-1.x-activation`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-toolkit-activation/apm-toolkit-log4j-1.x-activation) ：支持 Log4j1 日志框架，对应[《Application-toolkit-log4j-1.x.md》](https://github.com/SkyAPM/document-cn-translation-of-skywalking/blob/master/docs/zh/master/setup/service-agent/java-agent/Application-toolkit-log4j-1.x.md)文档。
- [`apm-toolkit-log4j-2.x-activation`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-toolkit-activation/apm-toolkit-log4j-1.x-activation) ：支持 Log4j2 日志框架，对应[《Application-toolkit-log4j-2.x.md》](https://github.com/SkyAPM/document-cn-translation-of-skywalking/blob/master/docs/zh/master/setup/service-agent/java-agent/Application-toolkit-log4j-2.x.md)文档。
- [`apm-toolkit-logback-1.x-activation`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-toolkit-activation/apm-toolkit-logback-1.x-activation/pom.xml) ：支持 Logback 日志框架，对应[《Application-toolkit-logback-1.x.md》](https://github.com/SkyAPM/document-cn-translation-of-skywalking/blob/master/docs/zh/master/setup/service-agent/java-agent/Application-toolkit-logback-1.x.md)文档。

本小节，我们来搭建一个 SLF4J + Logback 日志的 SkyWalking TraceId 的集成示例。对 Logging 感兴趣的胖友，可以后续去看看[《芋道 Spring Boot 日志框架 Logging 入门》](http://www.iocoder.cn/Spring-Boot/Logging/?self)文章。

## 12.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-logback/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">packaging</span>&gt;</span>jar<span class="tag">&lt;/<span class="name">packaging</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-logback<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- SkyWalking 对 Logback 的集成 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.skywalking<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>apm-toolkit-logback-1.x<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>6.6.0<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 12.2 应用配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-logback/src/main/resources/application.yml) 中，添加配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line"><span class="attr">  application:</span></span><br><span class="line"><span class="attr">    name:</span> <span class="string">demo-application-logback</span></span><br></pre></td></tr></tbody></table>

## 12.3 Logback 配置文件

在 [`logback-spring.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-logback/src/main/resources/logback-spring.xml) 中，添加 Logback 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;<span class="name">configuration</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="comment">&lt;!-- 引入 Spring Boot 默认的 logback XML 配置文件  --&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">include</span> <span class="attr">resource</span>=<span class="string">"org/springframework/boot/logging/logback/defaults.xml"</span>/&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="comment">&lt;!-- 控制台 Appender --&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">property</span> <span class="attr">name</span>=<span class="string">"CONSOLE_LOG_PATTERN"</span> <span class="attr">value</span>=<span class="string">"%clr(%d{${LOG_DATEFORMAT_PATTERN:-yyyy-MM-dd HH:mm:ss.SSS}}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %tid %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}"</span>/&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">appender</span> <span class="attr">name</span>=<span class="string">"console"</span> <span class="attr">class</span>=<span class="string">"ch.qos.logback.core.ConsoleAppender"</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 日志的格式化 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">encoder</span> <span class="attr">class</span>=<span class="string">"ch.qos.logback.core.encoder.LayoutWrappingEncoder"</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">layout</span> <span class="attr">class</span>=<span class="string">"org.apache.skywalking.apm.toolkit.log.logback.v1.x.TraceIdPatternLogbackLayout"</span>&gt;</span></span><br><span class="line">                <span class="tag">&lt;<span class="name">Pattern</span>&gt;</span>${CONSOLE_LOG_PATTERN}<span class="tag">&lt;/<span class="name">Pattern</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;/<span class="name">layout</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">encoder</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">appender</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="comment">&lt;!-- 从 Spring Boot 配置文件中，读取 spring.application.name 应用名 --&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">springProperty</span> <span class="attr">name</span>=<span class="string">"applicationName"</span> <span class="attr">scope</span>=<span class="string">"context"</span> <span class="attr">source</span>=<span class="string">"spring.application.name"</span> /&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">property</span> <span class="attr">name</span>=<span class="string">"FILE_LOG_PATTERN"</span> <span class="attr">value</span>=<span class="string">"%d{${LOG_DATEFORMAT_PATTERN:-yyyy-MM-dd HH:mm:ss.SSS}} ${LOG_LEVEL_PATTERN:-%5p} ${PID:- } %tid --- [%t] %-40.40logger{39} : %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}"</span>/&gt;</span></span><br><span class="line">    <span class="comment">&lt;!-- 日志文件的路径 --&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">property</span> <span class="attr">name</span>=<span class="string">"LOG_FILE"</span> <span class="attr">value</span>=<span class="string">"/Users/yunai/logs/${applicationName}.log"</span>/&gt;</span>​</span><br><span class="line">    <span class="comment">&lt;!-- 日志文件 Appender --&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">appender</span> <span class="attr">name</span>=<span class="string">"file"</span> <span class="attr">class</span>=<span class="string">"ch.qos.logback.core.rolling.RollingFileAppender"</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">file</span>&gt;</span>${LOG_FILE}<span class="tag">&lt;/<span class="name">file</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!--滚动策略，基于时间 + 大小的分包策略 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">rollingPolicy</span> <span class="attr">class</span>=<span class="string">"ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy"</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">fileNamePattern</span>&gt;</span>${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz<span class="tag">&lt;/<span class="name">fileNamePattern</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">maxHistory</span>&gt;</span>7<span class="tag">&lt;/<span class="name">maxHistory</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">maxFileSize</span>&gt;</span>10MB<span class="tag">&lt;/<span class="name">maxFileSize</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">rollingPolicy</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 日志的格式化 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">encoder</span> <span class="attr">class</span>=<span class="string">"ch.qos.logback.core.encoder.LayoutWrappingEncoder"</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">layout</span> <span class="attr">class</span>=<span class="string">"org.apache.skywalking.apm.toolkit.log.logback.v1.x.TraceIdPatternLogbackLayout"</span>&gt;</span></span><br><span class="line">                <span class="tag">&lt;<span class="name">Pattern</span>&gt;</span>${FILE_LOG_PATTERN}<span class="tag">&lt;/<span class="name">Pattern</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;/<span class="name">layout</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">encoder</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">appender</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="comment">&lt;!-- 设置 Appender --&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">root</span> <span class="attr">level</span>=<span class="string">"INFO"</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">appender-ref</span> <span class="attr">ref</span>=<span class="string">"console"</span>/&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">appender-ref</span> <span class="attr">ref</span>=<span class="string">"file"</span>/&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">root</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">configuration</span>&gt;</span></span><br></pre></td></tr></tbody></table>

日志配置有点长哈，**主要配置 2 处地方**，我们来看看图。如下图锁标记：![Logback 配置文件](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/101.png)

## 12.4 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-logback/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-logback/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="keyword">private</span> Logger logger = LoggerFactory.getLogger(getClass());</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/logback"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        logger.info(<span class="string">"测试日志"</span>);</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"logback"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/logback` 接口中，会执行一次日志的记录。

## 12.5 LogbackApplication

创建 [`LogbackApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-logback/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/LogbackApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">LogbackApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(LogbackApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 12.6 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/102.png)

然后，执行 `LogbackApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。启动日志如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment">// ... 省略其它日志</span></span><br><span class="line"></span><br><span class="line"><span class="number">2020</span>-<span class="number">01</span>-<span class="number">05</span> <span class="number">21</span>:<span class="number">19</span>:<span class="number">11.420</span>  INFO <span class="number">18611</span> TID:N/A --- [           main] c.i.s.l.s.LogbackApplication             : Started LogbackApplication in <span class="number">2.606</span> seconds (JVM running <span class="keyword">for</span> <span class="number">5.456</span>)</span><br></pre></td></tr></tbody></table>

- 因为此时没有 SkyWalking TraceId，所以 `%tid` 占位符被替换成了 `TID:N/A`。

## 12.7 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/logback](http://127.0.0.1:8079/demo/logback) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。看到日志如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="number">2020</span>-<span class="number">01</span>-<span class="number">05</span> <span class="number">21</span>:<span class="number">21</span>:<span class="number">51.521</span>  INFO <span class="number">18611</span> TID:<span class="number">22.37</span>.15782305114330001 --- [nio-<span class="number">8079</span>-exec-<span class="number">1</span>] c.i.s.l.s.controller.DemoController      : 测试日志</span><br></pre></td></tr></tbody></table>

- `%tid` 占位符被替换成了 SkyWalking TraceId `22.37.15782305114330001`。

2、然后，可以使用该 SkyWalking TraceId 在 SkyWalking UI 中，进行检索。如下图所示：![搜索链路](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/103.png)

具体实现原理，感兴趣的胖友，可以看看[《SkyWalking 源码分析 —— traceId 集成到日志组件》](http://www.iocoder.cn/SkyWalking/trace-id-integrate-into-logs/?self)文章。

# 13\. 自定义追踪方法

> 示例代码对应仓库：[lab-39-trace-annotations](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-trace-annotations/)。

在上述的示例中，我们都是通过 SkyWalking 提供的插件，实现对指定框架的链路追踪。那么，如果我们希望对项目中的业务方法，实现链路追踪，方便我们排查问题，需要怎么做呢？SkyWalking 提供了两种方式：

- 方式一，通过 SkyWalking `@Trace` 注解，可见[《Application-toolkit-trace.md》](https://github.com/SkyAPM/document-cn-translation-of-skywalking/blob/master/docs/zh/master/setup/service-agent/java-agent/Application-toolkit-trace.md)文档。
- 方式二，通过 SkyWalking XML `<enhanced />` 配置，可见[《Customize-enhance-trace.md》](https://github.com/SkyAPM/document-cn-translation-of-skywalking/blob/master/docs/zh/master/setup/service-agent/java-agent/Customize-enhance-trace.md)文档。

艿艿在项目中，使用 SkyWalking `@Trace` 注解较多，所以本小节我们就来看看它的使用示例。

## 13.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-trace-annotations/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">packaging</span>&gt;</span>jar<span class="tag">&lt;/<span class="name">packaging</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-trace-annotations<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- SkyWalking 工具类 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.skywalking<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>apm-toolkit-trace<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>6.6.0<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 13.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-trace-annotations/src/main/resources/application.yml) 中，添加配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br></pre></td></tr></tbody></table>

## 13.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-trace-annotations/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-trace-annotations/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/trace_annotations"</span>)</span><br><span class="line">    <span class="meta">@Trace</span>(operationName = <span class="string">"trace_annotations"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="comment">// &lt;X&gt; 自定义 SkyWalking Span</span></span><br><span class="line">        ActiveSpan.tag(<span class="string">"mp"</span>, <span class="string">"芋道源码"</span>);</span><br><span class="line"></span><br><span class="line">        <span class="comment">// 返回</span></span><br><span class="line">        <span class="keyword">return</span> <span class="string">"trace_annotations"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `#echo()` 方法上，添加了 SkyWalking [`@Trace`](https://github.com/apache/skywalking/blob/master/apm-application-toolkit/apm-toolkit-trace/src/main/java/org/apache/skywalking/apm/toolkit/trace/Trace.java) 注解，实现 SkyWalking 指定方法的追踪，会创建一个 SkyWalking LocalSpan。同时，可以通过 `operationName` 属性，设置操作名。
    
- 在 `<X>` 处，通过 `ActiveSpan#tag(String key, String value)` 方法，设置该 LocalSpan 的标签。ActiveSpan 还有其它方法，如下：
    
    > - `ActiveSpan.error()` 方法：将当前 Span 标记为出错状态.
    > - `ActiveSpan.error(String errorMsg)` 方法：将当前 Span 标记为出错状态, 并带上错误信息.
    > - `ActiveSpan.error(Throwable throwable)` 方法：将当前 Span 标记为出错状态, 并带上 Throwable。
    > - `ActiveSpan.debug(String debugMsg)` 方法：在当前 Span 添加一个 debug 级别的日志信息.
    > - `ActiveSpan.info(String infoMsg)` 方法：在当前 Span 添加一个 info 级别的日志信息.
    

另外，我们可以使用 [`TraceContext#traceId()`](https://github.com/apache/skywalking/blob/master/apm-application-toolkit/apm-toolkit-trace/src/main/java/org/apache/skywalking/apm/toolkit/trace/TraceContext.java) 方法，获得当前的 SkyWalking TraceId 链路追踪编号。

## 13.4 TraceAnnotationsApplication

创建 [`TraceAnnotationsApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-trace-annotations/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/TraceAnnotationsApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">TraceAnnotationsApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(TraceAnnotationsApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 13.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/111.png)

然后，执行 `TraceAnnotationsApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 13.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/trace\_annotations](http://127.0.0.1:8079/demo/trace_annotations) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，查看 SkyWalking UI 链路界面，可以看到我们自定义追踪的方法。示例如下图：![自定义链路追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/112.png)

点击红圈的 `@Trace` 注解的链路数据，可以看到具体信息。如下图所示：![SkyWalking UI 界面 —— 追踪（@Trace）](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/113.png)

具体实现原理，感兴趣的胖友，可以看看[《SkyWalking 源码分析 —— @Trace 注解想要追踪的任何方法》](http://www.iocoder.cn/SkyWalking/@trace-for-any-methods/?self)文章。

# 14\. OpenTracing 示例

> 示例代码对应仓库：[lab-39-opentracing](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-opentracing/)。

在开始本节之前，推荐胖友先阅读下[《OpenTracing 官方标准 —— 中文版》](https://github.com/opentracing-contrib/opentracing-specification-zh)规范，对 OpenTracing 有个简单的了解。

在 [opentracing-java](https://github.com/opentracing/opentracing-java) 项目中，定义了 OpenTracing Java API。而 SkyWalking [apm-toolkit-opentracing](https://github.com/apache/skywalking/tree/master/apm-application-toolkit/apm-toolkit-opentracing) 项目，提供了对该 OpenTracing Java API 的实现。因此，我们可以使用它，实现比[「13. 自定义追踪方法」](https://www.iocoder.cn/Spring-Boot/SkyWalking/#)小节，更加灵活的自定义追踪，同时可以做到和 SkyWalking **特性无关**，毕竟咱使用的是 OpenTracing Java API。

下面，我们来搭建一个 OpenTracing Java API 的使用示例。

## 14.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-opentracing/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-opentracing<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- SkyWalking Opentracing 集成 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.skywalking<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>apm-toolkit-opentracing<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>6.6.0<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 14.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-opentracing/src/main/resources/application.yml) 中，添加配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br></pre></td></tr></tbody></table>

## 14.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-opentracing/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-opentracing/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/opentracing"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        <span class="comment">// &lt;X&gt;创建一个 Span</span></span><br><span class="line">        Tracer tracer = <span class="keyword">new</span> SkywalkingTracer();</span><br><span class="line">        tracer.buildSpan(<span class="string">"custom_operation"</span>).withTag(<span class="string">"mp"</span>, <span class="string">"芋道源码"</span>).startManual().finish();</span><br><span class="line"></span><br><span class="line">        <span class="comment">// 返回</span></span><br><span class="line">        <span class="keyword">return</span> <span class="string">"opentracing"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `/demo/opentracing` 接口中的`<X>` 处，我们使用 Opentracing Java API 创建了一个 Span。
- 更多的 Opentracing Java API 的使用，可以看看 [opentracing-java](https://github.com/opentracing/opentracing-java) 项目提供的示例哈。

## 14.4 OpentracingApplication

创建 [`OpentracingApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-opentracing/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/OpentracingApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">OpentracingApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(OpentracingApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 14.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/121.png)

然后，执行 `OpentracingApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 14.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/opentracing](http://127.0.0.1:8079/demo/opentracing) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，查看 SkyWalking UI 链路界面，可以看到使用 Opentracing 创建的链路。示例如下图：![Opentracing 链路追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/122.png)

点击红圈的 Opentracing 创建的链路数据，可以看到具体信息。如下图所示：![SkyWalking UI 界面 —— Opentracing](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/123.png)

# 15\. Spring 异步任务示例

> 示例代码对应仓库：[lab-39-async](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-async/)。

本小节，我们来搭建一个 SkyWalking 对 Spring 异步任务的链路追踪。该链路通过如下插件实现收集：

- [`spring-plugins/async-annotation-plugin`](https://github.com/apache/skywalking/tree/master/apm-sniffer/apm-sdk-plugin/spring-plugins/async-annotation-plugin/)

下面，我们来搭建一个 Spring 异步任务的使用示例。。对 Spring 异步任务感兴趣的胖友，可以后续去看看[《芋道 Spring Boot 异步任务入门》](http://www.iocoder.cn/Spring-Boot/Async-Job/?self)文章。

## 15.1 引入依赖

在 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-async/pom.xml) 文件中，引入相关依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">packaging</span>&gt;</span>jar<span class="tag">&lt;/<span class="name">packaging</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-async<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 SpringMVC 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-web<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

## 15.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-async/src/main/resources/application.yml) 中，添加配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br></pre></td></tr></tbody></table>

## 15.3 DemoController

在 [`cn.iocoder.springboot.lab39.skywalkingdemo.controller`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-async/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/) 包路径下，创建 [DemoController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-async/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/controller/DemoController.java) 类，提供示例 API 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment">// DemoController.java</span></span><br><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/demo"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Autowired</span></span><br><span class="line">    <span class="keyword">private</span> DemoService demoService;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/async"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">echo</span><span class="params">()</span> </span>{</span><br><span class="line">        demoService.async();</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"async"</span>;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br><span class="line"></span><br><span class="line"><span class="comment">// DemoService.java</span></span><br><span class="line"><span class="meta">@Service</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">DemoService</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Async</span></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">void</span> <span class="title">async</span><span class="params">()</span> </span>{</span><br><span class="line">        System.out.println(<span class="string">"异步任务的执行"</span>);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 在 `DemoService#async()` 方法上，我们添加了 Spring `@Async` 注解，实现 Spring 异步任务。

## 15.4 AsyncApplication

创建 [`AsyncApplication.java`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-async/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/AsyncApplication.java) 类，配置 `@SpringBootApplication` 注解即可。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="meta">@EnableAsync</span>(proxyTargetClass = <span class="keyword">true</span>) <span class="comment">// 开启 @Async 的支持</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">AsyncApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(AsyncApplication.class, args);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

- 通过 `@EnableAsync` 注解，开启 Spring `@Async` 异步任务的支持。这里，我们设置了 `proxyTargetClass = true`，使用 CGLIB 实现动态代理，忘记当时踩过什么坑，好像是和 SkyWalking 发生了啥冲突，不太记得了。

## 15.5 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/131.png)

然后，执行 `OpentracingApplication#main(String[] args)` 方法，启动该 Spring Boot 应用。

## 15.6 简单测试

1、首先，使用浏览器，访问下 [http://127.0.0.1:8079/demo/async](http://127.0.0.1:8079/demo/async) 地址，请求下 Spring Boot 应用提供的 API。因为，我们要追踪下该链路。

2、然后，查看 SkyWalking UI 链路界面，可以看 Spring 异步任务的链路。示例如下图：![Spring 异步任务的链路追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/132.png)

另外，如果胖友不想使用 Spring 提供的异步任务的功能，想自己使用线程池实现异步，同时又想实现对该异步任务的链路追踪，可以参考[《SkyWalking —— Application-toolkit-trace-cross-thread.md》](https://github.com/SkyAPM/document-cn-translation-of-skywalking/blob/master/docs/zh/master/setup/service-agent/java-agent/Application-toolkit-trace-cross-thread.md)文档。

# 16\. Dubbo 示例

> 示例代码对应仓库：
> 
> - 服务 API 项目：[`lab-39-skywalking-dubbo-api`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-api/)
> - 服务 Provider 项目：[`skywalking-dubbo-provider`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-provider/)
> - 服务 Consumer 项目：[`skywalking-dubbo-consumer`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-consumer/)

本小节，我们来搭建一个 SkyWalking 对 **Dubbo** 的远程 RPC 调用的链路追踪。该链路通过如下**插件**实现收集：

- [`dubbo-2.7.x-conflict-patch`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/dubbo-2.7.x-conflict-patch/)
- [`dubbo-2.7.x-plugin`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/dubbo-2.7.x-plugin/)
- [`dubbo-conflict-patch`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/dubbo-conflict-patch/)
- [`dubbo-plugin`](https://github.com/apache/skywalking/blob/master/apm-sniffer/apm-sdk-plugin/dubbo-plugin/)

我们来新建一个 [`lab-39-skywalking-dubbo`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/) 模块，一共包含三个子项目。最终如下图所示：![项目结构](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/141.png)

另外，考虑到目前 Dubbo 主要使用 Zookeeper 作为注册中心，所以本小节也是使用 Zookeeper。不了解的胖友，后续可以看看[《Zookeeper 极简入门》](http://www.iocoder.cn/Zookeeper/install/?self)文章。

## 16.1 搭建 API 项目

创建 [`lab-39-skywalking-dubbo-api`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-api/) 项目，**服务接口**，定义 Dubbo Service API 接口，提供给消费者使用。

### 16.1.1 UserService

创建 [UserService](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-api/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/api/UserService.java) 接口，定义用户服务 RPC Service 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">interface</span> <span class="title">UserService</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="comment">/**</span></span><br><span class="line"><span class="comment">     * 根据指定用户编号，获得用户信息</span></span><br><span class="line"><span class="comment">     *</span></span><br><span class="line"><span class="comment">     * <span class="doctag">@param</span> id 用户编号</span></span><br><span class="line"><span class="comment">     * <span class="doctag">@return</span> 用户信息</span></span><br><span class="line"><span class="comment">     */</span></span><br><span class="line">    <span class="function">String <span class="title">get</span><span class="params">(Integer id)</span></span>;</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

## 16.2 搭建服务提供者

创建 [`skywalking-dubbo-provider`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-provider/) 项目，**服务提供者**，实现 `lab-39-skywalking-dubbo-api` 项目定义的 Dubbo Service API 接口，提供相应的服务。

### 16.2.1 引入依赖

创建 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-provider/pom.xml) 文件中，引入依赖。

<table><tbody><tr><td class="code"><pre><span class="line"><span class="php"><span class="meta">&lt;?</span>xml version=<span class="string">"1.0"</span> encoding=<span class="string">"UTF-8"</span><span class="meta">?&gt;</span></span></span><br><span class="line"><span class="tag">&lt;<span class="name">project</span> <span class="attr">xmlns</span>=<span class="string">"http://maven.apache.org/POM/4.0.0"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xmlns:xsi</span>=<span class="string">"http://www.w3.org/2001/XMLSchema-instance"</span></span></span><br><span class="line"><span class="tag">         <span class="attr">xsi:schemaLocation</span>=<span class="string">"http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">parent</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter-parent<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.2.2.RELEASE<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">relativePath</span>/&gt;</span> <span class="comment">&lt;!-- lookup parent from repository --&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">parent</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;<span class="name">modelVersion</span>&gt;</span>4.0.0<span class="tag">&lt;/<span class="name">modelVersion</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-skywalking-dubbo-provider<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line"></span><br><span class="line">    <span class="tag">&lt;<span class="name">dependencies</span>&gt;</span></span><br><span class="line">        <span class="comment">&lt;!-- 引入定义的 Dubbo API 接口 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>cn.iocoder.springboot.labs<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>lab-39-skywalking-dubbo-api<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>1.0-SNAPSHOT<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 引入 Spring Boot 依赖 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.springframework.boot<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>spring-boot-starter<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 实现对 Dubbo 的自动化配置 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.dubbo<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>dubbo<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.7.4.1<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.dubbo<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>dubbo-spring-boot-starter<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.7.4.1<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line"></span><br><span class="line">        <span class="comment">&lt;!-- 使用 Zookeeper 作为注册中心 --&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.curator<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>curator-framework<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.13.0<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;<span class="name">dependency</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">groupId</span>&gt;</span>org.apache.curator<span class="tag">&lt;/<span class="name">groupId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">artifactId</span>&gt;</span>curator-recipes<span class="tag">&lt;/<span class="name">artifactId</span>&gt;</span></span><br><span class="line">            <span class="tag">&lt;<span class="name">version</span>&gt;</span>2.13.0<span class="tag">&lt;/<span class="name">version</span>&gt;</span></span><br><span class="line">        <span class="tag">&lt;/<span class="name">dependency</span>&gt;</span></span><br><span class="line">    <span class="tag">&lt;/<span class="name">dependencies</span>&gt;</span></span><br><span class="line"></span><br><span class="line"><span class="tag">&lt;/<span class="name">project</span>&gt;</span></span><br></pre></td></tr></tbody></table>

- 具体每个依赖的作用，胖友自己认真看下艿艿添加的所有注释噢。

### 16.2.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-provider/src/main/resources/application.yaml) 中，添加 Dubbo 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">spring:</span></span><br><span class="line"><span class="attr">  application:</span></span><br><span class="line"><span class="attr">    name:</span> <span class="string">user-service-provider</span></span><br><span class="line"></span><br><span class="line"><span class="comment"># dubbo 配置项，对应 DubboConfigurationProperties 配置类</span></span><br><span class="line"><span class="attr">dubbo:</span></span><br><span class="line">  <span class="comment"># Dubbo 应用配置</span></span><br><span class="line"><span class="attr">  application:</span></span><br><span class="line"><span class="attr">    name:</span> <span class="string">${spring.application.name}</span> <span class="comment"># 应用名</span></span><br><span class="line">  <span class="comment"># Dubbo 注册中心配</span></span><br><span class="line"><span class="attr">  registry:</span></span><br><span class="line"><span class="attr">    address:</span> <span class="attr">zookeeper://127.0.0.1:2181</span> <span class="comment"># 注册中心地址。个鞥多注册中心，可见 http://dubbo.apache.org/zh-cn/docs/user/references/registry/introduction.html 文档。</span></span><br><span class="line">  <span class="comment"># Dubbo 服务提供者协议配置</span></span><br><span class="line"><span class="attr">  protocol:</span></span><br><span class="line"><span class="attr">    port:</span> <span class="bullet">-1</span> <span class="comment"># 协议端口。使用 -1 表示随机端口。</span></span><br><span class="line"><span class="attr">    name:</span> <span class="string">dubbo</span> <span class="comment"># 使用 `dubbo://` 协议。更多协议，可见 http://dubbo.apache.org/zh-cn/docs/user/references/protocol/introduction.html 文档</span></span><br><span class="line">  <span class="comment"># 配置扫描 Dubbo 自定义的 @Service 注解，暴露成 Dubbo 服务提供者</span></span><br><span class="line"><span class="attr">  scan:</span></span><br><span class="line"><span class="attr">    base-packages:</span> <span class="string">cn.iocoder.springboot.lab39.skywalkingdemo.providerdemo.service</span></span><br></pre></td></tr></tbody></table>

> 关于 `dubbo` 配置项，胖友可以后续阅读[《芋道 Spring Boot Dubbo 入门》](http://www.iocoder.cn/Spring-Boot/Dubbo/?self)文章。

### 16.2.3 UserServiceImpl

创建 [UserServiceImpl](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-provider/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/providerdemo/service/UserServiceImpl.java) 类，实现 UserService 接口，用户服务具体实现类。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@org</span>.apache.dubbo.config.annotation.Service(version = <span class="string">"1.0.0"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">UserServiceImpl</span> <span class="keyword">implements</span> <span class="title">UserService</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Override</span></span><br><span class="line">    <span class="function"><span class="keyword">public</span> String <span class="title">get</span><span class="params">(Integer id)</span> </span>{</span><br><span class="line">        <span class="keyword">return</span> <span class="string">"user:"</span> + id;</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

### 16.2.4 ProviderApplication

创建 [ProviderApplication](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-provider/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/providerdemo/ProviderApplication.java) 类，服务提供者的启动类。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">ProviderApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(ProviderApplication.class);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

### 16.2.5 IDEA 配置项

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/142.png)

## 16.3 搭建服务消费者

创建 [`skywalking-dubbo-consumer`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-consumer/) 项目，**服务消费者**，会调用 `lab-39-skywalking-dubbo-provider` 项目提供的 User Service 服务。

### 16.3.1 引入依赖

创建 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-consumer/pom.xml) 文件中，引入依赖。和[「16.2.1 引入依赖」](https://www.iocoder.cn/Spring-Boot/SkyWalking/#)基本是一致的，胖友可以点击 [`pom.xml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-consumer/pom.xml) 文件查看。

### 16.3.2 配置文件

在 [`application.yml`](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-consumer/src/main/resources/application.yaml) 中，添加 Dubbo 配置，如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="attr">server:</span></span><br><span class="line"><span class="attr">  port:</span> <span class="number">8079</span></span><br><span class="line"></span><br><span class="line"><span class="attr">spring:</span></span><br><span class="line"><span class="attr">  application:</span></span><br><span class="line"><span class="attr">    name:</span> <span class="string">user-service-consumer</span></span><br><span class="line"></span><br><span class="line"><span class="comment"># dubbo 配置项，对应 DubboConfigurationProperties 配置类</span></span><br><span class="line"><span class="attr">dubbo:</span></span><br><span class="line">  <span class="comment"># Dubbo 应用配置</span></span><br><span class="line"><span class="attr">  application:</span></span><br><span class="line"><span class="attr">    name:</span> <span class="string">${spring.application.name}</span> <span class="comment"># 应用名</span></span><br><span class="line">  <span class="comment"># Dubbo 注册中心配置</span></span><br><span class="line"><span class="attr">  registry:</span></span><br><span class="line"><span class="attr">    address:</span> <span class="attr">zookeeper://127.0.0.1:2181</span> <span class="comment"># 注册中心地址。个鞥多注册中心，可见 http://dubbo.apache.org/zh-cn/docs/user/references/registry/introduction.html 文档。</span></span><br></pre></td></tr></tbody></table>

> 关于 `dubbo` 配置项，胖友可以后续阅读[《芋道 Spring Boot Dubbo 入门》](http://www.iocoder.cn/Spring-Boot/Dubbo/?self)文章。

### 16.3.3 UserController

创建 [UserController](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-consumer/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/consumerdemo/controller/UserController.java) 类，提供调用 UserService 服务的 HTTP 接口。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@RestController</span></span><br><span class="line"><span class="meta">@RequestMapping</span>(<span class="string">"/user"</span>)</span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">UserController</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="meta">@Reference</span>(protocol = <span class="string">"dubbo"</span>, version = <span class="string">"1.0.0"</span>)</span><br><span class="line">    <span class="keyword">private</span> UserService userService;</span><br><span class="line"></span><br><span class="line">    <span class="meta">@GetMapping</span>(<span class="string">"/get"</span>)</span><br><span class="line">    <span class="function"><span class="keyword">public</span> String  <span class="title">get</span><span class="params">(@RequestParam(<span class="string">"id"</span>)</span> Integer id) </span>{</span><br><span class="line">        <span class="keyword">return</span> userService.get(id);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

### 16.3.4 ConsumerApplication

创建 [ConsumerApplication](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-skywalking-dubbo/lab-39-skywalking-dubbo-consumer/src/main/java/cn/iocoder/springboot/lab39/skywalkingdemo/consumerdemo/ConsumerApplication.java) 类，服务消费者的启动类。代码如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="meta">@SpringBootApplication</span></span><br><span class="line"><span class="keyword">public</span> <span class="class"><span class="keyword">class</span> <span class="title">ConsumerApplication</span> </span>{</span><br><span class="line"></span><br><span class="line">    <span class="function"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">void</span> <span class="title">main</span><span class="params">(String[] args)</span> </span>{</span><br><span class="line">        SpringApplication.run(ConsumerApplication.class);</span><br><span class="line">    }</span><br><span class="line"></span><br><span class="line">}</span><br></pre></td></tr></tbody></table>

### 16.3.5 IDEA 配置项

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/143.png)

## 16.4 简单测试

使用 ProviderApplication 启动服务提供者，使用 ConsumerApplication 启动服务消费者。

① 首先，使用浏览器，访问 [http://127.0.0.1:8079/user/get?id=1](http://127.0.0.1:8079/user/get?id=1) 地址，使用 Dubbo 调用 `user-service-provider` 服务。因为，我们要追踪下该链路。

② 然后，继续使用浏览器，打开 [http://127.0.0.1:8080/](http://127.0.0.1:8080/) 地址，进入 SkyWalking UI 界面。如下图所示：![SkyWalking UI 界面 —— 仪表盘](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/144.png)

③ 之后，点击「拓扑图」菜单，进入查看拓扑图的界面，可以看到两个服务的小方块，以及对应的调用关系。如下图所示：![SkyWalking UI 界面 —— 拓扑图](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/145.png)

④ 再之后，点击「追踪」菜单，进入查看链路数据的界面。如下图所示：![SkyWalking UI 界面 —— 追踪](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/146.png)

# 17\. 抽样收集示例

> 示例代码对应仓库：[lab-39-springmvc](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/) 。

在访问量**较少**时，链路**全量**收集不会对系统带来多少负担，同时能够**完整**的观测到系统的运行状况。但是在访问量**较大**时，**全量**的链路收集，对链路收集的客户端（应用）、服务端（例如说 SkyWalking OAP Collector）、存储器（例如说 Elastcsearch）都会带来**较大**的性能开销，甚至会影响应用的正常运行。

因此，在访问量级较大的情况下，我们往往会选择**抽样采样**，只选择收集**部分链路信息**。SkyWalking Agent 在 `agent/config/agent.config` 配置文件中，定义了 `agent.sample_n_per_3_secs` 配置项，设置每 3 秒可收集的链路数据的**数量**。

> Negative or zero means off, by default.SAMPLE\_N\_PER\_3\_SECS means sampling N TraceSegment in 3 seconds tops.

- 默认配置下，不进行限制，即获取全部链路数据。

> 友情提示：哈哈哈~如果访问量不是特别大，建议还是全量收集！

本小节，我们无需单独搭建项目，而是直接使用 [lab-39-springmvc](https://github.com/YunaiV/SpringBoot-Labs/blob/master/lab-39/lab-39-springmvc/) 项目即可。

## 17.1 SkyWalking Agent 配置文件

修改 SkyWalking Agent 的 `agent/config/agent.config` 配置文件，开放 `agent.sample_n_per_3_secs` 配置项，支持使用 `SW_AGENT_SAMPLE` 环境变量。命令行操作如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment"># 查看当前所在目录</span></span><br><span class="line">$ <span class="built_in">pwd</span></span><br><span class="line">/Users/yunai/skywalking/apache-skywalking-apm-bin-es7/agent</span><br><span class="line"></span><br><span class="line"><span class="comment"># 创建 apm-trace-ignore-plugin.config 配置文件，并进行设置</span></span><br><span class="line">$ vi config/apm-trace-ignore-plugin.config</span><br></pre></td></tr></tbody></table>

配置文件内容如下：

<table><tbody><tr><td class="code"><pre><span class="line"><span class="comment"># The number of sampled traces per 3 seconds</span></span><br><span class="line"><span class="comment"># Negative number means sample traces as many as possible, most likely 100%</span></span><br><span class="line"><span class="comment"># agent.sample_n_per_3_secs=${SW_AGENT_SAMPLE:-1}</span></span><br></pre></td></tr></tbody></table>

这样，每个项目可以通过 `SW_AGENT_SAMPLE` 环境变量，可以实现自定义抽样收集数量。

## 17.2 IDEA 配置

通过 IDEA 的「Run/Debug Configurations」配置使用 SkyWalking Agent。如下图所示：![Run/Debug Configurations](https://static.iocoder.cn/images/Spring-Boot/2020-04-01/151.png)

通过设置 `SW_AGENT_SAMPLE` 环境变量为 1，实现每 3 秒只收集一个链路数据，方便演示测试。

## 17.3 简单测试

① 首先，使用浏览器，**疯狂**访问下 [http://127.0.0.1:8079/demo/echo](http://127.0.0.1:8079/demo/echo) 地址，请求下 Spring Boot 应用提供的 API。

② 之后，在 SkyWalking UI 的查看链路追踪的界面，看到只有部分链路被收集。😈 这里暂时就不贴图了，嘿嘿~

# 666\. 彩蛋

发现给自己挖了一个深坑，竟然硬生生在一个周末，把主流框架的示例，一个一个搭建处理，并进行测试。😈 现在已经是周一的凌晨 00:22 ，我太难了~

有一点要注意，SkyWalking Agent 提供的插件，是基于我们使用的框架的方法，通过 AOP 的方式进行追踪，所以可能存在不兼容的情况，毕竟框架的代码在不断迭代，方法可能存在一定的变更。因此，一定要注意查看[《SkyWalking —— Supported-list.md》](https://github.com/SkyAPM/document-cn-translation-of-skywalking/blob/master/docs/zh/master/setup/service-agent/java-agent/Supported-list.md)文档。

当然，文档罗列的是 SkyWalking 开发团队已经测试过，经过确认支持的框架的版本。具体的，胖友最好动手测试下，自己再验证一波。如果碰到不支持的情况，可以查看对应框架的 SkyWalking 插件代码，比较容易就能定位到源码落。

嘻嘻，想要对 SkyWalking 做进一步深入的胖友，欢迎来看艿艿写的[《SkyWalking 源码解析》](http://www.iocoder.cn/categories/SkyWalking/?self)。美滋滋~

[Spring Boot](https://www.iocoder.cn/categories/Spring-Boot/)

[](https://www.iocoder.cn/Spring-Boot/SkyWalking/#)

扫描二维码分享到微信朋友圈[](https://www.iocoder.cn/Spring-Boot/SkyWalking/#share)**Loading...Please wait**

[](https://www.iocoder.cn/Spring-Boot/SkyWalking/#textlogo "Top")[](https://www.facebook.com/sharer.php?u=http%3A%2F%2Fwww.iocoder.cn%2FSpring-Boot%2FSkyWalking%2F "Facebook")[](https://www.iocoder.cn/Spring-Boot/SkyWalking/#qrcode "QRcode")[](https://twitter.com/intent/tweet?url=http%3A%2F%2Fwww.iocoder.cn%2FSpring-Boot%2FSkyWalking%2F "Twitter")[](http://service.weibo.com/share/share.php?title=%E8%8A%8B%E9%81%93%20Spring%20Boot%20%E9%93%BE%E8%B7%AF%E8%BF%BD%E8%B8%AA%20SkyWalking%20%E5%85%A5%E9%97%A8%20|%20%E8%8A%8B%E9%81%93%E6%BA%90%E7%A0%81%20%E2%80%94%E2%80%94%20%E7%BA%AF%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90%E5%8D%9A%E5%AE%A2&url=http%3A%2F%2Fwww.iocoder.cn%2FSpring-Boot%2FSkyWalking%2F&ralateUid=null&searchPic=true&style=number "Weibo")

[**PREVIOUS:**  
大厂面试！我和面试官之间关于Redis的一场对弈！](https://www.iocoder.cn/Fight/Big-factory-interview-A-chess-game-between-me-and-the-interviewer-about-Redis/ "大厂面试！我和面试官之间关于Redis的一场对弈！")

[**NEXT:**  
如何让 Mybatis 自动生成代码](https://www.iocoder.cn/Fight/How-do-I-get-Mybatis-to-generate-code-automatically/ "如何让 Mybatis 自动生成代码")

#locker { position: absolute; top: 1300px; left: 0; bottom: 0px; width: 150%; display: none; z-index: 10; } #locker .mask { height: 240px; width: 100%; background: -webkit-gradient(linear, 0 top, 0 bottom, from(rgba(255, 255, 255, 0)), to(#fff)); } #locker .info { background: white; height: 500px; padding-bottom: 50px; margin: 0px; } .lock { position: relative; overflow: hidden; padding-bottom: 30px; } .text-center { text-align: center; } .btn-outline { color: #E9405A !important; background-color: transparent; border-color: #E9405A; padding: 5px 16px; border-radius: 5px; } function getCookie(cookie\_name) { var allcookies = document.cookie; var cookie\_pos = allcookies.indexOf(cookie\_name); if (cookie\_pos != -1) { cookie\_pos = cookie\_pos + cookie\_name.length + 1; var cookie\_end = allcookies.indexOf(";", cookie\_pos); if (cookie\_end == -1) { cookie\_end = allcookies.length; } var value = unescape(allcookies.substring(cookie\_pos, cookie\_end)); } return value; }

**文章目录**

1. [1. 1\. 概述](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
2. [2. 2\. SpringMVC 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [2.1. 2.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [2.2. 2.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [2.3. 2.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [2.4. 2.4 Application](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [2.5. 2.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [2.6. 2.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
3. [3. 3\. 忽略部分 URL 的追踪](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [3.1. 3.1 复制插件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [3.2. 3.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [3.3. 3.3 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [3.4. 3.4 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
4. [4. 4\. MySQL 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [4.1. 4.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [4.2. 4.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [4.3. 4.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [4.4. 4.4 MySQLApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [4.5. 4.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [4.6. 4.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
5. [5. 5\. Redis 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [5.1. 5.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [5.2. 5.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [5.3. 5.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [5.4. 5.4 RedisApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [5.5. 5.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [5.6. 5.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
6. [6. 6\. MongoDB 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [6.1. 6.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [6.2. 6.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [6.3. 6.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [6.4. 6.4 MongoDBApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [6.5. 6.5 插件配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [6.6. 6.6 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    7. [6.7. 6.7 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
7. [7. 7\. Elasticsearch 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [7.1. 7.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [7.2. 7.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [7.3. 7.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [7.4. 7.4 ElasticsearchJestApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [7.5. 7.5 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
8. [8. 8\. RocketMQ 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [8.1. 8.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [8.2. 8.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [8.3. 8.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [8.4. 8.4 RocketMQApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [8.5. 8.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [8.6. 8.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    7. [8.7. 8.7 阿里云的消息队列 ONS 服务](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
9. [9. 9\. Kafka 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [9.1. 9.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [9.2. 9.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [9.3. 9.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [9.4. 9.4 KafkaApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [9.5. 9.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [9.6. 9.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
10. [10. 10\. RabbitMQ 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [10.1. 10.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [10.2. 10.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [10.3. 10.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [10.4. 10.4 RabbitMQApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [10.5. 10.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [10.6. 10.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
11. [11. 11\. ActiveMQ 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [11.1. 11.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [11.2. 11.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [11.3. 11.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [11.4. 11.4 ActiveMQApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [11.5. 11.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [11.6. 11.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
12. [12. 12\. 日志框架示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [12.1. 12.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [12.2. 12.2 应用配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [12.3. 12.3 Logback 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [12.4. 12.4 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [12.5. 12.5 LogbackApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [12.6. 12.6 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    7. [12.7. 12.7 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
13. [13. 13\. 自定义追踪方法](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [13.1. 13.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [13.2. 13.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [13.3. 13.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [13.4. 13.4 TraceAnnotationsApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [13.5. 13.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [13.6. 13.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
14. [14. 14\. OpenTracing 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [14.1. 14.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [14.2. 14.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [14.3. 14.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [14.4. 14.4 OpentracingApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [14.5. 14.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [14.6. 14.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
15. [15. 15\. Spring 异步任务示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [15.1. 15.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [15.2. 15.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [15.3. 15.3 DemoController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [15.4. 15.4 AsyncApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    5. [15.5. 15.5 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    6. [15.6. 15.6 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
16. [16. 16\. Dubbo 示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [16.1. 16.1 搭建 API 项目](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        1. [16.1.1. 16.1.1 UserService](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [16.2. 16.2 搭建服务提供者](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        1. [16.2.1. 16.2.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        2. [16.2.2. 16.2.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        3. [16.2.3. 16.2.3 UserServiceImpl](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        4. [16.2.4. 16.2.4 ProviderApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        5. [16.2.5. 16.2.5 IDEA 配置项](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [16.3. 16.3 搭建服务消费者](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        1. [16.3.1. 16.3.1 引入依赖](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        2. [16.3.2. 16.3.2 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        3. [16.3.3. 16.3.3 UserController](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        4. [16.3.4. 16.3.4 ConsumerApplication](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
        5. [16.3.5. 16.3.5 IDEA 配置项](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    4. [16.4. 16.4 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
17. [17. 17\. 抽样收集示例](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    1. [17.1. 17.1 SkyWalking Agent 配置文件](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    2. [17.2. 17.2 IDEA 配置](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
    3. [17.3. 17.3 简单测试](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)
18. [18. 666\. 彩蛋](https://www.iocoder.cn/Spring-Boot/SkyWalking/#null)

博主【芋艿】在看的课程

- [【老牛逼了】Dubbo 源码解析系列](https://www.iocoder.cn/Dubbo/good-collection/?side)
- [Netty 源码解析系列](https://www.iocoder.cn/Netty/Netty-collection/?side)
- [Spring 源码解析系列](https://www.iocoder.cn/Spring/good-collection/?side)
- [Spring MVC 源码解析系列](https://www.iocoder.cn/Spring-MVC/good-collection/?side)
- [Spring Boot 源码解析系列](https://www.iocoder.cn/Spring-Boot/good-collection/?side)
- [MyBatis 源码解析系列](https://www.iocoder.cn/MyBatis/good-collection/?side)
- [数据库实体设计合集](https://www.iocoder.cn/Entity/good-collection/?side)
- [【老牛逼了】Java 面试题](https://www.iocoder.cn/Interview/good-collection/?side)
- [Spring Boot 学习路线](http://blog.didispace.com/Spring-Boot%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B/?utm_term=yudaoyuanma)
- [Spring Cloud 学习路线](http://blog.didispace.com/spring-cloud-learning/?utm_term=yudaoyuanma)

![](/images/common/wechat_mp_2017_07_31_bak.jpg)

微信公众号福利：芋道源码

- [0\. 阅读源码葵花宝典](https://www.iocoder.cn/images/common/wechat_mp_2017_07_31_bak.jpg)
- [1\. RocketMQ / MyCAT / Sharding-JDBC 详细中文注释源码](https://www.iocoder.cn/images/common/wechat_mp_2017_07_31_bak.jpg)
- [2\. 您对于源码的疑问每条留言都将得到认真回复](https://www.iocoder.cn/images/common/wechat_mp_2017_07_31_bak.jpg)
- [3\. 新的源码解析文章实时收到通知，每周六十点更新](https://www.iocoder.cn/images/common/wechat_mp_2017_07_31_bak.jpg)
- [4\. 认真的源码交流微信群](https://www.iocoder.cn/images/common/wechat_mp_2017_07_31_bak.jpg)

分类

- [APISIX<sup>1</sup>](https://www.iocoder.cn/categories/APISIX/ "APISIX")
- [ActiveMQ<sup>2</sup>](https://www.iocoder.cn/categories/ActiveMQ/ "ActiveMQ")
- [Apollo<sup>35</sup>](https://www.iocoder.cn/categories/Apollo/ "Apollo")
- [CAT<sup>1</sup>](https://www.iocoder.cn/categories/CAT/ "CAT")
- [Canal<sup>7</sup>](https://www.iocoder.cn/categories/Canal/ "Canal")
- [Elastic-Job-Cloud<sup>6</sup>](https://www.iocoder.cn/categories/Elastic-Job-Cloud/ "Elastic-Job-Cloud")
- [Elastic-Job-Lite<sup>16</sup>](https://www.iocoder.cn/categories/Elastic-Job-Lite/ "Elastic-Job-Lite")
- [Elasticsearch<sup>3</sup>](https://www.iocoder.cn/categories/Elasticsearch/ "Elasticsearch")
- [Eureka<sup>24</sup>](https://www.iocoder.cn/categories/Eureka/ "Eureka")
- [Fescar<sup>5</sup>](https://www.iocoder.cn/categories/Fescar/ "Fescar")
- [Guava<sup>13</sup>](https://www.iocoder.cn/categories/Guava/ "Guava")
- [HikariCP<sup>21</sup>](https://www.iocoder.cn/categories/HikariCP/ "HikariCP")
- [Hmily<sup>1</sup>](https://www.iocoder.cn/categories/Hmily/ "Hmily")
- [Hystrix<sup>12</sup>](https://www.iocoder.cn/categories/Hystrix/ "Hystrix")
- [IDEA<sup>4</sup>](https://www.iocoder.cn/categories/IDEA/ "IDEA")
- [JDK 源码<sup>5</sup>](https://www.iocoder.cn/categories/JDK-%E6%BA%90%E7%A0%81/ "JDK 源码")
- [JUC<sup>36</sup>](https://www.iocoder.cn/categories/JUC/ "JUC")
- [JVM<sup>1</sup>](https://www.iocoder.cn/categories/JVM/ "JVM")
- [Java 面试<sup>12</sup>](https://www.iocoder.cn/categories/Java-%E9%9D%A2%E8%AF%95/ "Java 面试")
- [Jenkins<sup>1</sup>](https://www.iocoder.cn/categories/Jenkins/ "Jenkins")
- [Jetty<sup>6</sup>](https://www.iocoder.cn/categories/Jetty/ "Jetty")
- [Kafka<sup>27</sup>](https://www.iocoder.cn/categories/Kafka/ "Kafka")
- [Kong<sup>1</sup>](https://www.iocoder.cn/categories/Kong/ "Kong")
- [Maven<sup>1</sup>](https://www.iocoder.cn/categories/Maven/ "Maven")
- [MongoDB<sup>3</sup>](https://www.iocoder.cn/categories/MongoDB/ "MongoDB")
- [MyBatis<sup>7</sup>](https://www.iocoder.cn/categories/MyBatis/ "MyBatis")
- [MyCAT<sup>9</sup>](https://www.iocoder.cn/categories/MyCAT/ "MyCAT")
- [Nacos<sup>13</sup>](https://www.iocoder.cn/categories/Nacos/ "Nacos")
- [Netty<sup>11</sup>](https://www.iocoder.cn/categories/Netty/ "Netty")
- [Nginx<sup>3</sup>](https://www.iocoder.cn/categories/Nginx/ "Nginx")
- [NodeJS<sup>2</sup>](https://www.iocoder.cn/categories/NodeJS/ "NodeJS")
- [Onemall<sup>1</sup>](https://www.iocoder.cn/categories/Onemall/ "Onemall")
- [Prometheus<sup>1</sup>](https://www.iocoder.cn/categories/Prometheus/ "Prometheus")
- [RabbitMQ<sup>2</sup>](https://www.iocoder.cn/categories/RabbitMQ/ "RabbitMQ")
- [Redis<sup>3</sup>](https://www.iocoder.cn/categories/Redis/ "Redis")
- [Resilience4j<sup>12</sup>](https://www.iocoder.cn/categories/Resilience4j/ "Resilience4j")
- [Ribbon<sup>9</sup>](https://www.iocoder.cn/categories/Ribbon/ "Ribbon")
- [RocketMQ<sup>29</sup>](https://www.iocoder.cn/categories/RocketMQ/ "RocketMQ")
- [RxJava<sup>7</sup>](https://www.iocoder.cn/categories/RxJava/ "RxJava")
- [SOFA Mosn<sup>1</sup>](https://www.iocoder.cn/categories/SOFA-Mosn/ "SOFA Mosn")
- [SOFA RPC<sup>4</sup>](https://www.iocoder.cn/categories/SOFA-RPC/ "SOFA RPC")
- [Seata<sup>6</sup>](https://www.iocoder.cn/categories/Seata/ "Seata")
- [Sentinel<sup>17</sup>](https://www.iocoder.cn/categories/Sentinel/ "Sentinel")
- [Sentry<sup>1</sup>](https://www.iocoder.cn/categories/Sentry/ "Sentry")
- [Sharding Sphere<sup>6</sup>](https://www.iocoder.cn/categories/Sharding-Sphere/ "Sharding Sphere")
- [Sharding-JDBC<sup>19</sup>](https://www.iocoder.cn/categories/Sharding-JDBC/ "Sharding-JDBC")
- [Shiro<sup>8</sup>](https://www.iocoder.cn/categories/Shiro/ "Shiro")
- [SkyWalking<sup>42</sup>](https://www.iocoder.cn/categories/SkyWalking/ "SkyWalking")
- [Solr<sup>1</sup>](https://www.iocoder.cn/categories/Solr/ "Solr")
- [Soul<sup>1</sup>](https://www.iocoder.cn/categories/Soul/ "Soul")
- [Spring<sup>24</sup>](https://www.iocoder.cn/categories/Spring/ "Spring")
- [Spring Boot<sup>99</sup>](https://www.iocoder.cn/categories/Spring-Boot/ "Spring Boot")
- [Spring Cloud<sup>33</sup>](https://www.iocoder.cn/categories/Spring-Cloud/ "Spring Cloud")
- [Spring Security<sup>34</sup>](https://www.iocoder.cn/categories/Spring-Security/ "Spring Security")
- [Spring Session<sup>1</sup>](https://www.iocoder.cn/categories/Spring-Session/ "Spring Session")
- [Spring Webflux<sup>8</sup>](https://www.iocoder.cn/categories/Spring-Webflux/ "Spring Webflux")
- [Spring-Cloud-Config<sup>1</sup>](https://www.iocoder.cn/categories/Spring-Cloud-Config/ "Spring-Cloud-Config")
- [Spring-Cloud-Gateway<sup>26</sup>](https://www.iocoder.cn/categories/Spring-Cloud-Gateway/ "Spring-Cloud-Gateway")
- [Spring-MVC<sup>13</sup>](https://www.iocoder.cn/categories/Spring-MVC/ "Spring-MVC")
- [TCC-Transaction<sup>7</sup>](https://www.iocoder.cn/categories/TCC-Transaction/ "TCC-Transaction")
- [Tomcat<sup>18</sup>](https://www.iocoder.cn/categories/Tomcat/ "Tomcat")
- [XXL-JOB<sup>1</sup>](https://www.iocoder.cn/categories/XXL-JOB/ "XXL-JOB")
- [Yudao<sup>2</sup>](https://www.iocoder.cn/categories/Yudao/ "Yudao")
- [Zipkin<sup>11</sup>](https://www.iocoder.cn/categories/Zipkin/ "Zipkin")
- [Zookeeper<sup>2</sup>](https://www.iocoder.cn/categories/Zookeeper/ "Zookeeper")
- [Zuul<sup>8</sup>](https://www.iocoder.cn/categories/Zuul/ "Zuul")
- [书单<sup>33</sup>](https://www.iocoder.cn/categories/%E4%B9%A6%E5%8D%95/ "书单")
- [工作内推<sup>4</sup>](https://www.iocoder.cn/categories/%E5%B7%A5%E4%BD%9C%E5%86%85%E6%8E%A8/ "工作内推")
- [性能测试<sup>9</sup>](https://www.iocoder.cn/categories/%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/ "性能测试")
- [数据结构与算法<sup>4</sup>](https://www.iocoder.cn/categories/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/ "数据结构与算法")
- [电商开源项目<sup>2</sup>](https://www.iocoder.cn/categories/%E7%94%B5%E5%95%86%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE/ "电商开源项目")
- [精进<sup>4791</sup>](https://www.iocoder.cn/categories/%E7%B2%BE%E8%BF%9B/ "精进")
- [芋道源码的周八<sup>20</sup>](https://www.iocoder.cn/categories/%E8%8A%8B%E9%81%93%E6%BA%90%E7%A0%81%E7%9A%84%E5%91%A8%E5%85%AB/ "芋道源码的周八")
- [设计模式<sup>25</sup>](https://www.iocoder.cn/categories/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/ "设计模式")
- [鸡汤<sup>7</sup>](https://www.iocoder.cn/categories/%E9%B8%A1%E6%B1%A4/ "鸡汤")

![](/images/common/zsxq/02.png)

![](http://www.iocoder.cn/images/common/wechat_mp_simple.png)

© 2023 [芋道源码](http://www.iocoder.cn/ "芋道源码") && 总访客数 7200008 次 && 总访问量 16202855 次 && Hosted by [Coding Pages](https://pages.coding.me/)

沪ICP备17037075号-1
