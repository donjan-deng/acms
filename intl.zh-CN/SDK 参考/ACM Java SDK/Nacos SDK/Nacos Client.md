# Nacos Client {#concept_hwf_kmt_4fb .concept}

本文说明如何使用 Nacos Client SDK 管理 ACM 配置。

## 前提条件 {#section_skv_q0s_goh .section}

-   登录 [ACM 控制台](https://acm.console.alibabacloud.com/)，并创建一个示例配置。

    -   Data ID：com.alibaba.nacos.example.properties
    -   Group：不填写，即使用默认的 DEFAULT\_GROUP。
    -   配置格式：Properties
    -   配置内容：connectTimeoutInMills=3000

## 操作步骤 {#section_k34_vvs_ngb .section}

1.  在 Maven 项目的 pom.xml 文件中添加以下配置来获取 Nacos Client SDK。

    ``` {#codeblock_unr_72g_jnb}
    <dependency>
        <groupId>com.alibaba.nacos</groupId>
        <artifactId>nacos-client</artifactId>
        <version>${latest.version}</version>
    </dependency>
    <!--  有日志实现，下面可去掉  -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    ```

2.  在工程中添加以下代码进行配置管理。

    **说明：** 请将代码中的 $\{endpoint\}、$\{namespace\}、$\{accessKey\}、$\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 **End Point**、**命名空间 ID**、**AccessKey**、**SecretKey**。出于安全考虑，建议使用 RAM 用户的 AccessKey 和 SecretKey。

    ``` {#codeblock_v8p_h0m_jba}
    import com.alibaba.nacos.api.NacosFactory;
    import com.alibaba.nacos.api.PropertyKeyConst;
    import com.alibaba.nacos.api.config.ConfigService;
    import com.alibaba.nacos.api.exception.NacosException;
    import com.alibaba.nacos.client.config.listener.impl.PropertiesListener;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import java.util.Properties;
    /**
     * 示例代码，仅用于示例测试
     */
    public class NacosExample {
        private static final Logger LOGGER = LoggerFactory.getLogger(NacosExample.class);
        public static void main(String[] args) throws NacosException {
            Properties properties = new Properties();
            // 指定配置的 DataID 和 Group
            String dataId = "${dataId}";
            String group = "${group}";
            String content = "connectTimeoutInMills=5000";
            // 从控制台命名空间管理的"命名空间详情"中拷贝 endpoint、namespace 
            properties.put(PropertyKeyConst.ENDPOINT, "${endpoint}");
            properties.put(PropertyKeyConst.NAMESPACE, "${namespace}");
            // 推荐使用 RAM 账号的 accessKey、secretKey，
            properties.put(PropertyKeyConst.ACCESS_KEY, "${accessKey}");
            properties.put(PropertyKeyConst.SECRET_KEY, "${secretKey}");
            ConfigService configService = NacosFactory.createConfigService(properties);
            // 发布配置
            boolean publishConfig = configService.publishConfig(dataId, group, content);
            LOGGER.info("publishConfig: {}", publishConfig);
            wait2Sync();
            // 查询配置
            String config = configService.getConfig(dataId, group, 5000);
            LOGGER.info("getConfig: {}", config);
            // 监听配置
            configService.addListener(dataId, group, new PropertiesListener() {
                @Override
                public void innerReceive(Properties properties) {
                    LOGGER.info("innerReceive: {}", properties);
                }
            });
            // 更新配置
            boolean updateConfig = configService.publishConfig(dataId, group, "connectTimeoutInMills=3000");
            LOGGER.info("updateConfig: {}", updateConfig);
            wait2Sync();
            // 删除配置
            boolean removeConfig = configService.removeConfig(dataId, group);
            LOGGER.info("removeConfig: {}", removeConfig);
            wait2Sync();
            config = configService.getConfig(dataId, group, 5000);
            LOGGER.info("getConfig: {}", config);
        }
        private static void wait2Sync() {
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                // ignore
            }
        }
    }
    ```


## 结果验证 {#section_pxf_fkt_9jr .section}

1.  在本地启动项目，若 Console 打印出以下信息，则说明 SDK 可正常使用。

    ``` {#screen_4vw_1zf_p36 .screen}
    12:40:45.567 [main] INFO DemoConfig - getConfig: connectTimeoutInMills=3000
    ```

2.  在 ACM 控制台将示例配置 `com.alibaba.nacos.example.properties` 更改为以下内容并发布。

    ``` {#codeblock_9ai_263_pjv .lanuage-shell}
    connectTimeoutInMills=6000
    ```

    若 Console 打印出以下信息 ，则说明 SDK 的配置监听功能正常。

    ``` {#screen_a6v_euw_o1n .screen}
    [com.alibaba.nacos.client.Worker.longPolling.acm.aliyun.com-********] INFO DemoConfig - innerReceive: {connectTimeoutInMills=6000}
    ```


## 传参方式 {#section_thh_2vv_42b .section}

为帮助您快速入门，以上示例代码采用了以代码初始化参数的方式。但是实际生产中可能有不同环境（如不同的账号、地域或 等），参数因环境而异，因此需要通过变量传入。为方便配置入参、降低配置成本，ACM 针对不同的初始化参数分别提供了多种传参方式，详见下表。

**说明：** 对于阿里云 EDAS 用户而言，EDAS 会在发布环节自动注入参数，用户无需采取任何操作。

|初始化参数|传参方式|
|-----|----|
|endpoint|代码设置：详见以上示例代码。|
|namespace|优先级由高到低： 1.  JVM 参数：`-Dtenant.id=xxx`。
2.  代码设置：详见以上示例代码。

 |
|accessKey/secretKey|优先级由高到低： 1.  文件：accessKey 和 secretKey 以 Properties 格式（满足 `public void java.io.Reader.Properties.load(Reader reader)` 方法的要求）存储在 `-Dspas.identity` 指定的文件中。如果不指定，则默认取 `/home/admin/.spas_key/<应用名>`文件，<应用名\>以 `-Dproject.name` 指定。
2.  环境变量：`spas_accessKey=xxx spas_secretKey=xxx`
3.  代码设置：详见以上示例代码。

 |

## 更多信息 {#section_58b_t0z_r34 .section}

-   [创建配置](../intl.zh-CN/用户指南/创建配置.md#)
-   [管理配置](../intl.zh-CN/用户指南/管理配置.md#)

