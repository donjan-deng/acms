# Nacos Go SDK 概述 {#concept_gjn_pww_42b .concept}

您可以在 Go 程序中使用 Nacos Go SDK 管理 Nacos 配置，包括获取、监听、发布和删除配置。

## 准备工作 {#section_qbn_81q_l6b .section}

-   [在本地安装 Go](https://golang.org/doc/install)
-   [获取 Nacos Go SDK](github.com/nacos-group/nacos-sdk-go/clients)

## 公共参数 {#section_75x_a3o_apk .section}

|参数|参数类型|描述|
|--|----|--|
|ConfigParam.DataId|String|配置 ID，采用类似 `package.class`（如 `com.taobao.tc.refund.log.level`）的命名规则保证全局唯一性。建议根据配置的业务含义来定义 class 部分。全部字符均为小写。只允许使用英文字符和 4 种特殊字符（“.”、“:”、“-”、“\_”），不超过 256 字节。|
|ConfigParam.Group|String|配置分组，建议填写`产品名:模块名`（如 `ACM:Test`）来保证唯一性。只允许使用英文字符和 4 种特殊字符（“.”、“:”、“-”、“\_”），不超过 128 字节。|

## 示例代码 {#section_lkw_qww_42b .section}

`Go` 格式

**说明：** 请将代码中的 $\{endpoint\}、$\{namespace\}、$\{accessKey\}、$\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 **End Point**、**命名空间 ID**、**AccessKey**、**SecretKey**。出于安全考虑，建议使用 RAM 用户的 AccessKey 和 SecretKey。

``` {#codeblock_yym_nmu_q5n}
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

    // 发布配置
    success, err := configClient.PublishConfig(vo.ConfigParam{
        DataId:  dataId,
        Group:   group,
        Content: "connectTimeoutInMills=3000"})

    if success {
        fmt.Println("Publish config successfully.")
    }

    time.Sleep(3 * time.Second)

    // 获取配置
    content, err := configClient.GetConfig(vo.ConfigParam{
        DataId: dataId,
        Group:  group})

    fmt.Println("Get config：" + content)

    // 监听配置
    configClient.ListenConfig(vo.ConfigParam{
        DataId: dataId,
        Group:  group,
        OnChange: func(namespace, group, dataId, data string) {
            fmt.Println("ListenConfig group:" + group + ", dataId:" + dataId + ", data:" + data)
        },
    })

    // 删除配置
    success, err = configClient.DeleteConfig(vo.ConfigParam{
        DataId: dataId,
        Group:  group})

    if success {
        fmt.Println("Delete config successfully.")
    }

}           
```

## 更多信息 {#section_q1f_c2c_p2b .section}

-   [GetConfig](intl.zh-CN/SDK 参考/Nacos Go SDK/GetConfig.md#)
-   [ListenConfig](intl.zh-CN/SDK 参考/Nacos Go SDK/ListenConfig.md#)
-   [PublishConfig](intl.zh-CN/SDK 参考/Nacos Go SDK/PublishConfig.md#)
-   [DeleteConfig](intl.zh-CN/SDK 参考/Nacos Go SDK/DeleteConfig.md#)
-   [Nacos Go SDK](https://github.com/nacos-group/nacos-sdk-go)

