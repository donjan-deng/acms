# 使用定制版 confd 以无侵入方式使用 ACM 配置 {#concept_1079408 .concept}

confd 是一款开源轻量配置管理工具，通过将存储于 etcd、Dynamodb、Redis、Vault、Zookeeper 等后端存储系统的数据与配置模板结合起来使用，可实现保持配置处于最新状态的目的。confd 支持的后端存储系统不包括 ACM，因此本文以支持 ACM 的定制版 confd 为例，介绍如何使用 confd 以无侵入的方式使用 ACM 配置。

## 教程概述 {#section_bmw_ncz_33b .section}

以 API/SDK 方式使用 ACM 配置的方式是一种侵入式的方式，因为需要改造您的业务代码。与此相对，使用 confd 则可在不改造业务代码的情况下使用 ACM 配置。本教程以一个示例说明如何安装 confd，如何搭配使用 confd 和 ACM 来管理配置，以及如何使用 confd 的监听功能。

## 准备工作 {#section_qbn_81q_l6b .section}

-   [在本地安装 Go](https://golang.org/doc/install)：confd 的构建依赖于 Go 1.10 或更高版本。

-   登录 [ACM 控制台](https://acm.console.alibabacloud.com/)，并创建一个示例配置。

    -   Data ID：myapp.database.url
    -   Group：不填写，即使用默认 Group。
    -   配置内容：jdbc:mysql://localhost:3306/dbName

## 以源码方式安装定制版 confd {#section_y4c_bcy_33b .section}

请按照以下步骤安装定制版 confd。

1.  创建 confd 目录，并将 confd 源码下载至该目录。

    ``` {#codeblock_7ny_984_mdy}
    mkdir -p $GOPATH/src/github.com/kelseyhightower
    cd $GOPATH/src/github.com/kelseyhightower
    wget https://github.com/nacos-group/nacos-confd/archive/v0.19.0.tar.gz
    ```

2.  解压下载的 confd 源码。

    ``` {#codeblock_n9j_yv4_3u3}
    tar -xvf v0.19.0.tar.gz
    ```

3.  将 nacos-confd-0.19.0 移动至 confd 目录，并在该目录中通过编译命令生成可执行文件。

    ``` {#codeblock_cvv_a8d_ng5}
    mv nacos-confd-0.19.0 confd
    cd confd
    make
    ```

4.  将生成的可执行文件拷贝至 /usr/local/bin 目录并执行该文件。如果能执行则说明 confd 安装成功。

    ``` {#codeblock_nul_eyb_mhm}
    sudo cp bin/confd /usr/local/bin
    confd
    ```


## 搭配使用定制版 confd 和 ACM 管理配置 {#section_kwm_hny_33b .section}

请按照以下步骤来搭配使用 confd 和 ACM。

1.  创建 confd 所需的目录用来存放模板资源配置文件和模板文件。

    ``` {#codeblock_vjg_9k8_3fg}
    mkdir -p /etc/confd/{conf.d,templates}
    ```

2.  在 conf.d 目录下创建 TOML 格式的 confd 模板资源配置文件。

    ``` {#codeblock_moy_ami_r17}
    vim /etc/confd/conf.d/myconfig.toml
    ```

    在该资源配置文件中添加以下内容。

    -   src 参数指定 confd 模板文件
    -   dest 参数指定生成的配置文件
    -   keys 参数指定将模板渲染成配置文件所需的配置内容
    ``` {#codeblock_jx6_8cs_q4z}
    [template]
    src = "myconfig.conf.tmpl"
    dest = "/tmp/myconfig.conf"
    keys = [
    "/myapp/database/url",
    ]
    ```

3.  在 templates 目录下创建 confd 模板文件。

    ``` {#codeblock_x9g_fwy_6k5}
    vim /etc/confd/templates/myconfig.conf.tmpl
    ```

    在该模板文件中添加以下内容。其含义为通过 getv 从 ACM 获取 Data ID 为 `myapp.database.url` 的配置内容。

    **说明：** 在模板中必须将 ACM 的 Data ID 由 `myapp.database.url` 转换为`/myapp/database/url` 格式，即以斜线（/）代替句点（.）并在开头增加一个斜线。

    ``` {#codeblock_g3l_pwb_nbp}
    database.url = {{getv "/myapp/database/url"}}
    ```

4.  启动 confd。

    **说明：** 请将代码中的 \{endpoint\}、\{namespace\}、\{accessKey\}、\{secretKey\} 分别替换为 ACM 控制台上命名空间详情对话框内的 End Point、命名空间 ID、AccessKey、SecretKey。

    ``` {#codeblock_c27_8en_v0w}
    confd -backend nacos -endpoint {endpoint}:8080 -namespace {namespace} -accessKey {accessKey} -secretKey {secretKey}
    ```


启动 confd 后，如果在 /tmp 目录下生成了包含以下内容的 myconfig.conf 文件，则说明 confd 已成功获取 ACM 配置内容。

``` {#screen_08s_afr_ovt .screen}
database.url = jdbc:mysql://localhost:3306/dbName
```

## 开启 confd 监听 {#section_g3h_42z_33b .section}

如果按照上述方法操作，confd 生成一次配置文件后就会退出。只要在 confd 启动命令中添加 -watch 参数即可监听后端系统（在本示例中为 ACM）的配置变更，一旦配置内容发生变化，confd 就会重新生成配置文件。

1.  以监听模式启动 confd。

    ``` {#codeblock_t1e_3ro_88f}
    confd -watch -backend nacos -endpoint {endpoint}:8080 -namespace {namespace} -accessKey {accessKey} -secretKey {secretKey}
    ```

2.  登录 ACM 控制台，将示例配置 myapp.database.url 更改为以下内容并发布配置。

    jdbc:mysql://localhost:3306/dbName2


如果在 /tmp 目录下生成了包含以下内容的 myconfig.conf 文件，则说明执行成功。

``` {#screen_46k_des_o3y .screen}
database.url = jdbc:mysql://localhost:3306/dbName2
```

## 更多信息 {#section_hwy_vgz_33b .section}

关于 ACM 定制版 confd 的详细信息参见 [https://github.com/nacos-group/nacos-confd](https://github.com/nacos-group/nacos-confd)。

