# Update a node certificate {#task_ywz_drj_bfb .task}

You can update a node certificate of a Swarm cluster to avoid node certificate expiration.

1.  You have created a swarm cluster, see [Create a cluster](reseller.en-US/User Guide/Clusters/Create a cluster.md#).
2.  Updating a node certificate reboots the node Docker Daemon. Make sure that containers on the node are all configured to restart automatically.

    **Note:** You can configure a container restart policy when creating an application. When you create an application by using an image, select the **Always** check box for **Restart**. When you create an application by using a template, configure a container restart policy in the template `restart: always`.

3.  If a node certificate expires within 60 days, a prompt is displayed. You must timely update the node certificate.

Each cluster node has a certificate used to access system control services. Each issued certificate has a valid period. When the valid period of a certificate is about to expire, you must manually renew the certificate. Otherwise, the service of the node is affected.

1.  Log on to the [Container Service console](https://partners-intl.console.aliyun.com/#/cs). 
2.  Under the Swarm menu, click **Nodes** in the left-side navigation pane. The certificate expiration information of each cluster node is displayed. 

    **Note:** The certificate expiration time is displayed in the status column only if the node certificate expires within 60 days.

3.  Select a node in the node list, and click **More** \> **Update Certificate** on the right to reissue the node certificate. 

    **Note:** We recommend that you upgrade the cluster agent to the latest version before updating the node certificate.

4.  If the system prompts you to upgrade the cluster agent after you click **Update Certificate**, the current cluster agent does not support this feature. You need to upgrade the cluster agent to the new version first, see [Upgrade Agent](reseller.en-US/User Guide/Clusters/Upgrade Agent.md#). If no prompt is displayed, go to the next step. 
5.  If no prompt is displayed or the cluster agent is updated, click **Update Certificate**. Confirm updating information and then update the node cluster certificate. 

    **Note:** 

    -   When the node certificate update is completed, the Docker Daemon node is automatically restarted about 1 minute later.
    -   To guarantee that containers on the node can automatically restart, make sure that an automatic restart policy is configured.
6.  After the cluster node certificate is updated, the node certificate information is no longer displayed. 

