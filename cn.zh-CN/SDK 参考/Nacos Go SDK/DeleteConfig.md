# DeleteConfig {#concept_1681269 .concept}

使用 DeleteConfig 将配置从 Nacos 删除，以自动化手段降低运维成本。

## 描述 {#section_258_7z9_wgw .section}

使用以下接口将配置从 Nacos 删除。

``` {#codeblock_627_mdi_6t7}
DeleteConfig(param vo.ConfigParam) (bool, error)
```

## 请求参数 {#section_9pv_em1_ld9 .section}

|参数|参数类型|描述|
|--|----|--|
|ConfigParam.DataId|String|配置 ID，采用类似 `package.class`（如 `com.taobao.tc.refund.log.level`）的命名规则保证全局唯一性。建议根据配置的业务含义来定义 class 部分。全部字符均为小写。只允许使用英文字符和 4 种特殊字符（“.”、“:”、“-”、“\_”），不超过 256 字节。|
|ConfigParam.Group|String|配置分组，建议填写`产品名:模块名`（如 `ACM:Test`）来保证唯一性。只允许使用英文字符和 4 种特殊字符（“.”、“:”、“-”、“\_”），不超过 128 字节。|

## 返回参数 {#section_xll_2pq_7sa .section}

|参数类型|描述|
|----|--|
|Boolean|是否删除成功|

## 示例 {#section_zv0_2be_6gj .section}

`Go` 格式

**说明：** 请将代码中的 $\{endpoint\}、$\{namespace\}、$\{accessKey\}、$\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 **End Point**、**命名空间 ID**、**AccessKey**、**SecretKey**。出于安全考虑，建议使用 RAM 用户的 AccessKey 和 SecretKey。

``` {#codeblock_td2_k2b_2h4}
package main

import (
    "fmt"
    "github.com/nacos-group/nacos-sdk-go/clients"
    "github.com/nacos-group/nacos-sdk-go/common/constant"
    "github.com/nacos-group/nacos-sdk-go/vo"
    "time"
)

func main() {
    // 从控制台命名空间管理的"命名空间详情"中拷贝 End Point、命名空间 ID
    var endpoint = "${endpoint}"
    var namespaceId = "${namespaceId}"

    // 推荐使用 RAM 用户的 accessKey、secretKey
    var accessKey = "${accessKey}"
    var secretKey = "${secretKey}"


    clientConfig := constant.ClientConfig{
        //
        Endpoint:       endpoint + ":8080",
        NamespaceId:    namespaceId,
        AccessKey:      accessKey,
        SecretKey:      secretKey,
        TimeoutMs:      5 * 1000,
        ListenInterval: 30 * 1000,
    }

    configClient, err := clients.CreateConfigClient(map[string]interface{}{
        "clientConfig": clientConfig,
    })

    if err != nil {
        fmt.Println(err)
        return
    }

    var dataId = "com.alibaba.nacos.example.properties"
    var group = "DEFAULT_GROUP"

    // 删除配置
    success, err = configClient.DeleteConfig(vo.ConfigParam{
        DataId: dataId,
        Group:  group})

    if success {
        fmt.Println("Config deleted successfully.")
    }

}         
```

