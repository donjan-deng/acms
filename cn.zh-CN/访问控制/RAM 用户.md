# RAM 用户 {#concept_gyw_dwt_42b .concept}

ACM 支持阿里云访问控制 RAM 的账户体系。借助 RAM 用户，云账户（主账户）可以避免与其他用户共享账户密钥，并按需为 RAM 用户分配最小权限，实现各司其职的高效管理。

## 云账户（主账户, Account） {#section_eyr_tcd_bhb .section}

云账户是阿里云资源归属、资源使用计量计费的基本主体。当用户开始使用阿里云服务时，首先需要注册一个云账户。云账户为其名下所拥有的资源付费，并对其名下所有资源拥有完全控制权限。

默认情况下，资源只能被属主（ResourceOwner）所访问，任何其他用户访问都需要获得属主的显式授权。所以从权限管理的角度来看，云账户就是操作系统的 root 或 Administrator，所以我们有时称它为根账户或主账户。

## RAM 用户\(RAM User\) {#section_xn1_vcd_bhb .section}

RAM 允许在一个云账户下创建多个 RAM 用户（可以对应企业内的员工、系统或应用程序）。RAM 用户不拥有资源，没有独立的计量计费，这些用户由所属云账户统一控制和付费。RAM 用户是归属于云账户，只能在所属云账户的空间下可见，而不是独立的云账户。RAM 用户必须在获得云账户的授权后才能登录控制台或使用 API 操作云账户下的资源。

RAM 用户有两种身份类型：

-   **RAM-User**：是一种实体身份类型，有确定的身份 ID 和身份凭证，它通常与某个确定的人或应用程序一一对应。
-   **RAM-Role**：是一种虚拟身份类型，它没有确定的身份凭证，它必须关联到某个实体身份上才能使用。

## 关于 RAM 旧版和新版控制台 {#section_qzj_ty3_bhb .section}

RAM 控制台分为旧版和新版。切换版本的方法为：

-   在旧版控制台上，单击任意页面下方的**体验新版**，即可切换为新版控制台。
-   在新版控制台上，单击概览页面右上角的**切换旧版**，即可切换为旧版控制台。

## （旧版）创建 RAM 用户 {#sc_create_ram_user_old .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**用户管理**。
2.  在页面右上角单击**新建用户**。在创建用户对话框中输入用户名，按需输入其他可选信息，并单击**确定**。新创建的用户显示在用户管理页面上。

    **说明：** 用户名必须在云账户内保持唯一。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_db_create_user.png "创建用户对话框")

3.  在用户管理页面上单击新创建用户的**用户名/显示名**。
4.  在用户详情页面的 **Web 控制台登录管理**区域，单击**启用控制台登录**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_bt_enable_console_logon.png)

5.  在设置密码对话框中输入**新密码**和**确认密码**，选择是否**要求该账号下次登录成功后重置密码**，并单击**确定**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_db_enable_console_logon.png)

6.  如果**手机验证**对话框弹出，则单击**点击获取**，然后输入手机上收到的验证码。

完成以上步骤后，一个可以登录控制台的 RAM 用户就创建成功了。

## （新版）创建 RAM 用户 {#sc_create_ram_user .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**人员管理** \> **用户**。
2.  在用户页面上单击**新建用户**，在**用户账号信息**区域输入用户的**登陆名称**和**显示名称**。

    **说明：** 登陆名称必须在云账户内保持唯一。

    -   如需创建多个用户，则单击**添加用户**，并输入**登陆名称**和**显示名称**。
    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_create_user.png "新建用户页面")

3.  在**访问方式**区域选择**控制台密码登陆**，然后按需设置**控制台密码**、**要求重置密码**和**多因素认证**，并单击**确定**。

完成以上步骤后，一个可以登录控制台的 RAM 用户就创建成功了。

## （旧版）为 RAM 用户授权 {#sc_auth_ram_user_old .section}

RAM 授权的粒度是 ACM 服务级别，即 RAM 授权表示允许用户拥有 ACM 的所有权限。RAM 授权或者解除授权只能在 RAM 控制台上操作。

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**用户管理**。
2.  在用户管理页面上单击目标用户**操作**列中的**授权**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_users.png)

