# 通过 Docker 工具连接集群 {#concept_v35_c3c_5db .concept}

容器服务提供完全兼容 [Docker Swarm API](https://docs.docker.com/swarm/) 的能力。您可以通过常用的 Docker 工具访问和管理 Docker 集群，例如 Docker Client 和 Docker Compose。

## 安装证书 {#section_cnm_tmf_vdb .section}

1.  获取访问地址。
    1.  登录[容器服务管理控制台](https://cs.console.aliyun.com/?spm=a2c4g.11186623.2.4.AmPWm2#/overview/all)。
    2.  在Swarm菜单下，单击左侧导航栏中的**集群**，在集群列表中选择一个集群并单击**管理**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6870/15396894016069_zh-CN.png)

        您可以查看集群的连接信息，如下图所示。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6870/15396894016070_zh-CN.png) 

2.  下载和保存证书。

    要通过上面的访问地址访问 Docker 集群，您还需要配置 TLS 证书。

    在集群管理页面，单击**下载证书**开始下载 TLS 证书。下载到的文件为 certFiles.zip。在下面的例子中，下载的证书存放在 ~/.acs/certs/ClusterName/目录下。其中，ClusterName是您集群的名字。您也可以使用其他目录，但是为了便于管理，推荐您将文件存放在~/.acs/certs/ClusterName/ 目录下。

    ```
    mkdir ~/.acs/certs/ClusterName/ #替换成真正的集群名字 
    cd ~/.acs/certs/ClusterName/ 
    cp /path/to/certFiles.zip . 
    unzip certFiles.zip
    ```

    在这个目录里，您可以看到三个文件ca.pem、cert.pem和key.pem。


## 管理集群 {#section_r3b_tmf_vdb .section}

**使用 Docker Client 管理集群**

您可以使用 Docker Client 访问容器服务上的容器集群。访问集群之前，您需要使用以下两种方法之一配置使用的证书和访问地址。

-   在命令行参数里配置

    ```
    docker --tlsverify --tlscacert=~/.acs/certs/ClusterName/ca.pem --tlscert=~/.acs/certs/ClusterName/cert.pem --tlskey=~/.acs/certs/ClusterName/key.pem \
    -H=tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599 ps #把 ClusterName 和 tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599 替换成您实际使用的路径和访问地址。
    ```

-   使用环境变量

    ```
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599" #把 tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599 替换成您实际使用的访问地址
    export DOCKER_CERT_PATH=~/.acs/certs/ClusterName #把 ClusterName 替换成您实际使用的路径
    docker ps
    ```

    上面的两个例子在集群上执行了 `docker ps` 命令，您可以把 `ps` 替换成任何其他 Docker 支持的命令，比如使用 `docker run` 启动一个新的容器。


## 使用 Docker Compose 管理集群 {#section_cqq_smf_vdb .section}

目前 Docker Compose 支持通过环境变量声明访问地址和证书。

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://master2g1.cs-cn-Qingdao.aliyun.com:11599"
export DOCKER_CERT_PATH=~/.acs/certs/ClusterName
docker-compose up
```

## 证书吊销 {#section_sq5_34f_vdb .section}

如果在使用过程中不慎导致证书泄露，您需要尽快吊销证书。在集群管理页面，单击**吊销已下载的证书** 可以吊销您之前下载的证书。吊销之后，您可以重新下载证书。

**说明：** 单击**吊销已下载的证书**将导致您之前下载的证书不可用。

