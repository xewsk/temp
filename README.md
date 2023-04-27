# temp

源码中的解释：

指示应将@ConfigurationProperties对象中的字段视为嵌套类型。此注释与实际绑定过程无关，但它被spring-boot配置处理器用作一个提示，表明字段未绑定为单个值。指定此选项后，将为字段创建嵌套组，并获取其类型。

意思就是说，在@ConfigurationProperties修饰的对象中，如果有成员属性被@NestedConfigurationProperty修饰，那么这个属性将不再是可以绑定的单个值，而是作为一个属性嵌套组。

@Getter
@Setter
public class XxlJobAdminProperties {
 
    private String address;
}
@Getter
@Setter
public class XxlJobExecutorProperties {
 
    /**
     * 执行器AppName [选填]：执行器心跳注册分组依据；为空则关闭自动注册
     */
    private String appname;
 
    /**
     * 服务注册地址,优先使用该配置作为注册地址 为空时使用内嵌服务 ”IP:PORT“ 作为注册地址 从而更灵活的支持容器类型执行器动态IP和动态映射端口问题
     */
    private String address;
 
    /**
     * 执行器IP [选填]：默认为空表示自动获取IP，多网卡时可手动设置指定IP ，该IP不会绑定Host仅作为通讯实用；地址信息用于 "执行器注册" 和
     * "调度中心请求并触发任务"
     */
    private String ip;
 
    /**
     * 执行器端口号 [选填]：小于等于0则自动获取；默认端口为9099，单机部署多个执行器时，注意要配置不同执行器端口；
     */
    private Integer port = 9099;
 
    /**
     * 执行器通讯TOKEN [必填]：从配置文件中取不到值时使用默认值；
     */
    private String accessToken = "default_token";
 
    /**
     * 执行器运行日志文件存储磁盘路径 [选填] ：需要对该路径拥有读写权限；为空则使用默认路径；
     */
    private String logPath = "logs/applogs/xxl-job/jobhandler";
 
    /**
     * 执行器日志保存天数 [选填] ：值大于3时生效，启用执行器Log文件定期清理功能，否则不生效；
     */
    private Integer logRetentionDays = 30;
 
}
@Getter
@Setter
@Component
@ConfigurationProperties(prefix = "xxl.job")
public class XxlJobProperties {
 
    @NestedConfigurationProperty
    private XxlJobAdminProperties admin;
 
    @NestedConfigurationProperty
    private XxlJobExecutorProperties executor;
 
}
例如上面的XxlJobProperties这个类，如果其中的属性都不加@NestedConfigurationProperty注解，那么只能配置到 xxl.job.admin 和 xxl.job.executor 这个层级，而加了该注解，那么配置层级可以映射到 admin 和 executor 中的内部成员属性。

没有加该注解的可配置层级：

xxl:
  job:
    admin: xxl-job-executor-order
    executor: xxl-job-executor-order
加了该注解后的可配置层级：

xxl:
  job:
    admin:
      address: 192.168.56.10:8090/xxl-job-admin
    executor:
      address: 127.0.0.1:9007
      appname: xxl-job-executor-order
      ip: 127.0.0.1
      port: 9007
      log-path: /data/applogs/xxl-job/jobhandler
      log-retention-days: 30
      access-token: xxl-job-executor-order
————————————————
版权声明：本文为CSDN博主「龙茶清欢」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/python15397/article/details/129052427
