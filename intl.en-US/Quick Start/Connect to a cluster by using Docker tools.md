# Connect to a cluster by using Docker tools {#concept_v35_c3c_5db .concept}

Container Service is fully compatible with the [Docker Swarm API](https://docs.docker.com/swarm/). You can access and manage Docker clusters by using common Docker tools, such as Docker client and Docker Compose.

## Install a certificate {#section_cnm_tmf_vdb .section}

1.  Obtain the access address.
    1.  Log on to the [Container Service console](https://partners-intl.console.aliyun.com/#/cs%20%22Container%20Service%20console%22).
    2.  Click **Clusters** in the left-side navigation pane. On the Cluster List page, click **Manage** at the right of a cluster.

        ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/25983/cn_zh/1509936787945/Image%202.png)

        The cluster details page appears, showing the cluster connection information.

        ![](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/25983/cn_zh/1510020587388/connect2.png)

2.  Download and save the TLS certificate.

    Configure a TLS certificate before using the preceding access address to access the Docker cluster.

    Click **Download Certificate**in the cluster details page to download the TLS certificate.  The certFiles.zip file is downloaded.  In the following example, the downloaded certificate is saved to the ~/.acs/certs/ClusterName/directory. ClusterName indicates the name of your cluster.  You can save the certificate to a different directory, but we recommend that you use the ~/.acs/certs/ClusterName/ directory for easy management.

    ```
    mkdir ~/.acs/certs/ClusterName/ #Replace ClusterName with your cluster name 
    cd ~/.acs/certs/ClusterName/ 
    cp /path/to/certFiles.zip . 
    unzip certFiles.zip
    ```

    The certFiles.zip file contains ca.pem, cert.pem, and key.pem files.


## Manage clusters {#section_r3b_tmf_vdb .section}

**Use Docker client to manage clusters**

You can use Docker client to access the container clusters of Container Service. To do this, you must configure a certificate and an access address by using:

-   Command-line parameters

    ```
    docker --tlsverify --tlscacert=~/.acs/certs/ClusterName/ca.pem --tlscert=~/.acs/certs/ClusterName/cert.pem --tlskey=~/.acs/certs/ClusterName/key.pem \
    -H=tcp://master4g5.cs-cn-hangzhou.aliyun.com:21003 ps #Replace ClusterName and tcp://master4g5.cs-cn-hangzhou.aliyun.com:21003 with the actual path and access address
    ```

-   Environment variables

    ```
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://master4g5.cs-cn-hangzhou.aliyun.com:21003" #Replace tcp://master4g5.cs-cn-hangzhou.aliyun.com:21003 with the actual access address
    export DOCKER_CERT_PATH=~/.acs/certs/ClusterName #Replace ClusterName with the actual path
    docker ps
    ```

    The preceding two examples show how to run the `docker ps` command in the cluster. You can replace `ps` with any other Docker command. For example, you can run the `docker run` command to start a new container.


## Use Docker Compose to manage clusters {#section_cqq_smf_vdb .section}

Docker Compose supports declaring an access address and a certificate by using environment variables.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://master4g5.cs-cn-hangzhou.aliyun.com:21003"
export DOCKER_CERT_PATH=~/.acs/certs/ClusterName #Replace ClusterName with the actual path
docker-compose up
```

## Revoke a certificate {#section_sq5_34f_vdb .section}

If your downloaded certificate is accidentally leaked during usage, revoke the certificate as soon as possible. Click **Revoke Downloaded Certificate** in the cluster details page to revoke the downloaded certificate.  Then, you can download a new certificate.

**Note:** Clicking **Revoke Downloaded Certificate** will make the earlier downloaded certificate unavailable.

