# CLI 参考 {#concept_pdp_rz5_42b .concept}

借助 ACM 提供的 CLI 命令行工具，您可以通过命令获取、推送、导入和导出配置。

## 安装 CLI 工具 {#section_lv4_sz5_42b .section}

请按照以下步骤安装 ACM Python SDK，CLI 工具将随之安装。

-   对于 Python 2.7 及以上版本，运行以下安装命令：

    **说明：** 运行 `python -V` 可查看 Python 版本。

    ``` {#codeblock_god_dzk_o4q}
    pip install acm-sdk-python
    ```

-   对于 Python 2.6，运行以下安装命令：

    ``` {#codeblock_4gx_wu6_xie}
    # 安装setuptools
    wget https://pypi.io/packages/source/s/setuptools/setuptools-33.1.1.zip
    unzip setuptools-33.1.1.zip
    cd setuptools-33.1.1 && sudo python setup.py install
    
    # 如已安装setuptools则直接运行以下命令
    sudo easy_install acm-sdk-python
    ```


**说明：** 使用 `-h` 参数可获得关于指定命令的使用帮助。例如，运行 `acm -h` 可获得关于 CLI 工具全部命令的使用帮助。运行 `acm add -h` 可获得关于 acm add 命令的使用帮助。

## 管理类命令 {#section_afp_tz5_42b .section}

管理类命令用于管理 。

|命令|作用|示例|主要参数|是否必需|
|--|--|--|----|----|
|`acm add`|添加命名空间并为其设置别名，方便使用别名快速切换命名空间。对于已经添加过的命名空间，此命令的作用是更新该命名空间。 **说明：** 此命令不会创建新的命名空间。

 |`acm add acm.aliyun.com:ea6135**-****-****-*********** -a Glff****ao -s '654b*****n87sa=' -n foo`|endpoint:namespace\_id：endpoint 与命名空间的 ID|namespace\_id 是必需的。若 endpoint 缺失则使用默认值 acm.aliyun.com。|
|-a：命名空间的 AccessKey|若指定了 -role（RAM 角色名称）则非必需|
|-s：命名空间的 SecretKey|若指定了 -role（RAM 角色名称）则非必需|
|-n：命名空间的别名 **说明：** 别名中不可使用冒号（:）

 |否|
|-role：阿里云 RAM 角色名称|否|
|`acm use`|切换当前命名空间。可使用 endpoint:namespace\_id 或别名来指定命名空间。|`acm use acm.aliyun.com:ea6135**-****-****-***********` 或 `acm use foo`|endpoint:namespace\_id：endpoint 与命名空间的 ID|若指定了别名则非必需。若不指定别名，则namespace\_id 是必需的，若 endpoint 缺失则使用默认值 acm.aliyun.com。|
|别名|若指定了 endpoint:namespace\_id 则非必需|
|`acm current`|列出当前命名空间。|`acm current`|N/A|N/A|
|`acm show`|列出所有命名空间，即通过 `acm add` 命令添加的所有命名空间。|`acm show`|N/A|N/A|

## 数据操作类命令 {#section_sgk_2nx_33b .section}

数据操作类命令用于操作命名空间中的配置。

**说明：** 以下命令默认作用于当前命名空间，也可以使用 -n 参数传入 endpoint:namespace\_id 或别名来指定要操作的命名空间。

|命令|作用|示例|主要参数|是否必需|
|--|--|--|----|----|
|`acm pull`|获取一个配置并将其内容打印到 Console。|`acm pull group/dataId > dest.txt`|group/data\_id|data\_id（ ）是必需的。若配置属于默认 ，则 group 是可选的。|
|`acm push`|推送一个配置。|`cat source.txt | acm push group/dataId`|group/data\_id|data\_id 是必需的。若配置属于默认 Group，则 group 是可选的。|
|标准输入流中的内容（可使用管道命令）或用 -f 参数指定的输入文件|至少需要提供这两种输入中的一种。若同时提供，则用 -f 参数指定的输入文件优先级高。|
|`acm export`|将命名空间下的所有配置导出到本地。|`acm export -d ./myConfigs`|-f：要导出的 .zip 压缩文件名称。|否。若不指定，则使用默认值 `<endpoint>-<namespace_id>.zip`。|
|-d：将配置导出至该目录，目录结构为 `group/data_id`（默认 Group 中的配置存放于根目录）。|否。若指定，则忽略 -f 参数指定的压缩文件名称。|
|`acm import`|将本地配置文件导入到命名空间。|`acm export -d ./myConfigs`|-f：要导入的 .zip 压缩文件名称。|否。若不指定，则使用默认值 `<endpoint>-<namespace_id>.zip`。|
|-d：从该目录导入配置，目录结构为 `group/data_id`（默认 Group 中的配置存放于根目录）。|否。若指定，则忽略 -f 参数指定的文件名称。|

