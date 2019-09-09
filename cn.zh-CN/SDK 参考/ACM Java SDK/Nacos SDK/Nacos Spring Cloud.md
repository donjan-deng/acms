# Nacos Spring Cloud {#concept_edp_2fc_pfb .concept}

本文说明如何使用 Nacos Spring Cloud SDK 管理 ACM 配置。

## 前提条件 {#section_j35_70w_s6n .section}

-   登录 [ACM 控制台](https://acm.console.alibabacloud.com/)，并创建一个示例配置。

    -   Data ID：com.alibaba.nacos.example.properties
    -   Group：不填写，即使用默认的 DEFAULT\_GROUP。
    -   配置格式：Properties
    -   配置内容：connectTimeoutInMills=3000
-   （可选）下载[样例工程](https://github.com/nacos-group/nacos-examples/tree/acm/nacos-spring-cloud-example/nacos-spring-cloud-config-example)。

## 操作步骤 {#section_dkp_ywr_ngb .section}

1.  在 Maven 项目的 pom.xml 文件中增加以下配置来获取 Starter。

    ``` {#codeblock_7oc_7or_gyt}
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```

    **说明：** 使用时请根据 Spring Boot 版本选择相应的 spring-cloud-starter-alibaba-nacos-config 版本：spring-cloud-starter-alibaba-nacos-config 版本 0.2.x.RELEASE 对应 Spring Boot 2.x 版本，版本 0.1.x.RELEASE 对应 Spring Boot 1.x 版本。

2.  在 bootstrap.properties 文件中配置连接信息。

    **说明：** 请将代码中的 $\{endpoint\}、$\{namespace\}、$\{accessKey\}、$\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 **End Point**、**命名空间 ID**、**AccessKey**、**SecretKey**。出于安全考虑，建议使用 RAM 用户的 AccessKey 和 SecretKey。

    ``` {#codeblock_39q_s9b_2bd}
    spring.cloud.nacos.config.endpoint=${endpoint}
    spring.cloud.nacos.config.namespace=${namespace}
    # 推荐使用 RAM 用户的 accessKey 和 secretKey
    spring.cloud.nacos.config.access-key=${accessKey}
    spring.cloud.nacos.config.secret-key=${secretKey}
    # ACM 配置的 Data ID 等于 ${spring.application.name}.${spring.cloud.nacos.config.file-extension}
    # 指定配置的 Data ID 前缀，例如：
    spring.application.name=com.alibaba.nacos.example
    # 指定配置的 Data ID 后缀，支持 properties、yaml、yml，默认为 properties
    spring.cloud.nacos.config.file-extension=properties
    # 指定 ACM 配置的 Group，默认为 DEFAULT_GROUP
    spring.cloud.nacos.config.group=DEFAULT_GROUP
    ```

    **说明：** ACM 配置的 Data ID 的约定格式为 `${spring.application.name}.${spring.cloud.nacos.config.file-extension}`。本例中对应的 ACM 配置的 Data ID 为 `com.alibaba.nacos.example.properties`。

3.  使用 Spring 的注解 `@Value` 设置属性值，使用 Spring Cloud 原生注解 `@RefreshScope` 实现配置自动更新。

    ``` {#codeblock_cez_4lk_077}
    @RestController
    @RequestMapping("/config")
    @RefreshScope
    public class ConfigController {
    
        @Value("${connectTimeoutInMills:5000}")
        private int connectTimeoutInMills;
    
        public void setConnectTimeoutInMills(int connectTimeoutInMills) {
            this.connectTimeoutInMills = connectTimeoutInMills;
        }
    
        @RequestMapping(value = "/get", method = GET)
        @ResponseBody
        public int get() {
            return connectTimeoutInMills;
        }
    }
    ```


## 结果验证 {#section_jvf_dt4_bam .section}

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


## 更多信息 {#section_apj_hjx_fgb .section}

-   [样例工程](https://github.com/nacos-group/nacos-examples/tree/acm/nacos-spring-cloud-example/nacos-spring-cloud-config-example)
-   [创建配置](../intl.zh-CN/用户指南/创建配置.md#)
-   [管理配置](../intl.zh-CN/用户指南/管理配置.md#)
-   [Spring Cloud Alibaba Nacos Config](https://github.com/spring-cloud-incubator/spring-cloud-alibaba/wiki/Nacos-config)
-   [ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/articles/688108)
-   [公开课视频：ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/live/853)

