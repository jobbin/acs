# Connect to a cluster by using Docker tools {#concept_v35_c3c_5db .concept}

Container Service is fully compatible with the [Docker Swarm API](https://docs.docker.com/swarm/). You can access and manage Docker clusters by using common Docker tools, such as Docker client and Docker Compose.

## Install a certificate {#section_cnm_tmf_vdb .section}

1.  Obtain the access address.
    1.  Log on to the [Container Service console](https://partners-intl.console.aliyun.com/#/cs).
    2.  Under Swarm, click **Clusters** in the left-side navigation pane. In the cluster list, click **Manage** at the right of a cluster.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6870/15396912876069_en-US.png)

        As shown in the following figure, the cluster connection information is displayed.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6870/15396912876070_en-US.png) 

2.  Download and save the TLS certificate.

    Configure a TLS certificate before you use the preceding access address to access the Docker cluster.

    Click **Download Certificate**in the cluster details page to download the TLS certificate. The certFiles.zip file is downloaded. In the following example, the downloaded certificate is saved to the ~/.acs/certs/ClusterName/directory. ClusterName indicates the name of your cluster. You can save the certificate to a different directory, but we recommend that you use the ~/.acs/certs/ClusterName/ directory for easy management.

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
    -H=tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599 ps #Replace ClusterName and tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599 with the actual path and access address.
    ```

-   Environment variables

    ```
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599" #Replace tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599 with the actual access address
    export DOCKER_CERT_PATH=~/.acs/certs/ClusterName #Replace ClusterName with the actual path
    docker ps
    ```

    The preceding two examples show how to run the `docker ps` command in the cluster. You can replace `ps` with any other Docker command. For example, you can run the `docker run` command to start a new container.


## Use Docker Compose to manage clusters {#section_cqq_smf_vdb .section}

Docker Compose supports declaring an access address and a certificate by using environment variables.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599"
export DOCKER_CERT_PATH=~/.acs/certs/ClusterName
docker-compose up
```

## Revoke a certificate {#section_sq5_34f_vdb .section}

If your downloaded certificate is accidentally leaked during usage, revoke the certificate as soon as possible. Click **Revoke Downloaded Certificate** in the cluster details page to revoke the downloaded certificate. Then, you can download a new certificate.

**Note:** Clicking **Revoke Downloaded Certificate** makes the downloaded certificate unavailable.

