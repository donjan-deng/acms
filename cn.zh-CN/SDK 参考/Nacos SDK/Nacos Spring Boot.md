# Nacos Spring Boot {#concept_ydg_v2v_4fb .concept}

本文说明如何使用 Nacos Spring Boot SDK 管理 ACM 配置。

## 前提条件 {#section_52h_9q0_6cg .section}

-   登录 [ACM 控制台](https://acm.console.alibabacloud.com/)，并创建一个示例配置。

    -   Data ID：com.alibaba.nacos.example.properties
    -   Group：不填写，即使用默认的 DEFAULT\_GROUP。
    -   配置格式：Properties
    -   配置内容：connectTimeoutInMills=3000
-   （可选）下载[样例工程](https://github.com/nacos-group/nacos-examples/tree/acm/nacos-spring-boot-example/nacos-spring-boot-config-example)。

## 操作步骤 {#section_g7h_09d_sva .section}

1.  在 Maven 项目的 pom.xml 文件中增加以下配置来获取 Starter。

    ``` {#codeblock_chh_wua_cle .lanuage-shell}
    <dependency>
        <groupId>com.alibaba.boot</groupId>
        <artifactId>nacos-config-spring-boot-starter</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```

    **说明：** 使用时请根据 Spring Boot 版本选择相应的 nacos-config-spring-boot-starter 版本：nacos-config-spring-boot-starter 版本 0.2.x.RELEASE 对应 Spring Boot 2.x 版本，版本 0.1.x.RELEASE 对应 Spring Boot 1.x 版本。

2.  在 application.properties 文件中配置连接信息。

    **说明：** 请将代码中的 $\{endpoint\}、$\{namespace\}、$\{accessKey\}、$\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 **End Point**、**命名空间 ID**、**AccessKey**、**SecretKey**。出于安全考虑，建议使用 RAM 用户的 AccessKey 和 SecretKey。

    ``` {#codeblock_oc3_6ar_gbj .language-java}
    nacos.config.endpoint=${endpoint}
    nacos.config.namespace=${namespace}
    # 推荐使用 RAM 用户的 accessKey 和 secretKey
    nacos.config.access-key=${accessKey}
    nacos.config.secret-key=${secretKey}
    ```

3.  使用 @NacosPropertySource 注解加载配置源，并开启自动更新。

    **说明：** 实际开发时，请将 `com.alibaba.nacos.example.properties` 替换成真实的 ACM 配置 Data ID。

    ``` {#codeblock_p12_qxw_oak .language-java}
    @SpringBootApplication
    @NacosPropertySource(dataId = "com.alibaba.nacos.example.properties", autoRefreshed = true)
    public class NacosConfigApplication {
        public static void main(String[] args) {
            SpringApplication.run(NacosConfigApplication.class, args);
        }
    }
    ```

4.  使用 `@NacosValue` 注解设置属性值。

    ``` {#codeblock_cq7_rmx_tdo .language-java}
    @Controller
    @RequestMapping("config")
    public class ConfigController {
    
        @NacosValue(value = "${connectTimeoutInMills:5000}", autoRefreshed = true)
        private int connectTimeoutInMills;
    
        @RequestMapping(value = "/get", method = GET)
        @ResponseBody
        public int get() {
            return connectTimeoutInMills;
        }
    }
    ```


## 结果验证 {#section_ryx_26p_h46 .section}

1.  在本地启动项目，并运行以下命令：

    ``` {#d7e557 .lanuage-shell}
    curl localhost:8080/config/get
    ```

    若返回以下信息，则说明 SDK 可正常使用。

    ``` {#d7e563 .screen}
    3000
    ```

2.  在 ACM 控制台将示例配置 `com.alibaba.nacos.example.properties` 更改为以下内容并发布。

    ``` {#d7e574 .lanuage-shell}
    connectTimeoutInMills=6000
    ```

    若 Console 打印出更新的配置内容 ，则说明 SDK 的配置自动更新功能正常。


## 更多信息 {#section_c4l_zhx_fgb .section}

-   [样例工程](https://github.com/nacos-group/nacos-examples/tree/acm/nacos-spring-boot-example/nacos-spring-boot-config-example)
-   [创建配置](../intl.zh-CN/用户指南/创建配置.md#)
-   [管理配置](../intl.zh-CN/用户指南/管理配置.md#)
-   [Nacos Config Spring Boot](https://github.com/nacos-group/nacos-spring-boot-project/blob/master/NACOS-CONFIG-QUICK-START.md)
-   [ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/articles/688108)
-   [公开课视频：ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/live/853)
-   [Nacos Spring Boot 0.2.2 以及 0.1.2 版本支持 YAML、JSON、XML 等格式](https://github.com/nacos-group/nacos-spring-boot-project/wiki/spring-boot-0.2.2-%E4%BB%A5%E5%8F%8A-0.1.2%E7%89%88%E6%9C%AC%E6%96%B0%E5%8A%9F%E8%83%BD%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C#%E6%94%AF%E6%8C%81%E5%A4%9A%E7%A7%8D%E9%85%8D%E7%BD%AE%E6%A0%BC%E5%BC%8Fpropertiesyamljsonxml)

