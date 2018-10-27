# Add an existing ECS instance {#concept_vtv_1ll_xdb .concept}

You can add a purchased Elastic Compute Service \(ECS\) instance to a specified cluster.

**Note:** At most 20 ECS instances can be added to a cluster by default.  To add more ECS instances, [open a ticket](https://selfservice.console.aliyun.com/ticket/scene?productCode=cs&productName=%E5%AE%B9%E5%99%A8%E6%9C%8D%E5%8A%A1).

You can add an existing ECS instance in the following ways:

-   **Add ECS instances automatically:** The image and system disk of the ECS instance are reset by using this method. You can add one or more ECS instances to the cluster at a time.
-   **Add the ECS instance manually:** Manually add the ECS instance by running scripts on the ECS instance. You can only add one ECS instance to the cluster at a time.

## Prerequisites {#section_vpf_cll_xdb .section}

If you have not created a cluster before, create a cluster first. For information about how to create a cluster, see [Create a cluster](reseller.en-US/User Guide/Clusters/Create a cluster.md#).

## Instructions {#section_srk_ykf_zdb .section}

-   The ECS instance to be added must be in the same region and use the same network type \(Virtual Private Cloud \(VPC\)\) as the cluster.
-   When adding an existing ECS instance, make sure that your ECS instance has an Elastic IP \(EIP\) for the network type VPC, or the corresponding VPC has configured the NAT gateway. In short, make sure the corresponding node can access public network normally. Otherwise, the ECS instance fails to be added.
-   The ECS instance to be added must be under the same account as the cluster.
-   If you select to **manually add** the ECS instance, note that:
    -   If you have already installed Docker on your ECS instance, the ECS instance may fail to be added. We recommend that you uninstall Docker and remove the Docker folders before adding the ECS instance by running the following command:

        **Ubuntu:** `apt-get remove -y docker-engine`, `rm -fr /etc/docker/ /var/lib/docker /etc/default/docker`

        **CentOS:**`yum remove -y docker-engine`, `rm -fr /etc/docker /var/lib/docker`


## Procedure {#section_qd1_blf_zdb .section}

1.  Log on to the [Container Service console](https://partners-intl.console.aliyun.com/#/cs).
2.  Click Swarm \> **Clusters** in the left-side navigation pane.
3.  Click **More** at the right of the cluster that you want to add ECS instances and then select **Add Existing Instances** from the drop-down list.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344782_en-US.png)

4.  Add ECS instances.

    The ECS instances displayed are filtered and synchronized from your ECS instance list according to the region and network type defined by the cluster.

    Add the ECS instances in the following ways:

    -   Add ECS instances automatically.

        **Note:** As this method will reset the image and system disk of the ECS instance, proceed with caution. Create a snapshot to back up your data before adding the ECS instance. For information about how to create a snapshot, see [Create snapshots](../../../../reseller.en-US/User Guide/Snapshots/Create snapshots.md#).

        1.  Select the ECS instances you want to add to the cluster and click **Next Step**.

            You can add one or more ECS instances at a time.

        2.  Configure the instance information. Click **Next Step** and then click **Confirm** in the confirmation dialog box.
        3.  Click **Finish**.
    -   Manually add the ECS instance by running scripts on the ECS instance.

        1.  Select **Manually Add**. Select an ECS instance, and then click **Next Step**.

            You can only add one ECS instance at a time.

        2.  Confirm the instance information and click **Next Step**.
        3.  The scripts unique to this ECS instance are displayed. Click **log on to the ECS instance xxxxxx**.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344783_en-US.png)

        4.  The VNC connection password is displayed in the dialog box. Copy the password and click **Close**.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344784_en-US.png)

        5.  In the dialog box, enter the VNC connection password and click **OK**.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344785_en-US.png)

        6.  Enter the logon account \(root\) and password of the ECS instance, and press Enter to log on to the ECS instance.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344786_en-US.png)

        7.  Click **Input Commands**. Paste the preceding scripts into the dialog box, click **OK** and press Enter.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344787_en-US.png)

            The system runs the scripts. Wait until the scripts are successfully run. A success message is displayed. The ECS instance is successfully added.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344788_en-US.png)


## Related operation {#section_lqf_cll_xdb .section}

You can modify the VNC connection password of the ECS instance in the remote terminal connection page. Click **Modify Management Terminal Password**, enter the new password and click **OK** in the dialog box.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6994/15406097344789_en-US.png)

