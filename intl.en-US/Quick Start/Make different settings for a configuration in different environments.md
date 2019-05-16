# Make different settings for a configuration in different environments {#concept_t2p_g3t_42b .concept}

This topic explains how to set different values for the same configuration in testing, staging, and development environment with ACM's namespaces.

## Background information {#section_g4q_h3t_42b .section}

In this task, we will use ACM's namespaces to set different values for the same configuration in testing, staging, and development environment. The expected result is as follows:

 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15950/155799896947438_en-US.png)

## Step 1: Create a namespace in ACM {#section_xvl_k3t_42b .section}

The following is an example of creating the namespace “Development”.

1.  Log on to the [ACM console](https://acm.console.alibabacloud.com/).
2.  In the left-side navigation pane, select **Namespaces**, and click the **Create Namespace** button in the upper-right corner: The Create Namespace dialog box is displayed.
3.  In the dialog box, enter the namespace name Development.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15950/155799896947440_en-US.png)
4.  Repeat Steps 1 through 3 to create namespaces “Testing” and “Staging”.

## Step 2: Create a configuration under each namespace {#section_k3h_p3t_42b .section}

1.  On the Configurations page, select the namespace **Development**.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15950/155799896947441_en-US.png)
2.  Follow the instructions of [Step 1: Create the configuration in ACM](intl.en-US/Quick Start/Create and dynamically adjust configuration values.md#section_ljb_bgt_42b) to create configurations with the same name.

## Conclusion {#section_umg_r3t_42b .section}

In real-world business scenarios, we often need to set different values for one configuration item based on different environments. As you can see in this example, you can easily do so with the Namespace feature of ACM.

