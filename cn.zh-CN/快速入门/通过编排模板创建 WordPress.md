# 通过编排模板创建 WordPress {#task_b1g_k5t_4y .task}

如果您还未创建集群，您需要先创建集群。有关如何创建集群的详细信息，参见[创建集群](../../../../intl.zh-CN/用户指南/Swarm 集群/集群管理/创建集群.md#)。

1.   登录[容器服务管理控制台](https://cs.console.aliyun.com)。 
2.   在Swarm菜单下，单击左侧导航栏中的**应用**并单击右上角的**创建应用**，如下图所示。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964231833_zh-CN.png)

3.   输入应用相关信息，单击**使用编排模板创建**。 

    -   **应用名称**：要创建的应用的名称。本示例中，应用名称为 wordpress-test。
    -   **应用版本**：所创建应用的版本。默认为 1.0。
    -   **部署集群**：要部署到的集群。
    -   **默认更新策略**：应用更新的方式，您可以选择**标准发布** 或**蓝绿发布** ，参见 [发布策略说明](https://help.aliyun.com/document_detail/56874.html)。
    -   **应用描述**：应用的相关信息。该信息将显示在应用列表页面。
    -   **检查最新 Docker 镜像**：选中该选项后，表示当镜像 Tag 不变的情况下，也会去仓库拉取最新的镜像。

        为了提高效率，容器服务会对镜像进行缓存。部署时，如果发现镜像 Tag 与本地缓存的一致，则会直接复用而不重新拉取。所以，如果您基于上层业务便利性等因素考虑，在做代码和镜像变更时没有同步修改 Tag ，就会导致部署时还是使用本地缓存内旧版本镜像。而勾选该选项后，会忽略缓存，每次部署时重新拉取镜像，确保使用的始终是最新的镜像和代码。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964231834_zh-CN.png)

4.   单击**使用已有编排模板**， 单击 WordPress 模板对应的**选择**按钮。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964231836_zh-CN.png)

5.   在模板编辑框，修改相应的配置。 

    您可以在模板中进行修改，或选择要修改的服务，然后单击编辑修改配置。

    `aliyun.routing.port_80: http://wordpress` 代表该容器运行成功后，来自 `http://wordpress.$testDomain` 的请求会转发到容器的 80 端口。

6.   单击**创建并部署**。 
7.   单击**查看应用列表**，**返回应用列表** 或左侧导航栏中的 **应用**。如下图所示，单击应用名称 **wordpress-test**，查看应用详情。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964231839_zh-CN.png)

8.   单击服务列表中的服务名称**web**，查看服务的详细信息。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964231841_zh-CN.png)

9.   单击服务 web 的访问端点，即访问的域名。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964231843_zh-CN.png)

    **说明：** 

    -   该域名仅供测试使用。请绑定您自己的域名。
    -   如果您无法正常访问，可以参考[访问链路问题排查](../../../../intl.zh-CN/常见问题/Swarm FAQs/访问链路问题排查.md#) 进行问题排查。

