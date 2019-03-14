# 利用 RAM 角色实现跨账号访问 ACM {#concept_emy_vgb_ygb .concept}

使用企业 A 的阿里云主账号创建 RAM 角色、为该角色授权，并将该角色赋予企业 B，即可实现使用企业 B 的主账号或其 RAM 子账号访问您的 ACM 资源的目的。

## 关于 RAM 旧版和新版控制台 {#section_vys_xml_bhb .section}

RAM 控制台分为旧版和新版。切换版本的方法为：

-   在旧版控制台上，单击任意页面下方的**体验新版**，即可切换为新版控制台。
-   在新版控制台上，单击概览页面右上角的**切换旧版**，即可切换为旧版控制台。

## 跨账号授权流程 {#section_mxq_xgb_ygb .section}

假设企业 A（账号 ID 为 11223344，企业别名为 company-a）需要将 ACM 操作权限授予企业 B（账号 ID 为 12345678，企业别名为 company-b）的员工 C，则授权流程为：

**RAM 旧版控制台**

1.  [（旧版）步骤一：企业 A 创建角色](#step_1_old)
2.  [（旧版）步骤 二：企业 A 为该角色授权](#step_2_old)
3.  [（旧版）步骤 三：企业 B 创建 RAM 用户](#step_3_old)
4.  [（旧版）步骤四：企业 B 为 RAM 用户授权](#step_4_old)

**RAM 新版控制台**

1.  [（新版）步骤一：企业 A 创建角色](#step_1)
2.  [（新版）步骤 二：企业 A 为该角色授权](#step_2)
3.  [（新版）步骤 三：企业 B 创建 RAM 用户](#step_3)
4.  [（新版）步骤四：企业 B 为 RAM 用户授权](#step_4)

**说明：** 关于企业别名的详细信息，请参考[RAM 初始设置](../../../../../../intl.zh-CN/快速入门/RAM 初始设置.md#)的[设置账号别名](../../../../../../intl.zh-CN/快速入门/RAM 初始设置.md#section_fg4_jjj_pfb)部分。

## （旧版）步骤一：企业 A 创建角色 {#step_1_old .section}

1.  使用企业 A 的云账户登录 [RAM 控制台](https://ram.console.aliyun.com/#/overview)，在左侧导航栏中选择**角色管理**。
2.  在页面右上角单击**新建角色**。在创建角色对话框的**选择角色类型**子页面上选择**用户角色**。
3.  在**填写类型信息**子页面上，在**选择云账号**区域选择**其他云账号**，然后在**受信云账号ID**文本框中输入需授权的云帐户。
    -   在本示例中，输入企业 B 的账号 ID 12345678。
4.  在**配置角色基本信息**子页面上输入**角色名称**，然后单击**创建**。
    -   在本示例中，输入 acm-admin。
5.  如果**手机验证**对话框弹出，则单击**点击获取**，然后输入手机上收到的验证码。

## （旧版）步骤 二：企业 A 为该角色授权 {#step_2_old .section}

新创建的角色没有任何权限，因此企业 A 必须为该角色授权。在本示例中，企业 A 要将授权策略 AliyunACMFullAccess 分配给该角色，从而使该角色能够访问 ACM 资源。

1.  在创建角色对话框的**创建成功**子页面单击**授权**。如果此前已关闭创建角色对话框，则在角色管理页面单击新创建角色的名称，然后在左侧导航栏中选择**角色授权策略**。

    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/acms/db_ram_create_role_tab_create_success.png "创建角色对话框")

2.  在角色授权策略页面右上角单击**编辑授权策略**。
3.  在编辑角色授权策略对话框左侧的**可选授权策略名称**中找到 AliyunACMFullAccess 策略，并按中间的 **\>** 图标将其添加到**已选授权策略名称**中，然后单击**确定**。
4.  如果**手机验证**对话框弹出，则单击**点击获取**，然后输入手机上收到的验证码。

**说明：** 此步骤将授予 ACM 的全部访问权限。如果希望授予单个命名空间的特定权限，请参考[ZH-CN\_TP\_15960.md\#](intl.zh-CN/访问控制/访问权限控制.md#)

## （旧版）步骤 三：企业 B 创建 RAM 用户 {#step_3_old .section}

1.  使用企业 B 的主账号登录 [RAM 控制台](https://ram.console.aliyun.com/#/overview)，在左侧导航栏中选择**用户管理**。
2.  在页面右上角单击**新建用户**。在创建用户对话框中输入用户名，按需输入其他可选信息，并单击**确定**。新创建的用户显示在用户管理页面上。

    **说明：** 用户名必须在云账户内保持唯一。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_db_create_user.png "创建用户对话框")

3.  在用户管理页面上单击新创建用户的**用户名/显示名**。
4.  在用户详情页面的 **Web 控制台登录管理**区域，单击**启用控制台登录**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_bt_enable_console_logon.png)

5.  在设置密码对话框中输入**新密码**和**确认密码**，选择是否**要求该账号下次登录成功后重置密码**，并单击**确定**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_db_enable_console_logon.png)


## （旧版）步骤四：企业 B 为 RAM 用户授权 {#step_4_old .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**用户管理**。
2.  在用户管理页面上单击目标用户**操作**列中的**授权**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_users.png)

3.  在编辑个人授权策略对话框左侧的**可选授权策略名称**中找到 AliyunSTSAssumeRoleAccess 策略，并按中间的 **\>** 图标将其添加到**已选授权策略名称**中，然后单击**确定**。

## （新版）步骤一：企业 A 创建角色 {#step_1 .section}

1.  使用企业 A 的云账户登录 [RAM 控制台](https://ram.console.aliyun.com/#/overview)，在左侧导航栏中选择 **RAM 角色管理**。
2.  在 RAM 角色管理页面上单击**新建 RAM 角色**。
3.  在**新建 RAM 角色**对话框中的执行以下操作并单击**确定**。
    1.  在 **RAM 角色类型**区域选择**用户 RAM 角色**。
    2.  在**选择云账号**区域选择**其他云账号**，并在文本框内输入需授权的云帐户。
        -   在本示例中，输入企业 B 的账号 ID 12345678。
    3.  在 **RAM 角色名称**文本框内输入 RAM 角色名称。
        -   在本示例中，输入 acm-admin。

## （新版）步骤 二：企业 A 为该角色授权 {#step_2 .section}

新创建的角色没有任何权限，因此企业 A 必须为该角色授权。在本示例中，企业 A 要将授权策略 AliyunACMFullAccess 分配给该角色，从而使该角色能够访问 ACM 资源。

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择 **RAM 角色管理**。
2.  在 RAM 角色管理页面上单击目标角色**操作**列中的**添加权限**。

    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_db_add_permission_for_user.png)

3.  在添加权限对话框左侧的**系统权限策略**中找到 AliyunACMFullAccess 策略，并单击该策略，然后单击**确定**。
    -   如果还使用到 ACM 的[加解密配置](intl.zh-CN/用户指南/创建和使用加密配置.md#)功能，则还需要为用户添加 **AliyunKMSCryptoAccess** 授权策略。

**说明：** 此步骤将授予 ACM 的全部访问权限。如果希望授予单个命名空间的特定权限，请参考[ZH-CN\_TP\_15960.md\#](intl.zh-CN/访问控制/访问权限控制.md#)。

## （新版）步骤 三：企业 B 创建 RAM 用户 {#step_3 .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**人员管理** \> **用户**。
2.  在用户页面上单击**新建用户**，在**用户账号信息**区域输入用户的**登陆名称**和**显示名称**。

    **说明：** 登陆名称必须在云账户内保持唯一。

    -   如需创建多个用户，则单击**添加用户**，并输入**登陆名称**和**显示名称**。
    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_create_user.png "新建用户页面")

3.  在**访问方式**区域选择**控制台密码登陆**，然后按需设置**控制台密码**、**要求重置密码**和**多因素认证**，并单击**确定**。

完成以上步骤后，一个可以登录控制台的 RAM 用户就创建成功了。

## （新版）步骤四：企业 B 为 RAM 用户授权 {#step_4 .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**人员管理** \> **用户**。
2.  在用户页面上单击目标用户**操作**列中的**授权**。
3.  在添加权限对话框左侧的**系统权限策略**中找到 AliyunSTSAssumeRoleAccess 策略，并单击该策略，然后单击**确定**。

## 步骤五：使用企业 B 的 RAM 用户跨账号访问资源 {#step_5 .section}

1.  [使用企业 B 的 RAM 用户登录 ACM 控制台。](intl.zh-CN/访问控制/RAM 用户.md#sc_log_on_ram_user_old)
2.  登录成功后，将鼠标指针移到右上角头像，并在浮层中单击**切换身份**。
3.  在阿里云－角色切换页面，输入企业 A 的企业别名 company-a（或默认域名） 和角色名 acm-admin，然后单击**切换**。
4.  对企业 A 的 ACM 资源执行操作。

## 参考 {#section_qtm_1nl_bhb .section}

-   [ZH-CN\_TP\_15959.md\#](intl.zh-CN/访问控制/RAM 用户.md#)
-   [ZH-CN\_TP\_136604.md\#](intl.zh-CN/访问控制/RAM 角色.md#)
-   [RAM 初始设置](../../../../../../intl.zh-CN/快速入门/RAM 初始设置.md#)

