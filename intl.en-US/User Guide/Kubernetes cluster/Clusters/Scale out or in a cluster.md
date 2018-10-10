# Scale out or in a cluster {#task_glw_vmg_vdb .task}

In the Container Service console, you can scale out or scale in the worker nodes of a Kubernetes cluster according to your actual business requirements.

-   Currently, Container Service does not support scaling in and out the master nodes in a cluster.
-   Container Service only supports scaling in the worker nodes that are created when you create the cluster or added after you scale out the cluster. The worker nodes that are added as existing [Add an existing ECS instance](intl.en-US/User Guide/Kubernetes cluster/Clusters/Add an existing ECS instance.md#) when you create the cluster cannot be scaled in.
-   When you scale in a cluster, the worker nodes are removed from the cluster in the order that they are added after you scale out the cluster.
-   You must have more than 1 node that is not manually added to perform scaling in.

1.  Log on to the [Container Service console](https://cs.console.aliyun.com). 
2.  Under Kubernetes, click **Clusters** in the left-side navigation pane. 
3.  Click **Scale Cluster** at the right of the cluster. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6885/15391364024327_en-US.png)

4.  Select **Scale out** or **Scale in** in the Scale field and then configure the number of worker nodes. 

    In this example, scale out the cluster to change the number of worker nodes from one to four.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6885/15391364024328_en-US.png)

5.  Enter the logon password of the node. 

    **Note:** Make sure this password is the same as the one you entered when creating the cluster because you have to log on to the Elastic Compute Service \(ECS\) instance to copy the configuration information in the upgrade process.

6.  Click **Submit**. 

After scaling is complete, go to the Kubernetes Clusters Node List page to view that the number of worker nodes changes from one to four.

