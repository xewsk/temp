# 框架说明
## 概要信息
- 当前框架基于 [ruoyi-vue-pro](https://gitee.com/zhijiantianya/ruoyi-vue-pro)

## 为什么选择芋道
- 基本满足我们对规范性、主要技术选型、基本功能的期望。基于它进行我们框架的开发，能相对较好地平衡复杂度、工作量、进度要求。
- 模块划分比较合理，而且框架基础模块可以实现未来可在微服务框架中复用。若按规范的方式进行业务模块开发，业务模块也可较低成本实现单体/微服务切换。若遵循模块设计规范，业务模块可实现良好的分层、解耦，业务代码可基于模块在不同项目间复用。
- 我们选择了它不代表公司框架就是它，我们要基于公司内的整体项目需求不断完善优化，基于它产生公司自己的框架。


## 框架后续规划
- 因为前端技术选型不一致，要开发出对应的前端。在此过程可对框架中的功能按需求调整，要注意规范性，这些调整是对框架的调整。
- 梳理芋道中的技术选型及相关实现，按我们的需要进行适当调整。
- 基于芋道定义出我们自己的框架相关的设计实现/开发相关规范。
- 结合应用需求的优先级进行框架开发。

### 具体思路记录
- flyway
- ...

## 分支说明
- master 主分支
- dev 框架开发分支

> 补充说明：目前因为框架完善和代码生成同步来做，要注意两者之间的界限。代码生成对框架来说只是一个使用场景，相当于一个使用框架的业务系统。

## 模块说明
### 模块

| 项目                                                                       | 说明                 |
|--------------------------------------------------------------------------|--------------------|
| `rongdan-dependencies`                                                     | Maven 依赖版本管理       |
| `rongdan-framework`                                                        | Java 框架拓展          |
| `rongdan-server`                                                           | 管理后台 + 用户 APP 的服务端 |
| `rongdan-module-system`                                                    | 系统功能的 Module 模块    |
| `rongdan-module-infra`                                                     | 基础设施的 Module 模块    |
| `rongdan-module-bpm`                                                       | 工作流程的 Module 模块    |

**重要说明**
- 框架基于maven进行了科学的模块拆分，这是非常有必要的
- 框架交付时，业务开发拿到的并不是当前项目的完整代码，他们只需要一个包含rongdan-server及示例业务模块的业务初始代码项目。其他作为框架依赖引入业务初始代码中。
- 框架代码和业务代码之间有明确的界限，一般不允许越界开发。若情况特殊可一起沟通确定后特事特办。

#### rongdan-dependencies
- 定义了我们推荐使用的组件及版本
- 随框架不断升级演进，就像SpringBoot一样
- 业务开发中要按我们指定的组件使用，不允许随意使用同类型组件替代，若有需求可反馈我们一起探讨确定

#### rongdan-framework
- 业务无关，按作用定义了一个个starter。
- 其内部一般包含对基础组件的引入、我们定义的标准的封装、功能实现等内容。
- 业务系统按需引用使用。

#### rongdan-server
- 服务端模块。通常包含启动类、配置等。
- **若特殊业务系统不打算拆分业务模块，将所有业务代码都写到此模块也是可以的，但是一定要慎之又慎，这是不规范的。**

#### rongdan-module-infra
- 框架基础设施模块
- 由框架开发团队进行维护，源码一般不会交付给业务开发，这是为了规范性。允许业务开发同事获取、学习源码，鼓励反馈，但不允许在业务项目中对此模块改造。若特殊项目需求要定制改造，需讨论确定。
- 可以理解为这是一个业务系统的模块，只是所有业务系统都需要，我们拿到了框架中统一实现。所以它的实现是业务模块开发规范的实现示例。

#### rongdan-module-system	
- 系统功能模块，包含用户权限等
- 由框架开发团队进行维护，源码一般不会交付给业务开发，这是为了规范性。允许业务开发同事获取、学习源码，鼓励反馈，但不允许在业务项目中对此模块改造。若特殊项目需求要定制改造，需讨论确定。
- 可以理解为这是一个业务系统的模块，只是所有业务系统都需要，我们拿到了框架中统一实现。所以它的实现是业务模块开发规范的实现示例。

#### rongdan-module-bpm	
- 工作流相关业务层的封装实现
- 由框架开发团队进行维护，源码一般不会交付给业务开发，这是为了规范性。允许业务开发同事获取、学习源码，鼓励反馈，但不允许在业务项目中对此模块改造。若特殊项目需求要定制改造，需讨论确定。
- 可以理解为这是一个业务系统的模块，只是很多业务系统都需要，我们拿到了框架中统一实现。所以它的实现是一个基础业务模块开发规范的实现示例。

####业务模块
- 推荐的模块结构：外层是一个空的maven项目，内部包含两个子项目：api和biz。
  - api子项目：内容是当前模块的外部接口定义，无任何实现，用于被其他模块（如上层业务模块）调用。
  - biz子项目是实现模块，包含了当前业务模块对应的完整业务实现逻辑。
- 模块命名：【项目名】-module-【模块名】。api子模块：【项目名】-module-【模块名】-api。实现模块：【项目名】-module-【模块名】-biz。
- 若模块位于业务分层顶层，可不需要api模块。
- 将来biz模块可直接放到微服务框架中使用，api模块在微服务框架和单体框架通常是不同的。
- 若不需要api模块，可暂时不创建api子项目。
- 编码规范参照示例模块。

# 规范说明
## 版本规范
使用语义化版本。
> 概述：三段式版本号，如1.2.3，依次为主版本号、次版本号、修订号。进行了重大升级，不保证兼容性时，升级主版本号；进行了新功能的开发升级次版本号；进行了功能完善优化、bug修改，升级修订号。

## git工作流规范
框架使用gitflow，业务代码可由业务部门自行决定。

## 编码规范
- 阿里规约：《Java开发手册(黄山版).pdf》
- 使用阿里的静态检查工具及格式化模板，未来考虑引入sonar

## 工作流程
- 对框架的变更要谨慎，明确需求、实现方式，要内部讨论通过后再执行。

# 芋道boot版相关资料
## 🐶 新手必读

* 演示地址：<http://dashboard.rongdan.iocoder.cn>
* 启动文档：<https://doc.iocoder.cn/quick-start/>
* 视频教程：<https://doc.iocoder.cn/video/>

## 🐯 平台简介

**芋道**，以开发者为中心，打造中国第一流的快速开发平台，全部开源，个人与企业可 100% 免费使用。

> 有任何问题，或者想要的功能，可以在 _Issues_ 中提给艿艿。
>
> 😜 给项目点点 Star 吧，这对我们真的很重要！

![架构图](https://static.iocoder.cn/ruoyi-vue-pro-architecture.png?imageView2/2/format/webp)

* 管理后台的 Vue3 版本采用 [vue-element-plus-admin](https://gitee.com/kailong110120130/vue-element-plus-admin) ，Vue2 版本采用 [vue-element-admin](https://github.com/PanJiaChen/vue-element-admin) 
* 管理后台的移动端采用 [uni-app](https://github.com/dcloudio/uni-app) 方案，一份代码多终端适配，同时支持 APP、小程序、H5！
* 后端采用 Spring Boot 多模块架构、MySQL + MyBatis Plus、Redis + Redisson
* 数据库可使用 MySQL、Oracle、PostgreSQL、SQL Server、MariaDB、国产达梦 DM、TiDB 等
* 权限认证使用 Spring Security & Token & Redis，支持多终端、多种用户的认证系统，支持 SSO 单点登录
* 支持加载动态权限菜单，按钮级别权限控制，本地缓存提升性能
* 支持 SaaS 多租户，可自定义每个租户的权限，提供透明化的多租户底层封装
* 工作流使用 Flowable，支持动态表单、在线设计流程、会签 / 或签、多种任务分配方式
* 高效率开发，使用代码生成器可以一键生成前后端代码 + 单元测试 + Swagger 接口文档 + Validator 参数校验
* 集成微信小程序、微信公众号、企业微信、钉钉等三方登陆，集成支付宝、微信等支付与退款
* 集成阿里云、腾讯云等短信渠道，集成 MinIO、阿里云、腾讯云、七牛云等云存储服务
* 集成报表设计器、大屏设计器，通过拖拽即可生成酷炫的报表与大屏

##  🐳 项目关系

![架构演进](https://static.iocoder.cn/rongdan-roadmap.png?imageView2/2/format/webp)

三个项目的功能对比，可见社区共同整理的 [国产开源项目对比](https://www.yuque.com/xiatian-bsgny/lm0ec1/wqf8mn) 表格。

### 后端项目


| 项目                                                              | Star                                                                                                                                                                                                                                                                                             | 简介                          |
|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------|
| [ruoyi-vue-pro](https://gitee.com/zhijiantianya/ruoyi-vue-pro)  | [![Gitee star](https://gitee.com/zhijiantianya/ruoyi-vue-pro/badge/star.svg?theme=white)](https://gitee.com/zhijiantianya/ruoyi-vue-pro) [![GitHub stars](https://img.shields.io/github/stars/YunaiV/ruoyi-vue-pro.svg?style=social&label=Stars)](https://github.com/YunaiV/ruoyi-vue-pro)       | 基于 Spring Boot 多模块架构        |
| [rongdan-cloud](https://gitee.com/zhijiantianya/rongdan-cloud)      | [![Gitee star](https://gitee.com/zhijiantianya/rongdan-cloud/badge/star.svg?theme=white)](https://gitee.com/zhijiantianya/rongdan-cloud) [![GitHub stars](https://img.shields.io/github/stars/YunaiV/rongdan-cloud.svg?style=social&label=Stars)](https://github.com/YunaiV/rongdan-cloud)               | 基于 Spring Cloud 微服务架构       |
| [Spring-Boot-Labs](https://gitee.com/yudaocode/SpringBoot-Labs) | [![Gitee star](https://gitee.com/yudaocode/SpringBoot-Labs/badge/star.svg?theme=white)](https://gitee.com/zhijiantianya/rongdan-cloud) [![GitHub stars](https://img.shields.io/github/stars/yudaocode/SpringBoot-Labs.svg?style=social&label=Stars)](https://github.com/yudaocode/SpringBoot-Labs) | 系统学习 Spring Boot & Cloud 专栏 |

### 前端项目

| 项目                                                                                                       | Star                                                                                                                                                                                                                                                                                                                                                           | 简介                              |
|----------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| [rongdan-ui-admin-vue3](https://gitee.com/yudaocode/rongdan-ui-admin-vue3)                                   | [![Gitee star](https://gitee.com/yudaocode/rongdan-ui-admin-vue3/badge/star.svg?theme=white)](https://gitee.com/yudaocode/rongdan-ui-admin-vue3) [![GitHub stars](https://img.shields.io/github/stars/yudaocode/rongdan-ui-admin-vue3.svg?style=social&label=Stars)](https://github.com/yudaocode/rongdan-ui-admin-vue3)                                               | 基于 Vue3 + element-plus 实现的管理后台  |
| [rongdan-ui-admin](https://gitee.com/zhijiantianya/ruoyi-vue-pro/tree/master/rongdan-ui-admin)               | [![Gitee star](https://gitee.com/zhijiantianya/ruoyi-vue-pro/badge/star.svg?theme=white)](https://gitee.com/zhijiantianya/ruoyi-vue-pro/tree/master/rongdan-ui-admin) [![GitHub stars](https://img.shields.io/github/stars/YunaiV/ruoyi-vue-pro.svg?style=social&label=Stars)](https://github.com/YunaiV/ruoyi-vue-pro/tree/master/rongdan-ui-admin)               | 基于 Vue2 + element-ui 实现的管理后台    |
| [rongdan-ui-admin-uniapp](https://gitee.com/zhijiantianya/ruoyi-vue-pro/tree/master/rongdan-ui-admin-uniapp) | [![Gitee star](https://gitee.com/zhijiantianya/ruoyi-vue-pro/badge/star.svg?theme=white)](https://gitee.com/zhijiantianya/ruoyi-vue-pro/tree/master/rongdan-ui-admin-uniapp) [![GitHub stars](https://img.shields.io/github/stars/YunaiV/ruoyi-vue-pro.svg?style=social&label=Stars)](https://github.com/YunaiV/ruoyi-vue-pro/tree/master/rongdan-ui-admin-uniapp) | 基于 uni-app + uni-ui 实现的管理后台的小程序 |
| [rongdan-ui-go-view](https://gitee.com/yudaocode/rongdan-ui-go-view)                                         | [![Gitee star](https://gitee.com/yudaocode/rongdan-ui-go-view/badge/star.svg?theme=white)](https://gitee.com/yudaocode/rongdan-ui-go-view) [![GitHub stars](https://img.shields.io/github/stars/yudaocode/rongdan-ui-go-view.svg?style=social&label=Stars)](https://github.com/yudaocode/rongdan-ui-go-view)                                                           | 基于 Vue3 + naive-ui 实现的大屏报表      |
| [rongdan-ui-app](https://gitee.com/zhijiantianya/ruoyi-vue-pro/tree/master/rongdan-ui-app)                   | [![Gitee star](https://gitee.com/zhijiantianya/ruoyi-vue-pro/badge/star.svg?theme=white)](https://gitee.com/zhijiantianya/ruoyi-vue-pro/tree/master/rongdan-ui-app) [![GitHub stars](https://img.shields.io/github/stars/YunaiV/ruoyi-vue-pro.svg?style=social&label=Stars)](https://github.com/YunaiV/ruoyi-vue-pro/tree/master/rongdan-ui-app)                   | 基于 uni-app + uview 实现的用户 App    |

## 😎 开源协议

**为什么推荐使用本项目？**

① 本项目采用比 Apache 2.0 更宽松的 [MIT License](https://gitee.com/zhijiantianya/ruoyi-vue-pro/blob/master/LICENSE) 开源协议，个人与企业可 100% 免费使用，不用保留类作者、Copyright 信息。

② 代码全部开源，不会像其他项目一样，只开源部分代码，让你无法了解整个项目的架构设计。[国产开源项目对比](https://www.yuque.com/xiatian-bsgny/lm0ec1/wqf8mn)

![开源项目对比](https://static.iocoder.cn/project-vs.png?imageView2/2/format/webp/w/1280)

③ 代码整洁、架构整洁，遵循《阿里巴巴 Java 开发手册》规范，代码注释详细，57000 行 Java 代码，22000 行代码注释。

## 🐼 内置功能

系统内置多种多种业务功能，可以用于快速你的业务系统：

![功能分层](https://static.iocoder.cn/ruoyi-vue-pro-biz.png)

* 系统功能
* 基础设施
* 工作流程
* 支付系统
* 会员中心
* 数据报表
* 商城系统
* 微信公众号

> 友情提示：本项目基于 RuoYi-Vue 修改，**重构优化**后端的代码，**美化**前端的界面。
>
> * 额外新增的功能，我们使用 🚀 标记。
> * 重新实现的功能，我们使用 ⭐️ 标记。

🙂 所有功能，都通过 **单元测试** 保证高质量。

### 系统功能

|     | 功能    | 描述                              |
|-----|-------|---------------------------------|
|     | 用户管理  | 用户是系统操作者，该功能主要完成系统用户配置          |
| ⭐️  | 在线用户  | 当前系统中活跃用户状态监控，支持手动踢下线           |
|     | 角色管理  | 角色菜单权限分配、设置角色按机构进行数据范围权限划分      |
|     | 菜单管理  | 配置系统菜单、操作权限、按钮权限标识等，本地缓存提供性能    |
|     | 部门管理  | 配置系统组织机构（公司、部门、小组），树结构展现支持数据权限  |
|     | 岗位管理  | 配置系统用户所属担任职务                    |
| 🚀  | 租户管理  | 配置系统租户，支持 SaaS 场景下的多租户功能        |
| 🚀  | 租户套餐  | 配置租户套餐，自定每个租户的菜单、操作、按钮的权限       |
|     | 字典管理  | 对系统中经常使用的一些较为固定的数据进行维护          |
| 🚀  | 短信管理  | 短信渠道、短息模板、短信日志，对接阿里云、腾讯云等主流短信平台 |
| 🚀  | 邮件管理  | 邮箱账号、邮件模版、邮件发送日志，支持所有邮件平台       |
| 🚀  | 站内信   | 系统内的消息通知，提供站内信模版、站内信消息          |
| 🚀  | 操作日志  | 系统正常操作日志记录和查询，集成 Swagger 生成日志内容 |
| ⭐️  | 登录日志  | 系统登录日志记录查询，包含登录异常               |
| 🚀  | 错误码管理 | 系统所有错误码的管理，可在线修改错误提示，无需重启服务     |
|     | 通知公告  | 系统通知公告信息发布维护                    |
| 🚀  | 敏感词   | 配置系统敏感词，支持标签分组                  |
| 🚀  | 应用管理  | 管理 SSO 单点登录的应用，支持多种 OAuth2 授权方式 |
| 🚀  | 地区管理  | 展示省份、城市、区镇等城市信息，支持 IP 对应城市      |

### 工作流程

|     | 功能    | 描述                                     |
|-----|-------|----------------------------------------|
| 🚀  | 流程模型  | 配置工作流的流程模型，支持文件导入与在线设计流程图，提供 7 种任务分配规则 |
| 🚀  | 流程表单  | 拖动表单元素生成相应的工作流表单，覆盖 Element UI 所有的表单组件 |
| 🚀  | 用户分组  | 自定义用户分组，可用于工作流的审批分组                    |
| 🚀  | 我的流程  | 查看我发起的工作流程，支持新建、取消流程等操作，高亮流程图、审批时间线    |
| 🚀  | 待办任务  | 查看自己【未】审批的工作任务，支持通过、不通过、转发、委派、退回等操作    |
| 🚀  | 已办任务  | 查看自己【已】审批的工作任务，未来会支持回退操作               |
| 🚀  | OA 请假 | 作为业务自定义接入工作流的使用示例，只需创建请求对应的工作流程，即可进行审批 |

### 支付系统

|     | 功能   | 描述                        |
|-----|------|---------------------------|
| 🚀  | 商户信息 | 管理商户信息，支持 Saas 场景下的多商户功能  |
| 🚀  | 应用信息 | 配置商户的应用信息，对接支付宝、微信等多个支付渠道 |
| 🚀  | 支付订单 | 查看用户发起的支付宝、微信等的【支付】订单     |
| 🚀  | 退款订单 | 查看用户发起的支付宝、微信等的【退款】订单     |

ps：核心功能已经实现，正在对接微信小程序中...

### 基础设施

|     | 功能       | 描述                                           |
|-----|----------|----------------------------------------------|
| 🚀  | 代码生成     | 前后端代码的生成（Java、Vue、SQL、单元测试），支持 CRUD 下载       |
| 🚀  | 系统接口     | 基于 Swagger 自动生成相关的 RESTful API 接口文档          |
| 🚀  | 数据库文档    | 基于 Screw 自动生成数据库文档，支持导出 Word、HTML、MD 格式      |
|     | 表单构建     | 拖动表单元素生成相应的 HTML 代码，支持导出 JSON、Vue 文件         |
| 🚀  | 配置管理     | 对系统动态配置常用参数，支持 SpringBoot 加载                 |
| ⭐️  | 定时任务     | 在线（添加、修改、删除)任务调度包含执行结果日志                     |
| 🚀  | 文件服务     | 支持将文件存储到 S3（MinIO、阿里云、腾讯云、七牛云）、本地、FTP、数据库等   | 
| 🚀  | API 日志   | 包括 RESTful API 访问日志、异常日志两部分，方便排查 API 相关的问题   |
|     | MySQL 监控 | 监视当前系统数据库连接池状态，可进行分析SQL找出系统性能瓶颈              |
|     | Redis 监控 | 监控 Redis 数据库的使用情况，使用的 Redis Key 管理           |
| 🚀  | 消息队列     | 基于 Redis 实现消息队列，Stream 提供集群消费，Pub/Sub 提供广播消费 |
| 🚀  | Java 监控  | 基于 Spring Boot Admin 实现 Java 应用的监控           |
| 🚀  | 链路追踪     | 接入 SkyWalking 组件，实现链路追踪                      |
| 🚀  | 日志中心     | 接入 SkyWalking 组件，实现日志中心                      |
| 🚀  | 分布式锁     | 基于 Redis 实现分布式锁，满足并发场景                       |
| 🚀  | 幂等组件     | 基于 Redis 实现幂等组件，解决重复请求问题                     |
| 🚀  | 服务保障     | 基于 Resilience4j 实现服务的稳定性，包括限流、熔断等功能          |
| 🚀  | 日志服务     | 轻量级日志中心，查看远程服务器的日志                           |
| 🚀  | 单元测试     | 基于 JUnit + Mockito 实现单元测试，保证功能的正确性、代码的质量等    |


## 🐨 技术栈

### 模块

| 项目                                                                       | 说明                 |
|--------------------------------------------------------------------------|--------------------|
| `rongdan-dependencies`                                                     | Maven 依赖版本管理       |
| `rongdan-framework`                                                        | Java 框架拓展          |
| `rongdan-server`                                                           | 管理后台 + 用户 APP 的服务端 |
| `rongdan-module-system`                                                    | 系统功能的 Module 模块    |
| `rongdan-module-infra`                                                     | 基础设施的 Module 模块    |
| `rongdan-module-bpm`                                                       | 工作流程的 Module 模块    |

### 框架

| 框架                                                                                          | 说明               | 版本          | 学习指南                                                           |
|---------------------------------------------------------------------------------------------|------------------|-------------|----------------------------------------------------------------|
| [Spring Boot](https://spring.io/projects/spring-boot)                                       | 应用开发框架           | 2.7.8       | [文档](https://github.com/YunaiV/SpringBoot-Labs)                |
| [MySQL](https://www.mysql.com/cn/)                                                          | 数据库服务器           | 5.7 / 8.0+  |                                                                |
| [Druid](https://github.com/alibaba/druid)                                                   | JDBC 连接池、监控组件    | 1.2.15      | [文档](http://www.iocoder.cn/Spring-Boot/datasource-pool/?yudao) |
| [MyBatis Plus](https://mp.baomidou.com/)                                                    | MyBatis 增强工具包    | 3.5.3.1     | [文档](http://www.iocoder.cn/Spring-Boot/MyBatis/?yudao)         |
| [Dynamic Datasource](https://dynamic-datasource.com/)                                       | 动态数据源            | 3.6.1       | [文档](http://www.iocoder.cn/Spring-Boot/datasource-pool/?yudao) |
| [Redis](https://redis.io/)                                                                  | key-value 数据库    | 5.0 / 6.0   |                                                                |
| [Redisson](https://github.com/redisson/redisson)                                            | Redis 客户端        | 3.18.0      | [文档](http://www.iocoder.cn/Spring-Boot/Redis/?yudao)           |
| [Spring MVC](https://github.com/spring-projects/spring-framework/tree/master/spring-webmvc) | MVC 框架           | 5.3.24      | [文档](http://www.iocoder.cn/SpringMVC/MVC/?yudao)               |
| [Spring Security](https://github.com/spring-projects/spring-security)                       | Spring 安全框架      | 5.7.6       | [文档](http://www.iocoder.cn/Spring-Boot/Spring-Security/?yudao) |
| [Hibernate Validator](https://github.com/hibernate/hibernate-validator)                     | 参数校验组件           | 6.2.5       | [文档](http://www.iocoder.cn/Spring-Boot/Validation/?yudao)      |
| [Flowable](https://github.com/flowable/flowable-engine)                                     | 工作流引擎            | 6.8.0       | [文档](https://doc.iocoder.cn/bpm/)                              |
| [Quartz](https://github.com/quartz-scheduler)                                               | 任务调度组件           | 2.3.2       | [文档](http://www.iocoder.cn/Spring-Boot/Job/?yudao)             |
| [Knife4j](https://gitee.com/xiaoym/knife4j)                                                 | Swagger 增强 UI 实现 | 4.0.0       | [文档](http://www.iocoder.cn/Spring-Boot/Swagger/?yudao)         |
| [Resilience4j](https://github.com/resilience4j/resilience4j)                                | 服务保障组件           | 1.7.1       | [文档](http://www.iocoder.cn/Spring-Boot/Resilience4j/?yudao)    |
| [SkyWalking](https://skywalking.apache.org/)                                                | 分布式应用追踪系统        | 8.12.0      | [文档](http://www.iocoder.cn/Spring-Boot/SkyWalking/?yudao)      |
| [Spring Boot Admin](https://github.com/codecentric/spring-boot-admin)                       | Spring Boot 监控平台 | 2.7.10      | [文档](http://www.iocoder.cn/Spring-Boot/Admin/?yudao)           |
| [Jackson](https://github.com/FasterXML/jackson)                                             | JSON 工具库         | 2.13.3      |                                                                |
| [MapStruct](https://mapstruct.org/)                                                         | Java Bean 转换     | 1.5.3.Final | [文档](http://www.iocoder.cn/Spring-Boot/MapStruct/?yudao)       |
| [Lombok](https://projectlombok.org/)                                                        | 消除冗长的 Java 代码    | 1.18.24     | [文档](http://www.iocoder.cn/Spring-Boot/Lombok/?yudao)          |
| [JUnit](https://junit.org/junit5/)                                                          | Java 单元测试框架      | 5.8.2       | -                                                              |
| [Mockito](https://github.com/mockito/mockito)                                               | Java Mock 框架     | 4.8.0       | -                                                              |
