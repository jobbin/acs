# 通过镜像创建 Nginx {#task_oz2_clt_41y .task}

如果您还未创建集群，您需要先创建集群。有关如何创建集群的详细信息，参见[创建集群](../../../../intl.zh-CN/用户指南/Swarm 集群/集群管理/创建集群.md#)。

1.   登录[容器服务管理控制台](https://cs.console.aliyun.com)。 
2.   单击左侧导航栏中的**应用** 并单击右上角的**创建应用**，如下图所示。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963851789_zh-CN.png)

3.   输入应用相关信息，单击**使用镜像创建**。 
    -   **应用名称**：要创建的应用的名称。本示例中，应用名称为 nginx。
    -   **应用版本**：所创建应用的版本。默认为 1.0。
    -   **部署集群**：要部署到的集群。
    -   **默认更新策略**：应用更新的方式，您可以选择标准发布 或蓝绿发布，参见 [发布策略说明](../../../../intl.zh-CN/用户指南/Swarm 集群/发布策略/发布策略说明.md#)。
    -   **应用描述**：应用的相关信息。该信息将显示在应用列表页面。
    -   **检查最新 Docker 镜像**：选中该选项后，表示当镜像 Tag 不变的情况下，也会去仓库拉取最新的镜像。

        为了提高效率，容器服务会对镜像进行缓存。部署时，如果发现镜像 Tag 与本地缓存的一致，则会直接复用而不重新拉取。所以，如果您基于上层业务便利性等因素考虑，在做代码和镜像变更时没有同步修改 Tag ，就会导致部署时还是使用本地缓存内旧版本镜像。而勾选该选项后，会忽略缓存，每次部署时重新拉取镜像，确保使用的始终是最新的镜像和代码。

4.   单击**选择镜像**。在常用镜像里，查找nginx镜像，单击**确定**。 

    容器服务会默认使用镜像的最新版本。如果您需要使用镜像的其它版本，单击**选择镜像版本**，单击所需版本并单击**确定**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963851810_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963856071_zh-CN.png)

5.   在**端口映射**中配置容器与主机的端口映射。为了能够通过公网访问容器内的 Nginx 服务器，我们还需要配置**简单路由配置**。 

    1.   配置容器的 80 和 443 的端口映射。本示例未指定主机端口。 
    2.   配置简单路由。 
        -   单击**简单路由配置**右侧的加号图标。
        -   在**容器端口**框中输入80，即表示访问 nginx 容器的 80 端口。
        -   在**域名**框中输入nginx。域名字段只填写了域名前缀 nginx，如果域名前缀为 XXX，会给到域名 XXX.$cluster\_id.$region\_id.alicontainer.com 供测试使用。本例中您获得的测试域名为 nginx.c9b424ed591eb4892a2d18dd264a6fdfb.cn-hangzhou.alicontainer.com。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963851797_zh-CN.png)

    **说明：** 您也可以填写自己的域名，关于如何添加您的自有域名，参见 [简单路由-域名配置](../../../../intl.zh-CN/用户指南/Swarm 集群/服务发现和负载均衡/简单路由-域名配置.md#)。关于配置路由的容器端口和 HTTP 服务的域名，参见 [标签概览](../../../../intl.zh-CN/用户指南/Swarm 集群/服务编排/标签概览.md#) 中的 **routing**。关于路由服务如何将请求转发到容器，参见 [简单路由（支持 HTTP/HTTPS\)](../../../../intl.zh-CN/用户指南/Swarm 集群/服务发现和负载均衡/简单路由（支持 HTTP__HTTPS).md#)。

6.   单击**创建**。容器服务根据以上设置创建应用 nginx。 
7.   单击**查看应用列表**，**返回应用列表** 或左侧导航栏中的 **应用** 返回应用列表。如下图所示，单击应用名称**nginx**，查看应用详情。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963851813_zh-CN.png)

8.   在服务列表中，单击服务名称**nginx**，进入服务 nginx 页面。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963851814_zh-CN.png)

9.   单击服务 nginx 的访问端点，即可进入 Nginx 服务器的默认欢迎页面。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963851815_zh-CN.png)

    **说明：** 如果您无法正常访问该页面，可以参考 [访问链路问题排查](../../../../intl.zh-CN/常见问题/Swarm FAQs/访问链路问题排查.md#)进行问题排查。


