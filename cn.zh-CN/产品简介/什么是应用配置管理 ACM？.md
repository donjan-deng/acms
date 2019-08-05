# 什么是应用配置管理 ACM？ {#concept_y4m_zgv_m2b .concept}

应用配置管理 ACM 是一款在分布式架构环境中对应用配置进行集中管理和推送的产品。利用 ACM，您可以在微服务、DevOps、大数据等场景下极大减轻配置管理的工作量，并增强配置管理的服务能力。

应用配置管理 ACM（Application Configuration Management）前身为淘宝内部配置中心 Diamond，现已作为 [Nacos](https://nacos.io/) 的配置中心模块开源。

在应用生命周期管理中，开发人员通常会将应用中需要变更的一些配置项或者元数据从代码中分离出来，放在单独的配置文件中管理，这些单独管理的内容就称为应用配置。这种分离应用配置的方法是管理应用变更的常见手段之一。发布应用后，运维人员或最终用户可以通过调整配置来适配环境，或调整应用程序的运行行为。

ACM 是面向分布式系统的配置中心。凭借配置变更、配置推送、历史版本管理、灰度发布、配置变更审计等配置管理工具，ACM 能帮助您集中管理所有应用环境中的配置，降低分布式系统中管理配置的成本，并降低因错误的配置变更带来可用性下降甚至发生故障的风险。

## 传统架构中的配置管理 {#section_y4k_bhv_m2b .section}

在传统架构中，如果配置信息有变更，通常需要登录服务器并手动修改配置来使配置生效，如下图。

![Diagram Traditional](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15941/15649985477242_zh-CN.png)

## ACM 配置管理 {#section_apk_bhv_m2b .section}

在 ACM 的配置管理场景下，您只需要在 ACM 控制台上更改配置，配置信息就会自动被推送到各个服务器中，并在秒级延迟内生效。完整的 ACM 产品包括三个主要部分：客户端、服务端和用于配置管理的控制台。

![Diagram ACM](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15941/15649985477243_zh-CN.png)

## ACM 价值 {#section_cpk_bhv_m2b .section}

通过 ACM 管理配置可以为 IT 运维带来以下益处：

-   更新的配置秒级自动下发到各机器，降低配置手动分发的工作量；
-   通过接入 ACM 配置监听接口，各应用端的配置可立即生效，无需重启应用；
-   所有配置监听、更改和版本信息自动记录在案，增强了审计、版本管理、诊断等方面的能力。

## ACM 与 Nacos 的关系 {#section_4fp_9p1_mts .section}

阿里巴巴中间件于 2018 年开源了 [Nacos](https://nacos.io) 产品，致力于打造一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。目前，Nacos 主要包含两大功能：

-   分布式配置中心：该功能对应应用配置管理 ACM。您可以使用 [Nacos SDK](../../../../intl.zh-CN/SDK 参考/Nacos SDK/Nacos Client.md#) 直接访问 ACM 服务。
-   服务注册与发现：该功能对应企业级分布式应用服务 EDAS 中的应用名字服务 ANS（注册中心）。

与 Nacos 分布式配置中心相比，ACM 具备以下优势：

-   高可用与高性能：服务端高可用性和多级缓存与客户端容灾，确保即使服务中断也不影响业务。
-   敏感配置的安全保障：使用[加解密配置](../../../../intl.zh-CN/用户指南/创建和使用加密配置.md#)功能后，配置只会在内存中解密成明文，最大限度保证了配置安全，大幅降低了敏感配置的泄露风险。
-   全面的权限管理：支持[细粒度权限控制](../../../../intl.zh-CN/访问控制/访问权限控制.md#)，支持 [通过 ECS 实例 RAM 角色访问 ACM](../../../../intl.zh-CN/访问控制/通过 ECS 实例 RAM 角色访问 ACM.md#)。
-   更多实用特性：[查询推送轨迹](../../../../intl.zh-CN/用户指南/查询推送轨迹.md#)、[多语言支持](../../../../intl.zh-CN/SDK 参考/SDK 简介.md#)等。

## 学习路径图 {#section_hgv_k8k_mja .section}

您可以借助 [ACM 产品学习路径图](https://www.alibabacloud.com/getting-started/learningpath/acm)来快速了解如何使用 ACM 的配置管理基础功能，和一键回滚、推送轨迹、命名空间、权限控制等高级功能，以及如何使用丰富的 API 和 SDK 来满足您的特定需求。

