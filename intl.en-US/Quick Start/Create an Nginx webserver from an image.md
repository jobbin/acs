# Create an Nginx webserver from an image {#task_oz2_clt_41y .task}

If you have not created a cluster before, create a cluster first. For information about how to create a cluster, see [Create a cluster](../../../../reseller.en-US/User Guide/Swarm cluster/Clusters/Create a cluster.md#).

1.   Log on to the [Container Service console](https://partners-intl.console.aliyun.com/#/cs). 
2.   Click **Applications** in the left-side navigation pane, and click **Create Application** in the upper-right corner. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963971789_en-US.png)

3.   Complete the following configurations for the application you are about to create and then click **Create with Image**. 
    -   **Name**: Enter the name of the application. Enter nginx in this example.
    -   **Version**: Enter the version of the application. By default, 1.0 is entered.
    -   **Cluster**: Select the cluster on which the application is to be deployed.
    -   **Update**: The update method of the application. Select Standard Release or Blue-Green Release. For more information, see [Introductions on release strategies](../../../../reseller.en-US/User Guide/Swarm cluster/Release policy/Introductions on release strategies.md#).
    -   **Description**: Enter the information of the application. The entered description will be displayed on the Application List page.
    -   **Pull Docker Image**: With this check box selected, Container Service pulls the latest Docker image from the repository to deploy the application, even when the image tag does not change.

        To improve efficiency, Container Service caches the image. When deploying an application, Container Service uses the cached image instead of pulling the image from the repository if the image tag is the same as that of the local cache. Therefore, if you modify your codes and image but do not modify the image tag for the convenience of upper business, Container Service will use the old image cached locally to deploy the application. With this check box selected, Container Service ignores the cached image and re-pulls the image from the repository when deploying the application to make sure the latest image and codes are always used.

4.   Configure the port mapping of host and container in **Port Mapping**. To enable access to the Nginx server in the container by means of Internet, configure the **Web Routing**. 

    1.   Click the plus icon next to Port Mapping and add two port mappings by entering 80 and 443 in the Container Port field respectively. The host port is not specified in this example. 
    2.   Configure the Web Routing. 
        -   Click the plus icon next to **Web Routing**.
        -   Enter 80 in the **Container Port** field, indicating to access port 80 on the Nginx container.
        -   Enter nginx in the **Domain** field. Only the domain name prefix nginx is entered. If the domain name prefix is XXX, you get the domain name XXX.$cluster\_id.$region\_id.alicontainer.com for test. In this example, you get the test domain name nginx.c9b424ed591eb4892a2d18dd264a6fdfb.cn-hangzhou.alicontainer.com.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963971797_en-US.png)

    **Note:** You can also enter your own domain name. For how to add your own domain name, see [Simple routing - Configure domain names](../../../../reseller.en-US/User Guide/Swarm cluster/Service discovery and load balancing/Simple routing - Configure domain names.md#). For how to configure the container port used for routing and the domain name of HTTP services, see **routing** in [Label description](../../../../reseller.en-US/User Guide/Swarm cluster/Service orchestrations/Label description.md#). For information about how the routing service forwards requests to the container, see [Simple routing - supports HTTP and HTTPS](../../../../reseller.en-US/User Guide/Swarm cluster/Service discovery and load balancing/Simple routing - supports HTTP and HTTPS.md#).

5.   Click **Create**. Container Service creates the application nginx according to the preceding settings. 
6.   A page indicating the application is created appears. Click **View Application List** or **Back to Application List** on the page or click **Applications** in the left-side navigation pane. Click the application name **nginx** to view the application details.. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963971813_en-US.png)

7.   Click the service name **nginx** under Services to view the service details. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963981814_en-US.png)

8.   Click the access endpoint of the service nginx. The Nginx server default welcome page is displayed. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6868/15395963981815_en-US.png)

    **Note:** If you cannot access this page, see [How to troubleshoot access link issues?](../../../../reseller.en-US/FAQ/Swarm FAQs/How to troubleshoot access link issues?.md#).


