# Blue-green release policy with simple routing {#concept_ybt_z45_xdb .concept}

**Note:** By default, simple routing performs session persistence. During a blue-green release, when the new service weight is set to 100%, old requests might be forwarded to the old version of the service. To forward requests to the new version of the service, disable session persistence for the application before the release, see [routing.session\_sticky](intl.en-US/User Guide/Swarm cluster/Service orchestrations/Routing.md#section_oq1_ptr_xdb) or clear cookies after the release.

## Background information {#section_rhl_dp5_xdb .section}

Blue-green release is a zero downtime application update policy. During a blue-green release, the old and new service versions of an application coexist, and also share routes. By adjusting route weights, you can switch traffic between different service versions. After the new version passes the verification, you can delete the old service version by confirming the release. If the new version does not pass the verification, the release is rolled back and the new version is deleted.

## Prerequisites {#section_shl_dp5_xdb .section}

The routing service must be upgraded to the latest version. For more information, see [Upgrade system services](intl.en-US/User Guide/Swarm cluster/Clusters/Upgrade system services.md#).

## Scenario {#section_thl_dp5_xdb .section}

In the following example, assume that you perform a blue-green release for an Nginx static page application. The initial application template is as follows:

```
nginx-v1:
  image: 'registry.aliyuncs.com/ringtail/nginx:1.0'
  labels:
    aliyun.routing.port_80: nginx
  restart: always
```

After the deployment, the page is as follows.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677395609_en-US.png)

## Procedure {#section_h4b_byy_zdb .section}

1.  Log on to the [Container Service console](https://cs.console.aliyun.com).
2.  Under Swarm, click **Applications** in the left-side navigation pane.
3.  Select the cluster in which the application resides from the Cluster drop-down list.
4.  Click **Update** at the right of the application.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405610_en-US.png)

5.  Set the release mode and the configurations of the new service version.

    -   The new and old versions cannot share the same name.
    -   To make sure that the application does not experience downtime when switching versions, the weight of the new service version is set to 0 by default. On the route management page, you must adjust the weight to switch traffic to the new version.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405611_en-US.png)

    The template sample is as follows:

    ```
    nginx-v2:
     image: 'registry.aliyuncs.com/ringtail/nginx:2.0'
     labels:
         aliyun.routing.port_80: nginx
     restart: always
    ```

6.  Click **OK** to release the changed version.

    The release process goes through two statuses:

    -   Blue-green release in progress: Indicates that the startup of the new service version is not completed.
    -   Blue-green release awaiting confirmation: Indicates that the startup of the new service version is completed. Another release can be performed until this release is confirmed or rolled back.

        Click the application name to go to the application details page. You can see that the new and old application versions coexist.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405612_en-US.png)

7.  Click the **Routes** tab and then click **Set service weight**.

    As shown in the following figure, the old service has a weight of 100 and the new service has a weight of 0.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405613_en-US.png)

    To realize zero downtime update, set the weight of the new version to 100. Now the new version and old version account for 50% of the weight respectively. Test if both versions have stable traffic.

    **Note:** Adjusting the weights of the new version and old version at the same time might result in the failure of some requests. Therefore, adjust the weights in two steps and only adjust the weight of one version in each step. For example, adjust the weight of the new version from 0 to 100 first, and then adjust the weight of the old version from 100 to 0 after the traffic is stable.

    Then, adjust the weight of the old version to 0 and that of the new version to 100.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405614_en-US.png)

8.  The routing service enables the session persistence by default. Therefore, you can open a new browser window to access the new version.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405615_en-US.png)

9.  After the entire release process has been verified, click **Confirm Release Completion** on the Application List page. Select whether or not to automatically perform the **smooth update** in the displayed dialog box and click **OK** to confirm the release before you can release subsequent versions.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405616_en-US.png)

    Now the service list of the application has been updated and the old service version has been taken offline and deleted.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7117/15371677405617_en-US.png)


