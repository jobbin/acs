# Migrate a cluster {#task_kvz_5yy_lfb .task}

For a Swarm cluster created earlier, you can guarantee the performance and stability of the cluster by migrating the cluster.

-   The latest time for migrating a cluster is displayed through SMS, station message, or email. Complete the Swarm cluster migration before the latest time. The system automatically migrates the cluster if you do not migrate the cluster before the latest time.
-   Cluster migration rebuilds connections from cluster nodes to the container server without affecting applications deployed in the cluster, nor adding or modifying any data. Make sure that you perform this operation during the low peak period of your business because unpredictable risks might still exist throughout the migration process.

1.  Log on to the [Container Service console](https://cs.console.aliyun.com). 
2.  Under the Swarm menu, click **Clusters**. 
3.  Click **Cluster Migration** in the action column at the right of the cluster to be migrated.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23685/153975905613732_en-US.png)

 
4.  Click **OK** in the Prompt dialog box. 

    **Note:** During cluster migration:

    -   Information query, deployment, upgrade, and other operations cannot be performed in the console.
    -   The cluster cannot be connected to through the cluster access point API.
    -   The data and application status in the cluster remain unchanged. Applications deployed on the cluster are still accessible.
    -   The migration process takes about three minutes.
    On the Cluster List page, **Migrating** is displayed in the **Cluster Status** column.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23685/153975905613733_en-US.png)


After cluster migration is completed, on the Cluster List page, **Running** is displayed in the **Cluster Status** column.

**Note:** 

-   The cluster ID, access point address, and other attributes remain unchanged.
-   Please be sure to confirm that your business is running properly.
-   During the migration process, if you have any questions, please open a ticket in which you include the cluster ID and state whether your deployed applications are normal.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23685/153975905613734_en-US.png)

