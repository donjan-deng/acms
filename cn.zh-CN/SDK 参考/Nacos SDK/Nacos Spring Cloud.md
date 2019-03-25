# Nacos Spring Cloud {#concept_edp_2fc_pfb .concept}

本文介绍 spring-cloud-starter-alibaba-nacos-config 的接入方法。

## 准备工作 {#section_ktm_h5r_ngb .section}

-   您已成功[创建命名空间](../../../../../intl.zh-CN/用户指南/创建命名空间.md#)。
-   您已成功[创建配置](../../../../../intl.zh-CN/用户指南/创建配置.md#)，并将配置格式选择 `Properties`。

**说明：** 

打开 ACM 控制台，创建配置。

-   Data ID：本例中为 `com.alibaba.nacos.example.properties`

-   Group：本例中为 `DEFAULT_GROUP`
-   配置格式选择 `Properties`，配置内容即为想要注入到应用中的具体 key-value，本例中为：


```
connectTimeoutInMills=3000
```

## Nacos Spring Cloud SDK 的使用步骤 {#section_dkp_ywr_ngb .section}

您可以下载[demo 工程](https://github.com/nacos-group/nacos-examples/tree/acm/nacos-spring-cloud-example/nacos-spring-cloud-config-example)来操作以下步骤。

1.  在您的 Maven 项目的 pom.xml 文件中增加以下配置来获取 Starter。

    ```
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```

    **说明：** spring-cloud-starter-alibaba-nacos-config 版本为 0.2.x.RELEASE 对应的是 Spring Boot 2.x 版本；版本 0.1.x.RELEASE 对应的是 Spring Boot 1.x 版本。使用时请根据 Spring Boot 版本选择相应的 spring-cloud-starter-alibaba-nacos-config 版本。

2.  在 bootstrap.properties 文件中配置连接信息。

    **说明：** 

    -   请将 `${endpoint}`、`${namespace}`、`${accessKey}` 和 `${secretKey}` 替换成您的真实信息。
    -   在控制台选择**命名空间**标签页，在命名空间列表中单击目标命名空间**操作**栏的**详情**，即可查看 endpoint 和 namespace 等信息。
    ```
    # 在命名空间详情处可以获取到 endpoint 和 namespace 的值
    spring.cloud.nacos.config.endpoint=${endpoint}
    spring.cloud.nacos.config.namespace=${namespace}
    # 推荐使用 RAM 账户的 accessKey 和 secretKey
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

    **说明：** 

    -   ACM 配置的 Data ID 按照以下约定格式：

        `${spring.application.name}.${spring.cloud.nacos.config.file-extension}`

        如本例中对应的 ACM 配置的 Data ID 为 `com.alibaba.nacos.example.properties`

3.  在您的工程中，使用 Spring 的注解 `@Value` 设置属性值，使用 Spring Cloud 原生注解 `@RefreshScope` 实现配置自动更新。例如：

    ```
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

4.  打开 ACM 控制台，在配置列表中的目标配置右侧**操作**栏单击**编辑**，更改配置内容为想要注入到应用中的具体 key-value 并单击**发布**。例如：

    ```
    connectTimeoutInMills=6000
    ```

    动态发布配置之后，本地工程的 Console 区域会打印日志，您可以根据日志信息检查是否发布成功。


## 相关文档 {#section_apj_hjx_fgb .section}

-   [Spring Cloud Alibaba Nacos Config](https://github.com/spring-cloud-incubator/spring-cloud-alibaba/wiki/Nacos-config)

-   [ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/articles/688108)
-   [公开课视频：ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/live/853)

