# getAllConfigByTenant {#concept_abb_3dv_42b .concept}

使用 getAllConfigByTenant 接口获取指定命名空间内的 ACM 配置信息。

## 请求类型 {#section_a2g_5cv_42b .section}

GET

## 请求 URL {#section_b2g_5cv_42b .section}

/diamond-server/basestone.do?method=getAllConfigByTenant

## 请求参数 {#section_f2y_wcv_42b .section}

|参数|类型|是否必需|描述|
|--|--|----|--|
|tenant|String|是|租户信息，对应 ACM 的命名空间 ID。|
|pageNo|Integer|是|分页页号|
|pageSize|Integer|是|分页大小|

## Header 参数 {#section_m34_xcv_42b .section}

|参数|类型|是否必需|描述|
|--|--|----|--|
|Spas-AccessKey|String|是|在 ACM 控制台上的命名空间详情对话框内可获取 AccessKey。|
|timeStamp|String|是|以毫秒为单位的请求时间。|
|Spas-Signature|String|是|使用 SecretKey 对“Tenant+Group+TimeStamp”签名（`SpasSigner.sign(tenant+ group+ timeStamp, secretKey)`），签名算法为 HmacSHA1。TimeStamp 签名的作用是防止重放攻击。该签名有效期为 60 秒。|
|Spas-SecurityToken|String|否|SecurityToken 需从 STS 临时凭证中获取。STS 临时凭证需从实例元数据 URL 中获取。详情请参考： -   [借助于实例 RAM 角色访问其他云产品](https://help.aliyun.com/document_detail/54579.html)
-   [通过 ECS 实例 RAM 角色访问 ACM](https://help.aliyun.com/document_detail/72013.html)

 |

## 返回参数 {#section_n3m_ycv_42b .section}

|参数|类型|描述|
|--|--|--|
|totalCount|Integer|总配置数|
|pageNumber|Integer|分页页号|
|pagesAvailable|Integer|可用分页数|
|pageItems|Array|配置信息|
|└appName|String|归属应用的名称|
|└dataId|String|配置的 ID|
|└group|String|配置的分组|

## 错误码 {#section_qxw_hpr_lgb .section}

|错误码|错误信息|描述|
|---|----|--|
|400|Bad Request|客户端请求中的语法错误|
|403|Forbidden|没有权限|
|404|Not Found|客户端错误，未找到。|
|500|Internal Server Error|服务器内部错误|

## 代码示例 {#section_jyd_1dv_42b .section}

-   请求示例（Shell）

    ``` {#codeblock_9q3_13m_p3i .lanuage-shell}
    #!/bin/bash
    ## config param
    namespace="04754ad1-4f67-4d67-b2bf-1f73a04a****"
    accessKey="8c5cbb849ae04682ad9f455a96aa****"
    secretKey="lwO5T7vfPJu27FclPa+/CyIG****"
    endpoint="acm.aliyun.com"
    ## config param end
    ## get serverIp from address server
    serverIp=`curl $endpoint:8080/diamond-server/diamond -s | awk '{a[NR]=$0}END{srand();i=int(rand()*NR+1);print a[i]}'`
    ## config sign
    timestamp=`echo $[$(date +%s%N)/1000000]`
    signStr=$namespace+$timestamp
    signContent=`echo -n $signStr | openssl dgst -hmac $secretKey -sha1 -binary | base64`
    ## request to get configs in a namespace
    curl -H "Spas-AccessKey:"$accessKey -H "timeStamp:"$timestamp -H "Spas-Signature:"$signContent "http://"$serverIp":8080/diamond-server/basestone.do?method=getAllConfigByTenant&tenant="$namespace"&pageNo="1"&pageSize="10 -v
    ```

-   返回示例（JSON）

    ``` {#codeblock_lua_qjf_6f6 .language-json}
    {
       "totalCount":4,
       "pageNumber":1,
       "pagesAvailable":1,
       "pageItems":[
          {
             "appName":"",
             "dataId":"com.alibaba.nacos.example01.properties",
             "group":"DEFAULT_GROUP"
          },
          {
             "appName":"",
             "dataId":"com.alibaba.nacos.example02.properties",
             "group":"DEFAULT_GROUP"
          },
          {
             "appName":"",
             "dataId":"com.alibaba.nacos.example03.properties",
             "group":"DEFAULT_GROUP"
          },
          {
             "appName":"",
             "dataId":"com.alibaba.nacos.example04.properties",
             "group":"DEFAULT_GROUP"
          }
       ]
    }
    ```


## 更多信息 {#section_tdj_nqu_dih .section}

[API 概览](cn.zh-CN/API 参考/API 概览.md#)

