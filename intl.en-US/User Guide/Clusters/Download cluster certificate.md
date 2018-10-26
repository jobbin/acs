# Download cluster certificate {#task_thx_xrl_xdb .task}

With the downloaded certificate, you can connect to the endpoint exposed from the cluster by using Docker Swarm API or Docker client. For more information, see [Connect to a cluster by using Docker tools](../../../../reseller.en-US/Quick Start/Connect to a cluster by using Docker tools.md#).

1.   Obtain the access address. 
    1.   Click  **Clusters** in the left-side navigation pane. On the Cluster List page, click **Manage** at the right of a cluster. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6998/15405491454821_en-US.png)

    2.   The cluster details page is displayed, showing the cluster connection information. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6998/15405491454822_en-US.png)

2.   Download and save the TLS certificate. 

    Configure a TLS certificate before you use the preceding access address to access the Docker cluster.

    Click **Download Certificate** in the cluster details page to download the TLS certificate. The certFiles.zip file is downloaded.   certificate.  The `certFiles.zip` file is downloaded.   In the following example, the downloaded certificate is saved to the   ~/.acs/certs/ClusterName/ directory. `ClusterName` indicates the name of your cluster.  You can save the certificate to a different directory, but we recommend using the ~/.acs/certs/ClusterName/ directory for easy management.

    ```
    mkdir ~/.acs/certs/ClusterName/ #Replace ClusterName with your cluster name
     cd ~/.acs/certs/ClusterName/
     cp /path/to/certFiles.zip .
     unzip certFiles.zip
    ```

    The certFiles.zip file contains ca.pem, cert.pem, and key.pem.


