# API 概览 {#concept_wr5_t1v_42b .concept}

本文介绍 ACM API 的概要信息，包括 API 列表、获取服务器 IP 的方法、通信协议、请求方法、公共参数、签名算法等。

## 配置管理 API {#section_3kn_bjn_ywl .section}

|API|描述|
|---|--|
|[getConfig](intl.zh-CN/API 参考/getConfig.md#)|获取 ACM 配置内容。|
|[getAllConfigByTenant](intl.zh-CN/API 参考/getAllConfigByTenant.md#)|获取指定命名空间内的 ACM 配置信息。|
|[addListener](intl.zh-CN/API 参考/addListener.md#)|监听 ACM 配置的变更。|
|[syncUpdateAll](intl.zh-CN/API 参考/syncUpdateAll.md#)|发布 ACM 配置。|
|[deleteAllDatums](intl.zh-CN/API 参考/deleteAllDatums.md#)|删除 ACM 配置。|

## 获取服务器 IP 列表 {#section_v4z_qbv_42b .section}

通过地址服务器获取服务 IP 列表，以便通过服务 IP 发起请求。

``` {#codeblock_t89_hki_w0w}
http://${地址服务器域名}:8080/diamond-server/diamond
```

|地域|地址服务器域名|
|--|-------|
|公网|acm.aliyun.com|
|华东 1（杭州）|addr-hz-internal.edas.aliyun.com|
|华北 1（青岛）|addr-qd-internal.edas.aliyun.com|
|华东 2（上海）|addr-sh-internal.edas.aliyun.com|
|华北 2（北京）|addr-bj-internal.edas.aliyun.com|
|华南 1（深圳）|addr-sz-internal.edas.aliyun.com|
|中国（香港）|addr-hk-internal.edas.aliyuncs.com|
|新加坡（新加坡）|addr-singapore-internal.edas.aliyun.com|
|澳大利亚（悉尼）|addr-ap-southeast-2-internal.edas.aliyun.com|
|美国（硅谷）|addr-us-west-1-internal.acm.aliyun.com|
|美国（弗吉尼亚）|addr-us-east-1-internal.acm.aliyun.com|
|华东 2（上海）金融云|addr-cn-shanghai-finance-1-internal.edas.aliyun.com|

代码示例：

``` {#codeblock_254_owm_wee}
curl http://acm.aliyun.com:8080/diamond-server/diamond

# 返回
139.196.135.144
```

## 通信协议 {#section_hbf_vbv_42b .section}

支持通过 HTTP 进行请求通信。

## 请求方法 {#section_ibf_vbv_42b .section}

支持 HTTP GET 或 POST 方法发送请求，GET 方式下请求参数需要包含在请求的 URL 中。

## 请求参数 {#section_jbf_vbv_42b .section}

每个请求都需要包含公共的鉴权、签名相关请求参数和相关操作所特有的请求参数。

## 公共参数 {#section_rzk_ave_r9k .section}

调用 ACM API 时都会用到的 Header 参数如下表所示。

|参数|类型|是否必需|描述|
|--|--|----|--|
|Spas-AccessKey|String|是|在 ACM 控制台上的命名空间详情对话框内可获取 AccessKey。|
|timeStamp|String|是|以毫秒为单位的请求时间。|
|Spas-Signature|String|是|使用 SecretKey 对“Tenant+Group+TimeStamp”签名（`SpasSigner.sign(tenant+ group+ timeStamp, secretKey)`），签名算法为 HmacSHA1。TimeStamp 签名的作用是防止重放攻击。该签名有效期为 60 秒。|
|Spas-SecurityToken|String|否|SecurityToken 需从 STS 临时凭证中获取。STS 临时凭证需从实例元数据 URL 中获取。详情请参考： -   [借助于实例 RAM 角色访问其他云产品](https://www.alibabacloud.com/help/doc-detail/54579.htm)
-   [通过 ECS 实例 RAM 角色访问 ACM](https://www.alibabacloud.com/help/doc-detail/72013.htm)

 |
|longPullingTimeout|String|是|长轮询等待 30 秒，此处填写 30000。|

## 字符编码 {#section_kbf_vbv_42b .section}

请求及返回结果都使用 GBK 字符集进行编码。

## 签名机制 {#section_etm_zbv_42b .section}

ACM 服务会对每个访问的请求进行身份验证，使用 HTTP 需要在请求中包含签名（Signature）信息。ACM 通过使用 AccessKey 和 SecretKey 进行对称加密的方法来验证请求的发送者身份。

AccessKey 和 SecretKey 由 ACM 颁发给访问者。其中 AccessKey 用于标识访问者的身份，SecretKey 是用于加密签名字符串和服务器端验证签名字符串的密钥，出于安全考虑，请务必严格保密。

## 签名算法 {#section_ftm_zbv_42b .section}

签名采用 HmacSHA1 算法。

-   Java 签名算法参考

    ``` {#codeblock_5sb_w5q_jaz}
    public static void main(String[] args) throws Exception {
        String tenant= "tenant";
        String group = "group";
        String timeStamp = String.valueOf(System.currentTimeMillis());
        String abc = HmacSHA1Encrypt(tenant+ "+" + group + "+" + timeStamp , "1234");
        System.out.println(abc);
    }
    public static String HmacSHA1Encrypt(String encryptText, String encryptKey) throws Exception {
        byte[] data = encryptKey.getBytes("UTF-8");
        // 根据给定的字节数组构造一个密钥,第二参数指定一个密钥算法的名称
        SecretKey secretKey = new SecretKeySpec(data, "HmacSHA1");
        // 生成一个指定 Mac 算法 的 Mac 对象
        Mac mac = Mac.getInstance("HmacSHA1");
        // 用给定密钥初始化 Mac 对象
        mac.init(secretKey);
        byte[] text = encryptText.getBytes("UTF-8");
        byte[] textFinal = mac.doFinal(text);
        // 完成 Mac 操作, base64编码，将byte数组转换为字符串
        return new String(Base64.encodeBase64(textFinal));
    }
    ```

-   Shell 签名算法

    ``` {#codeblock_0gj_hyh_nwj}
    ## config sign
    timestamp=`echo $[$(date +%s%N)/1000000]`
    signStr=$namespace+$group+$timestamp
    signContent=`echo -n $signStr | openssl dgst -hmac $sk -sha1 -binary | base64`
    echo $signContent
    ```


## 签名处理步骤 {#section_aky_bcv_42b .section}

1.  使用请求参数构造规范的请求字符串（QueryParam）。
2.  使用上一步构造的规范字符串，按照以下规则构造用于计算签名的字符串。

    ``` {#codeblock_bvh_zuc_56i}
    Signature=HMAC-SHA1(QueryParam)
    ```

    **说明：** 对于不同的请求，QueryParam 会不同。

3.  按照 RFC2104 的定义，使用上一步构造的用于签名的字符串来计算签名 HMAC 值。

    **说明：** 计算签名时使用的 Key 就是您持有的 AccessKeySecret（ASCII:38），使用的哈希算法是 SHA1。

4.  按照 Base64 编码规则将上一步算出的 HMAC 值编码成字符串，即得到签名值（Signature）。
5.  将上一步得到的签名值作为 Signature 参数添加到请求参数中，即完成对请求的签名处理。

## 示例代码 {#section_zcq_kcv_42b .section}

以下示例代码展示了如何以 Shell 构造 ACM 请求。

``` {#codeblock_kor_7zd_t5c .lanuage-shell}
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
## request to get a config
curl -H "Spas-AccessKey:"$accessKey -H "timeStamp:"$timestamp -H "Spas-Signature:"$signContent "http://"$serverIp":8080/diamond-server/config.co?dataId="$dataId"&group="$group"&tenant="$namespace -v
```

