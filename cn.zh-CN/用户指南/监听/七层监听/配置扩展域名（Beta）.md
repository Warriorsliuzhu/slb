# 配置扩展域名（Beta） {#concept_blk_jy1_wdb .concept}

性能保障型负载均衡HTTPS监听支持挂载多个证书，将来自不同访问域名的请求转发至不同的后端服务器组。

## 扩展域名介绍 {#section_kfl_lsc_wdb .section}

服务器名称指示（Server Name Indication, SNI）是对SSL / TLS协议的扩展，允许在单个IP地址上承载多个SSL证书。性能保障型负载均衡支持SNI。当客户端访问负载均衡时，默认使用访问域名配置的证书解密。如果找不到匹配的证书，则使用监听配置的证书。

当您有将多个域名绑定到同一个负载均衡服务地址上， 然后通过不同的域名区分不同的访问来源并且使用HTTPS加密访问的需求时，可以通过配置扩展域名实现。

扩展域名功能已在各地域发布。

**说明：** 金融云暂不支持扩展域名功能。

## 步骤一 创建HTTPS监听 {#section_wvf_3nb_wdb .section}

1.  登录[负载均衡管理控制台](https://slbnew.console.aliyun.com)。
2.  在实例管理页面，选择一个地域，然后单击**创建负载均衡**。

    配置负载均衡实例，详情参见[创建负载均衡实例](cn.zh-CN/用户指南/负载均衡实例/创建负载均衡实例.md#)。

    **说明：** 只有性能保障实例支持配置扩展域名。

3.  返回实例管理页面，单击已创建的实例ID。
4.  在左侧导航栏，单击**监听**，然后单击**添加监听**。
5.  配置监听。

    本操作的主要配置如下，其他配置参考[配置七层监听](cn.zh-CN/用户指南/监听/七层监听/配置七层监听.md#)。

    -   **双向认证**：关闭
    -   **服务器证书**：上传一个名称为example的服务器证书。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13400/2848_zh-CN.png)


## 步骤二 添加扩展域名 {#section_ykj_pvb_wdb .section}

1.  在监听页面，找到已创建的HTTPS监听，然后单击**更多** \> **扩展域名管理**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13400/2851_zh-CN.png)

2.  单击**添加扩展域名**，配置扩展域名：
    -   **域名**：输入域名。

        支持精确匹配和通配符匹配两种模式：

        -   精确域名：www.aliyun.com

        -   通配符域名（泛域名）: \*.aliyun.com, \*.market.aliyun.com

            当前端请求同时匹配多条域名规则时，规则的匹配优先级为：精确匹配 \> 小范围通配符匹配 \> 大范围通配符匹配，如下表所示（√代表匹配，—代表不匹配）。

            |模式|请求URL|域名规则|
            |www.aliyun.com|\*.aliyun.com|\*.market.aliyun.com|
            |精确匹配|www.aliyun.com|√|—|—|
            |泛域名匹配|market.aliyun.com|—|√|—|
            |泛域名匹配|info.market.aliyun.com|—|—|√|

        -   **证书**：选择该域名使用的证书。

            如果您没有上传好的证书，单击**上传证书**或单击**购买证书**通过阿里云购买证书。

            **说明：** 证书中的域名和您添加的扩展域名必须一致。

3.  重复步骤2，添加更多扩展域名。

    在**扩展域名管理**页面，无法删除的域名是监听的域名和证书。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13400/2868_zh-CN.png)


## 步骤三 添加转发规则 {#section_fns_2qc_wdb .section}

1.  在监听页面，找到已创建的HTTPS监听，然后单击**更多** \> **添加转发策略**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13400/2851_zh-CN.png)

2.  在转发策略页面，单击**添加转发策略**。
3.  配置转发策略，详情参见[添加域名和路径转发规则](cn.zh-CN/用户指南/监听/七层监听/添加域名和路径转发规则.md#)。

    **说明：** 确保转发策略中配置的域名和您添加的扩展域名一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13400/2912_zh-CN.png)


