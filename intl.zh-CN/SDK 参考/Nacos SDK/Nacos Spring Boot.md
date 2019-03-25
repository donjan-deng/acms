# Nacos Spring Boot {#concept_ydg_v2v_4fb .concept}

本文介绍 nacos-config-spring-boot-starter 的接入方法。

## 准备工作 {#section_ktm_h5r_ngb .section}

-   您已成功创建[创建命名空间](../../../../../intl.zh-CN/用户指南/创建命名空间.md#)。
-   您已成功[创建配置](../../../../../intl.zh-CN/用户指南/创建配置.md#)，并将配置格式选择 `Properties`。

**说明：** 

打开 ACM 控制台，创建配置。

-   Data ID ：本例中为 `com.alibaba.nacos.example.properties`

-   Group：本例中为 `DEFAULT_GROUP`
-   配置格式选择 `Properties`，配置内容即为想要注入到应用中的具体 key-value，本例中为：


```
connectTimeoutInMills=3000
```

## Nacos Spring Boot SDK 的使用步骤 {#section_v2r_3gs_ngb .section}

您可以下载[demo 工程](https://github.com/nacos-group/nacos-examples/tree/acm/nacos-spring-boot-example/nacos-spring-boot-config-example)来操作以下步骤。

1.  在您的 Maven 项目的 pom.xml 文件中增加以下配置来获取 Starter。

    ```
    <dependency>
        <groupId>com.alibaba.boot</groupId>
        <artifactId>nacos-config-spring-boot-starter</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```

    **说明：** nacos-config-spring-boot-starter 版本为 0.2.x.RELEASE 对应的是 Spring Boot 2.x 版本；版本 0.1.x.RELEASE 对应的是 Spring Boot 1.x 版本。使用时请根据 Spring Boot 版本选择相应的 nacos-config-spring-boot-starter 版本。

2.  在 application.properties 文件中配置连接信息。

    **说明：** 

    -   请将 `${endpoint}`、`${namespace}`、`${accessKey}` 和 `${secretKey}` 替换成您的真实信息。
    -   在控制台选择**命名空间**标签页，在命名空间列表中单击目标命名空间**操作**栏的**详情**，即可查看 endpoint 和 namespace 等信息。
    ```
    # 在命名空间详情处可以获取到 endpoint 和 namespace 的值
    nacos.config.endpoint=${endpoint}
    nacos.config.namespace=${namespace}
    # 推荐使用 RAM 账户的 accessKey 和 secretKey
    nacos.config.access-key=${accessKey}
    nacos.config.secret-key=${secretKey}
    
    ```

3.  在您的工程中，使用 `@NacosPropertySource` 加载配置源，并开启自动更新。

    **说明：** 实际开发时，请将 `com.alibaba.nacos.example.properties` 替换成您的 ACM 配置的 Data ID。

    ```
    @SpringBootApplication
    @NacosPropertySource(dataId = "com.alibaba.nacos.example.properties", autoRefreshed = true)
    public class NacosConfigApplication {
        public static void main(String[] args) {
            SpringApplication.run(NacosConfigApplication.class, args);
        }
    }
    ```

4.  使用 Nacos 的注解 `@NacosValue` 设置属性值。

    ```
    @Controller
    @RequestMapping("config")
    public class ConfigController {
        
        @NacosValue("${connectTimeoutInMills:5000}", autoRefreshed = true)
        private int connectTimeoutInMills;
      
        @RequestMapping(value = "/get", method = GET)
        @ResponseBody
        public int get() {
            return connectTimeoutInMills;
        }
    }
    ```

5.  打开 ACM 控制台，在配置列表中的目标配置右侧**操作**栏单击**编辑**，更改配置内容为想要注入到应用中的具体 key-value 并单击**发布**。例如：

    ```
    connectTimeoutInMills=6000
    ```

    动态发布配置之后，本地工程的 Console 区域会打印日志，您可以根据日志信息检查是否发布成功。


## 相关文档 {#section_c4l_zhx_fgb .section}

-    [Nacos Config Spring Boot](https://github.com/nacos-group/nacos-spring-boot-project/blob/master/NACOS-CONFIG-QUICK-START.md)

-   [ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/articles/688108)
-   [公开课视频：ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/live/853)

