# Create configuration {#task_e1w_fx3_n2b .task}

Some variables and parameters can be extracted from code to be managed in a configuration file. The configuration can be retrieved from the configuration file when the code is executed. By doing so, you can change parameters quickly and reduce the code maintenance costs. This topic explains how to create a configuration.

1.  Log on to the , and select a region as needed in the upper-left corner. 
2.  In the left-side navigation pane of the console, select **Configurations**, and then click the **+** icon on the right. 

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/acms/bt_create_configuration.png)

    The Create Configuration page is displayed.

    ![](http://aliware-images.oss-cn-hangzhou.aliyuncs.com/acms/pg_create_config.png)

3.  Enter the configuration information on the page according to the following table, and click **Publish**. 

    **Note:** In ACM, the data model for a configuration is &lt;Namespace+DataId+Group, Content \>. You don't need to apply for the DataId or Group, as long as they're unique in a [namespace](reseller.en-US/User Guide/Create a namespace.md#).

    |Field|Description|
    |-----|-----------|
    |**Data ID**|Configuration ID. Use a naming rule like *package*.**class** \(for example, *com.taobao.tc.refund.log*.**level**\) to ensure global uniqueness. It is recommended to indicate business meaning of the configuration in the “class” section. All characters must be in lowercase. Only English characters and four special characters \(".", ":", "-", and "\_"\) are allowed. It must not exceed 256 bytes.|
    |**Group**|Configuration group \(namespace\). We recommend that you use product name \(for example ACM\) or module name to guarantee the uniqueness. You can perform authentication by group. Only English characters and four special characters \(".", ":", "-", and "\_"\) are allowed. It must not exceed 128 bytes.|
    |**Tags**|You can use tags to manage configurations according to your own dimensions. Up to 5 tags are allowed, and the maximum length of each tag is 32 characters.|
    |**Description**|Configure description information to help others understand the meaning of the configuration. Up to 128 characters are allowed.|
    |**Application**|Name of the application associated with the configuration. Only English characters and four special characters \(".", ":", "-", "\_"\) are allowed.|
    |**Target Region**|Most users publish the configuration to the current region only.  To publish the configuration to multiple regions, select all target regions.|
    |**Data Encryption**|If the configuration contains sensitive data, we recommend that you use encrypted storage function to minimize the risk of data leakage. You must activate Key Management Service \(KMS\), and authorize ACM to encrypt and decrypt with your KMS before you can use this function, because ACM data encryption function uses KMS to encrypt configurations. Note that the Data ID of an encrypted configuration always begins with `cipher-` . For more information, see [Create and use encrypted configuration](https://help.aliyun.com/document_detail/69178.html) .|
    |**Format**|Select a format to have ACM validate the syntax and format for you.|
    |**Configuration Content**|The content of the configuration. A size of less than 10 KB is preferred, and do no exceed 100 KB at the maximum.|


**Related documents**  


[Manage namespace](reseller.en-US/User Guide/Create a namespace.md#)

[Create and use encrypted configuration](https://help.aliyun.com/document_detail/69178.html)

