# Create WordPress with an orchestration template {#task_b1g_k5t_4y .task}

If you have not created a cluster before, create a cluster first. For information about how to create a cluster, see [Create a cluster](../../../../reseller.en-US/User Guide/Swarm cluster/Clusters/Create a cluster.md#).

1.   Log on to the [Container Service console](https://partners-intl.console.aliyun.com/#/cs). 
2.   Click **Applications** in the left-side navigation pane and then click **Create Application** in the upper-right corner. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964301833_en-US.png)

3.   Complete the following configurations for the application you are about to create and then click **Create with Orchestration Template**. 

    -   **Name**: Enter the name of the application. Enter wordpress-test in this example.
    -   **Version**: Enter the version of the application. The default version is 1.0.
    -   **Cluster**: Select the cluster on which the application is to be deployed.
    -   **Update**: The update method of the application. Select **Standard Release** or **Blue-Green Release**. For more information, see [Introductions on release strategies](https://help.aliyun.com/document_detail/56874.html).
    -   **Description**: Enter the information of the application. The entered description will be displayed on the Application List page.
    -   **Pull Docker Image**: With this check box selected, Container Service pulls the latest Docker image from the repository to deploy the application, even when the image tag does not change.

        To improve efficiency, Container Service caches the image. When deploying an application, Container Service uses the cached image instead of pulling the image from the repository if the image tag is the same as that of the local cache. Therefore, if you modify your codes and image but do not modify the image tag for the convenience of upper business, Container Service will use the old image cached locally to deploy the application. With this check box selected, Container Service ignores the cached image and re-pulls the image from the repository when deploying the application to make sure the latest image and codes are always used.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964301834_en-US.png)

4.   Click **Use Existing Orchestration Template**, and click **Select** next to the wordpress template. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964311836_en-US.png)

5.   Modify the corresponding configurations 

    by making modifications in the template directly or clicking Edit to edit the services.

    `aliyun.routing.port_80: http://wordpress` indicates that requests from `http://wordpress.$testDomain` are forwarded to port 80 on the container after the container has run successfully.

6.   Click **Create and Deploy**. 
7.   A page indicating the application is created appears. Click **View Application List** or **Back to Application List** on the page or click **Applications** in the left-side navigation pane. Click the application name **wordpress-test** to view the application details. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964311839_en-US.png)

8.   Click the service name **web** under Services to view the service details. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964311841_en-US.png)

9.   Click the access endpoint, the domain name for access, of the service web.  

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6869/15395964311843_en-US.png)

    **Note:** 

    -   The domain name in this example is only used for test. Bind your own domain name.
    -   If you cannot access the endpoint, see [How to troubleshoot access link issues?](../../../../reseller.en-US/FAQ/Swarm FAQs/How to troubleshoot access link issues?.md#) to troubleshoot the issue.