3.  在编辑个人授权策略对话框左侧的**可选授权策略名称**中找到 AliyunACMFullAccess 策略，并按中间的 **\>** 图标将其添加到**已选授权策略名称**中，然后单击**确定**。
    -   如果还使用到 ACM 的[加解密配置](intl.zh-CN/用户指南/创建和使用加密配置.md#)功能，则还需要为用户添加 **AliyunKMSCryptoAccess** 授权策略。

## （新版）为 RAM 用户授权 {#sc_auth_ram_user .section}

RAM 授权的粒度是 ACM 服务级别，即 RAM 授权表示允许用户拥有 ACM 的所有权限。RAM 授权或者解除授权只能在 RAM 控制台上操作。

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**人员管理** \> **用户**。
2.  在用户页面上单击目标用户**操作**列中的**授权**。

    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_db_add_permission_for_user.png)

3.  在添加权限对话框左侧的**系统权限策略**中找到 AliyunACMFullAccess 策略，并单击该策略，然后单击**确定**。
    -   如果还使用到 ACM 的[加解密配置](intl.zh-CN/用户指南/创建和使用加密配置.md#)功能，则还需要为用户添加 **AliyunKMSCryptoAccess** 授权策略。

## （旧版）为 RAM 用户解除授权 {#sc_deauth_ram_user_old .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**用户管理**。
2.  在用户管理页面上单击目标用户**操作**列中的**授权**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_users.png)

3.  在编辑个人授权策略对话框中，选择**已选授权策略名称**中的 **AliyunACMFullAccess**，并按中间的 **<** 图标将其移至左边的**可选授权策略名称**，然后单击**确定**。

授权解除后，RAM 用户无权登录 ACM。

## （新版）为 RAM 用户解除授权 {#sc_deauth_ram_user .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**人员管理** \> **用户**。
2.  在用户管理页面上单击目标用户的**用户登陆名称/显示名称**，然后单击**权限管理**页签。
3.  在**个人权限**子页签的表格中，单击**操作**列中的**移除权限**。
4.  在移除权限对话框中单击**确认**。

授权解除后，RAM 用户无权登录 ACM。

## （旧版）删除 RAM 用户 {#sc_delete_ram_user_old .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/#/overview)，在左侧导航栏中选择**用户管理**。
2.  在用户管理页面上单击目标用户**操作**列中的**删除**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_users.png)

3.  在删除用户对话框中单击**确定**。

## （新版）删除 RAM 用户 {#sc_delete_ram_user .section}

1.  登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**人员管理** \> **用户**。
2.  在用户页面上单击目标用户**操作**列中的**删除**。
3.  在删除用户对话框中单击**确认**。

## （旧版）使用 RAM 用户登录 ACM 控制台 {#sc_log_on_ram_user_old .section}

1.  使用云账户登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**概览**。
2.  单击 **RAM 用户登录链接**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_sc_ram_user_login.png)

3.  在阿里云-RAM用户登录页面上按照提示输入登录用户名称，单击**下一步**，然后输入密码并单击**登录**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_ram_user_login.png)

4.  在子用户用户中心页面上，单击互联网中间件类目下的**应用配置管理**进入 ACM 控制台。

## （新版）使用 RAM 用户登录 ACM 控制台 {#sc_log_on_ram_user .section}

1.  使用云账户登录 [RAM 控制台](https://ram.console.aliyun.com/)，在左侧导航栏中选择**概览**。
2.  在**账号管理**区域单击 **用户登录地址**链接。

    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_sc_ram_user_login_new.png)

3.  在阿里云-RAM用户登录页面上按照提示输入登录用户名称，单击**下一步**，然后输入密码并单击**登录**。

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/common/ram_pg_ram_user_login.png)

4.  在子用户用户中心页面上，单击互联网中间件类目下的**应用配置管理**进入 ACM 控制台。

## 参考 {#section_pmb_mj2_bhb .section}

-   [RAM 角色](intl.zh-CN/访问控制/RAM 角色.md#)
-   [ZH-CN\_TP\_15960.md\#](intl.zh-CN/访问控制/访问权限控制.md#)
-   [相关术语](../../intl.zh-CN/产品简介/相关术语.md#)

