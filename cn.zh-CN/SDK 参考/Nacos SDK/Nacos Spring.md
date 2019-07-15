# Nacos Spring {#concept_d45_dy5_4fb .concept}

本文说明如何使用 Nacos Spring SDK 管理 ACM 配置。

## 前提条件 {#section_ktm_h5r_ngb .section}

-   登录 [ACM 控制台](https://acm.console.alibabacloud.com/)，并创建一个示例配置。

    -   Data ID：com.alibaba.nacos.example
    -   Group：不填写，即使用默认 Group。
    -   配置格式：Properties
    -   配置内容：connectTimeoutInMills=3000
-   （可选）下载[样例工程](https://github.com/acm-group/nacos-spring-config-example)。

## 操作步骤 {#section_uxl_jrs_ngb .section}

1.  在 Maven 项目的 pom.xml 文件中增加以下配置来获取 Nacos Spring SDK。

    ``` {#codeblock_kdi_p5k_663}
    <dependency>
        <groupId>com.alibaba.nacos</groupId>
        <artifactId>nacos-spring-context</artifactId>
        <version>${latest.version}</version>
    </dependency>
    ```

2.  使用 @EnableNacosConfig 注解启用 Nacos Spring 的配置管理服务。

    **说明：** 请将代码中的 $\{endpoint\}、$\{namespace\}、$\{accessKey\}、$\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 **End Point**、**命名空间 ID**、**AccessKey**、**SecretKey**。

    ``` {#codeblock_u6g_9ng_fj5}
    @Configuration
    // 在命名空间详情对话框内可以获取 endpoint 和 namespace；推荐使用 RAM 用户的 accessKey 和 secretKey
    @EnableNacosConfig(globalProperties = @NacosProperties(
        endpoint = "${endpoint}", 
        namespace = "${namespace}",
        accessKey = "${accessKey}", 
        secretKey = "${secretKey}"
    ))
    ```

3.  使用 @NacosPropertySource 注解加载配置源，并开启自动更新。

    **说明：** 实际开发时，请将 `com.alibaba.nacos.example` 替换成真实的 ACM 配置 Data ID。

    ``` {#codeblock_zzb_8nx_bff}
    @NacosPropertySource(dataId = "com.alibaba.nacos.example", autoRefreshed = true)
    public class NacosConfiguration {
    
    }
    ```

4.  使用 `@NacosValue` 注解设置属性值。

    ``` {#codeblock_cq7_rmx_tdo}
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


## 结果验证 {#section_hyd_tyj_k3b .section}

1.  在本地启动 Maven 项目。

    若 Console 打印出以下信息，则说明 Nacos Spring SDK 可正常使用。

    ``` {#screen_ylv_mt6_yig .screen}
    17:04:20.523 [RMI TCP Connection(2)-127.0.0.1] DEBUG com.alibaba.nacos.spring.context.annotation.config.NacosValueAnnotationBeanPostProcessor - Update value of the connectTimeoutInMills (field) in configController (bean) with 3000
    ```

2.  按照 [编辑配置](../../../../intl.zh-CN/用户指南/管理配置.md#section_az4_lkt_42b)的说明，在 ACM 控制台将示例配置 `com.alibaba.nacos.example` 更改为以下内容并发布。

    ``` {#codeblock_5az_j8x_wax}
    connectTimeoutInMills=6000
    ```

    若 Console 打印出以下信息，则说明 Nacos Spring SDK 的配置自动更新功能正常。

    ``` {#screen_696_egz_i6b .screen}
    17:07:31.033 [NacosConfigListener-ThreadPool-2] DEBUG com.alibaba.nacos.spring.context.annotation.config.NacosValueAnnotationBeanPostProcessor - Update value of the connectTimeoutInMills (field) in configController (bean) with 6000
    ```


## 更多信息 {#section_jpz_z3x_fgb .section}

-   [样例工程](https://github.com/acm-group/nacos-spring-config-example)
-   [管理配置](../../../../intl.zh-CN/用户指南/管理配置.md#)
-   [Nacos Spring Project](https://github.com/nacos-group/nacos-spring-project/blob/master/README.md)
-   [ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/articles/688108)
-   [公开课视频：ACM 无缝支持 Spring 全栈](https://yq.aliyun.com/live/853)

