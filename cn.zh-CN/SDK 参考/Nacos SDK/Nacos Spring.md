# Nacos Spring {#concept_d45_dy5_4fb .concept}

本文说明如何使用 Nacos Spring SDK 管理 ACM 配置。

## 前提条件 {#section_ktm_h5r_ngb .section}

-   登录 [ACM 控制台](https://acm.console.alibabacloud.com/)，并创建一个示例配置。

    -   Data ID：com.alibaba.nacos.example.properties
    -   Group：不填写，即使用默认的 DEFAULT\_GROUP。
    -   配置格式：Properties
    -   配置内容：connectTimeoutInMills=3000
-   （可选）下载[样例工程](https://github.com/acm-group/nacos-spring-config-example)。

## 操作步骤 {#section_uxl_jrs_ngb .section}

1.  在 Maven 项目的 pom.xml 文件中增加以下配置来获取 Nacos Spring SDK。

    ``` {#codeblock_kdi_p5k_663 .lanuage-go}
    <dependency>
        <groupId>com.alibaba.nacos</groupId>
        <artifactId>nacos-spring-context</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```

2.  使用 @EnableNacosConfig 注解启用 Nacos Spring 的配置管理服务。

    **说明：** 请将代码中的 $\{endpoint\}、$\{namespace\}、$\{accessKey\}、$\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 **End Point**、**命名空间 ID**、**AccessKey**、**SecretKey**。出于安全考虑，建议使用 RAM 用户的 AccessKey 和 SecretKey。

    ``` {#codeblock_u6g_9ng_fj5 .language-java}
    @Configuration
    @EnableNacosConfig(globalProperties = @NacosProperties(
        endpoint = "${endpoint}", 
        namespace = "${namespace}",
        accessKey = "${accessKey}", 
        secretKey = "${secretKey}"
    ))
    ```

3.  使用 @NacosPropertySource 注解加载配置源，并开启自动更新。

    **说明：** 实际开发时，请将 `com.alibaba.nacos.example.properties` 替换成真实的 ACM 配置 Data ID。

    ``` {#codeblock_zzb_8nx_bff .language-java}
    @NacosPropertySource(dataId = "com.alibaba.nacos.example.properties", autoRefreshed = true)
    public class NacosConfiguration {
    
    }
    ```

4.  使用 `@NacosValue` 注解设置属性值。

    ``` {#codeblock_cq7_rmx_tdo .language-java}
    @Controller
    @RequestMapping("config")
    public class ConfigController {
    
        @NacosInjected
        private ConfigService configService;
    
        @NacosValue(value = "${connectTimeoutInMills:5000}", autoRefreshed = true)
        private int connectTimeoutInMills;
    
        @RequestMapping(value = "/get", method = GET)
        @ResponseBody
        public int get() {
            return connectTimeoutInMills;
        }
    }
    ```


## 结果验证 {#section_20u_ivy_926 .section}

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


## 更多信息 {#section_jpz_z3x_fgb .section}

-   [样例工程](https://github.com/acm-group/nacos-spring-config-example)
-   [创建配置](../intl.zh-CN/用户指南/创建配置.md#)
-   [管理配置](../intl.zh-CN/用户指南/管理配置.md#)
-   [Nacos Spring Project](https://github.com/nacos-group/nacos-spring-project/blob/master/README.md)
-   [ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/articles/688108)
-   [公开课视频：ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/live/853)
-   [Nacos Spring 0.3.1 版本支持 YAML、JSON、XML 等格式](https://github.com/nacos-group/nacos-spring-project/wiki/Nacos-Spring-Project-0.3.1-%E6%96%B0%E5%8A%9F%E8%83%BD%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C#%E5%A4%9A%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%94%AF%E6%8C%81)

