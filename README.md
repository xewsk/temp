import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.FastAutoGenerator;
import com.baomidou.mybatisplus.generator.config.OutputFile;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.sddbjt.boot.framework.mybatis.core.dataobject.BaseDO;

import java.util.Collections;

public class FastAutoGeneratorTest {

    /**
     * 执行 run
     */
    public static void main(String[] args) {
        //1、配置数据源
// 《=============================需要修改的地方================================》
        FastAutoGenerator.create("jdbc:mysql://localhost:3306/workflow?tinyInt1isBit=false&serverTimezone=GMT%2B8", "root", "4624346")
                //2、全局配置
                .globalConfig(builder -> {
                    builder
                            //（重要）覆盖已生成文件，谨慎配置，开启后是整个文件夹覆盖替换，此时建议开启设置父包模块名.moduleName("")
                            /*（重要）指定输出目录
                                 指定输出目录，springboot项目可以使用如下方式，相对目录，自动生成当前项目目录
                                .outputDir(System.getProperty("user.dir") + "/src/main/java")
                             */
// 《=============================需要修改的地方================================》
                            // 设置输出目录,当前项目目录下的src/main/java
                            .outputDir(System.getProperty("user.dir") +"/boot-module-user/boot-module-user-biz"+ "/src/main/java")
                            // 设置作者
                            .author("cc")
//                            //开启 swagger 模式
//                            .enableSwagger()
                            //（重要）时间类型，看你喜欢用sql包中的Date、util包中的Date还是更新的LocalDateTime
                            .dateType(DateType.TIME_PACK)
                            // 注释日期的格式
                            .commentDate("yyyy-MM-dd");
                })
                //3、包配置
                .packageConfig(builder -> {
                    builder
// 《=============================需要修改的地方================================》
                            // 设置父包名
                            .parent("com.sddbjt.boot.module")
                            // 设置父包模块名 .moduleName("system")
                            .moduleName("test")
                            //Entity 包名
                            .entity("domain")
                            //Service 包名
                            .service("service")
                            //Service Impl 包名
                            .serviceImpl("service.impl")
                            //Mapper 包名
                            .mapper("mapper")
                            //Mapper XML 包名
                            .xml("mapper.xml")
                            //Controller 包名
                            .controller("controller")
                            //路径配置信息
                            //设置mapperxml生成到resources下
                            .pathInfo(Collections.singletonMap(OutputFile.xml, "src/main/resources/mapper"));
                })
                //4、策略配置
                .strategyConfig(builder -> {
                    builder
// 《=============================需要修改的地方================================》
                            // 设置需要生成的表名
                            .addInclude("sys_role", "sys_post")
                            // 设置过滤表前缀
                            .addTablePrefix("sys_")
                            // 跳过视图的生成
                            .enableSkipView()
                            //Entity 策略配置
                            .entityBuilder()
                            // （重要）主键模式，这里设置自动模式，配合mysql的自增主键
                            .idType(IdType.ASSIGN_ID)
                            //开启 lombok 模型
                            .enableLombok()
                            //开启生成实体时生成字段注解
                            .enableTableFieldAnnotation()
                            .superClass(BaseDO.class)
                            // entity文件名，这里配置后面统一加Entity后缀
                            .formatFileName("%sDO")
                            //service 策略配置
                            .serviceBuilder()
                            .formatServiceFileName("%sService")
                            // activeRecord模式，使用上来说就是
                            // 可以直接在entity对象上执行insert、update等操作
                            //.enableActiveRecord()
                            //Mapper 策略配置
                            .mapperBuilder()
                            //开启 @Mapper 注解
                            .enableMapperAnnotation()
                            //启用 BaseResultMap 生成
                            .enableBaseResultMap()
                            //启用 BaseColumnList


                            .enableBaseColumnList()
                            //Controller 策略配置
                            .controllerBuilder()
                            //开启驼峰转连字符
                            .enableHyphenStyle()
                            //开启生成@RestController 控制器
                            .enableRestStyle();


                })
                // 5、使用Freemarker引擎模板，默认的是Velocity引擎模板
//                .templateEngine(new FreemarkerTemplateEngine())
                // 6、执行
                .execute();



    }

}
