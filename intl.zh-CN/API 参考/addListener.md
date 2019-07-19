# addListener {#concept_k3y_qdv_42b .concept}

使用 addListener 接口监听 ACM 配置的变更。

## API 描述 {#section_zdg_5cv_42b .section}

addListener 接口可监听 ACM 上的配置，并实时感知配置变更。如果配置变更，则您可以用 [getConfig](intl.zh-CN/API 参考/getConfig.md#) 接口获取配置的最新值，并动态刷新本地缓存。

注册监听采用的是异步 Servlet 技术。注册监听的本质是将配置和配置值的 MD5 值与后台对比，如果 MD5 值不一致，则立即返回不一致的配置。如果 MD5 值一致，则等待 30 秒，且返回值为空。

## 请求类型 {#section_a2g_5cv_42b .section}

POST

## 请求 URL {#section_b2g_5cv_42b .section}

/diamond-server/config.co

## 请求参数 {#section_f2y_wcv_42b .section}

|参数|类型|是否必需|描述|
|--|--|----|--|
|Probe-Modify-Request|String|是|监听数据报文，格式为 `dataId^2group^2contentMD5^2tenant^1`。 -   配置的多个字段间分隔符：`(Java)^2 = Character.toString((char) 2)` 或 `(Shell) ^2=%02`。
-   配置间分隔符：`(Java) ^1 = Character.toString((char) 1)` 或 `(Shell) ^1=%01`
-   contentMD5：MD5\(content\)。第一次本地缓存为空，所以为空串。

 |

## Header 参数 {#section_t7i_4aa_lzh .section}

|参数|类型|是否必需|描述|
|--|--|----|--|
|Spas-AccessKey|String|是|在 ACM 控制台上的命名空间详情对话框内可获取 AccessKey。|
|timeStamp|String|是|以毫秒为单位的请求时间。|
|Spas-Signature|String|是|使用 SecretKey 对“Tenant+Group+TimeStamp”签名（`SpasSigner.sign(tenant+ group+ timeStamp, secretKey)`），签名算法为 HmacSHA1。TimeStamp 签名的作用是防止重放攻击。该签名有效期为 60 秒。|
|Spas-SecurityToken|String|否|SecurityToken 需从 STS 临时凭证中获取。STS 临时凭证需从实例元数据 URL 中获取。详情请参考： -   [借助于实例 RAM 角色访问其他云产品](https://www.alibabacloud.com/help/doc-detail/54579.htm)
-   [通过 ECS 实例 RAM 角色访问 ACM](https://www.alibabacloud.com/help/doc-detail/72013.htm)

 |

## 返回参数 {#section_n3m_ycv_42b .section}

|参数|类型|描述|
|--|--|--|
|configInfo|String|已变更配置的 Data ID、Group、命名空间信息，格式为 `dataId^2group^2tenant^1` -   配置的多个字段间分隔符：`(Java)^2 = Character.toString((char) 2)` 或 `(Shell) ^2=%02`。
-   配置间分隔符：`(Java) ^1 = Character.toString((char) 1)` 或 `(Shell) ^1=%01`

 |

## 错误码 {#section_qxw_hpr_lgb .section}

|错误码|错误信息|描述|
|---|----|--|
|400|Bad Request|客户端请求中的语法错误|
|403|Forbidden|没有权限|
|404|Not Found|客户端错误，未找到。|
|500|Internal Server Error|服务器内部错误|

## 代码示例 {#section_jyd_1dv_42b .section}

-   请求示例（Shell）

    ``` {#codeblock_5uz_5pz_9wh .lanuage-shell}
    #!/bin/bash
    ## config param
    dataId="com.alibaba.nacos.example.properties"
    group="DEFAULT_GROUP"
    namespace="04754ad1-4f67-4d67-b2bf-1f73a04a****"
    accessKey="8c5cbb849ae04682ad9f455a96aa****"
    secretKey="lwO5T7vfPJu27FclPa+/CyIG****"
    endpoint="acm.aliyun.com"
    ## config param end
    ## get serverIp from address server
    serverIp=`curl $endpoint:8080/diamond-server/diamond -s | awk '{a[NR]=$0}END{srand();i=int(rand()*NR+1);print a[i]}'`
    ## config sign
    timestamp=`echo $[$(date +%s%N)/1000000]`
    signStr=$namespace+$group+$timestamp
    signContent=`echo -n $signStr | openssl dgst -hmac $secretKey -sha1 -binary | base64`
    
    echo "Listening..."
    curl -X POST -H "Spas-AccessKey:"$accessKey -H "timeStamp:"$timestamp -H "Spas-Signature:"$signContent  -H "longPullingTimeout:30000" "http://"$serverIp":8080/diamond-server/config.co" -d "Probe-Modify-Request=com.alibaba.nacos.example.properties%02DEFAULT_GROUP%02f4da32aa4289aa861974f949b639****%0204754ad1-4f67-4d67-b2bf-1f73a04a****%01" -v
    ```

-   返回示例（Shell）

    ``` {#codeblock_hjh_ch1_9ug .lanuage-shell}
    # 如果配置有变更
    com.alibaba.nacos.example.properties%02DEFAULT_GROUP%02f4da32aa4289aa861974f949b639****%0204754ad1-4f67-4d67-b2bf-1f73a04a****%01
    
    # 如果配置无变更，则返回空串
    ```


## 更多信息 {#section_2i3_9xv_38y .section}

[API 概览](intl.zh-CN/API 参考/API 概览.md#)

