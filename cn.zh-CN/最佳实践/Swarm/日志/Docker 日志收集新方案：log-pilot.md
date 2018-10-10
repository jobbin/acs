# Docker 日志收集新方案：log-pilot {#concept_ccx_g51_ydb .concept}

本文档介绍一款新的 Docker 日志收集工具：log-pilot。log-pilot 是我们为您提供的日志收集镜像。您可以在每台机器上部署一个 log-pilot 实例，就可以收集机器上所有 Docker 应用日志。\(注意：只支持Linux版本的Docker，不支持Windows/Mac版\)。

log-pilot 具有如下特性：

-   一个单独的 log 进程收集机器上所有容器的日志。不需要为每个容器启动一个 log 进程。
-   支持文件日志和 stdout。docker log dirver 亦或 logspout 只能处理 stdout，log-pilot 不仅支持收集 stdout 日志，还可以收集文件日志。
-   声明式配置。当您的容器有日志要收集，只要通过 label 声明要收集的日志文件的路径，无需改动其他任何配置，log-pilot 就会自动收集新容器的日志。
-   支持多种日志存储方式。无论是强大的阿里云日志服务，还是比较流行的 elasticsearch 组合，甚至是 graylog，log-pilot 都能把日志投递到正确的地点。
-   开源。log-pilot 完全开源，您可以从 [Git项目地址](https://github.com/AliyunContainerService/log-pilot) 下载代码。如果现有的功能不能满足您的需要，欢迎提 issue。

## 快速启动 {#section_s2p_h51_ydb .section}

下面演示一个最简单的场景：先启动一个 log-pilot，再启动一个 tomcat 容器，让 log-pilot 收集 tomcat 的日志。为了简单起见，这里先不涉及阿里云日志服务或者 ELK。如果您想在本地运行，只需要有一台运行 Docker 的机器就可以了。

首先启动 log-pilot。

**说明：** 以这种方式启动，由于没有配置后端使用的日志存储，所有收集到的日志都会直接输出到控制台，所以主要用于调试。

打开终端，输入命令：

```
docker run --rm -it \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /:/host \
    --privileged  \
    registry.cn-hangzhou.aliyuncs.com/acs-sample/log-pilot:0.9.5-filebeat
```

您会看到 log-pilot 的启动日志。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7264/15391417946137_zh-CN.png)

不要关闭终端。新开一个终端启动 tomcat。tomcat 镜像属于少数同时使用了 stdout 和文件日志的 Docker 镜像，非常适合这里的演示。

```
docker run -it --rm  -p 10080:8080 \
-v /usr/local/tomcat/logs \
--label aliyun.logs.catalina=stdout \
--label aliyun.logs.access=/usr/local/tomcat/logs/localhost_access_log.*.txt \
tomcat
```

**说明：**

-   `aliyun.logs.catalina=stdout` 告诉 log-pilot 这个容器要收集 stdout 日志。
-   `aliyun.logs.access=/usr/local/tomcat/logs/localhost_access_log.*.txt` 则表示要收集容器内 /usr/local/tomcat/logs/目录下所有名字匹配localhost\_access\_log.\*.txt的文件日志。后面会详细介绍 label 的用法。

**说明：** 如果您在本地部署 tomcat，而不是在阿里云容器服务上，您需要指定 `-v /usr/local/tomcat/logs`；否则 log-pilot 没法读取到日志文件。容器服务已经自动做了优化，不需自己加 `-v` 了。

log-pilot 会监控 Docker 容器事件，当发现带有 `aliyun.logs.xxx` 容器时，自动解析容器配置，并且开始收集对应的日志。启动 tomcat 之后，您会发现 log-pilot 的终端立即输出了一大堆的内容，其中包含 tomcat 启动时输出的 stdout 日志，也包括 log-pilot 自己输出的一些调试信息。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7264/15391417946140_zh-CN.png)

您可以打开浏览器访问刚刚部署的 tomcat，您会发现每次刷新浏览器，在 log-pilot 的终端里都能看到类似的记录。其中`message` 后面的内容就是从 /usr/local/tomcat/logs/localhost\_access\_log.XXX.txt 里收集到的日志。

## 使用 ElasticSearch + kibana {#section_bfp_h51_ydb .section}

首先要部署一套 ElastichSearch + kibana，您可以参考 [容器服务中使用 ELK](intl.zh-CN/最佳实践/Swarm/日志/容器服务中使用 ELK.md#) 在阿里云容器服务里部署 ELK，或者按照 ElasticSearch/kibana 的文档直接在机器上部署。本文档假设您已经部署好了这两个组件。

如果您还在运行刚才启动的 log-pilot，先关掉，然后使用下面的命令启动。

**说明：** 执行之前，先把 `ELASTICSEARCH_HOST` 和 `ELASTICSEARCH_PORT` 两个变量替换成您实际使用的值。`ELASTICSEARCH_PORT` 一般为 9200。

```
docker run --rm -it \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /:/host \
    --privileged \
    -e FLUENTD_OUTPUT=elasticsearch \
    -e ELASTICSEARCH_HOST=${ELASTICSEARCH_HOST} \
    -e ELASTICSEARCH_PORT=${ELASTICSEARCH_PORT} \
    registry.cn-hangzhou.aliyuncs.com/acs-sample/log-pilot:0.9.5-filebeat
```

相比前面启动 log-pilot 的方式，这里增加了三个环境变量：

-   `FLUENTD_OUTPUT=elasticsearch`：把日志发送到 ElasticSearch。
-   `ELASTICSEARCH_HOST=${ELASTICSEARCH_HOST}`：ElasticSearch 的域名。
-   `ELASTICSEARCH_PORT=${ELASTICSEARCH_PORT}`：ElasticSearch 的端口号。

继续运行前面的 tomcat，再次访问，让 tomcat 产生一些日志，所有这些新产生的日志都将发送到 ElasticSearch 里。

打开 kibana，此时您还看不到新日志，需要先创建 index。log-pilot 会把日志写到 ElasticSearch 特定的 index下，规则如下：

如果应用上使用了标签 `aliyun.logs.tags`，并且 `tags` 里包含 `target`，使用 `target` 作为 ElasticSearch 里的 index。否则，使用标签 `aliyun.logs.XXX` 里的 `XXX` 作为 index。

在前面 tomcat 里的例子里，没有使用 `aliyun.logs.tags` 标签，所以默认使用了 `access` 和 `catalina` 作为 index。我们先创建 index `access`。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7264/15391417946141_zh-CN.png)

创建好 index 就可以查看日志了。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7264/15391417946142_zh-CN.png)

## 在阿里云容器服务里使用 log-pilot {#section_jfp_h51_ydb .section}

容器服务专门为 log-pilot 做了优化，最适合 log-pilot 运行。

要在容器服务里运行 log-pilot，您仅需要使用下面的编排文件创建一个新应用。有关如何创建应用，参见 [创建应用](../../../../intl.zh-CN/用户指南/Swarm 集群/应用管理/创建应用.md#)。

```
pilot:
  image: registry.cn-hangzhou.aliyuncs.com/acs-sample/log-pilot:0.9.5-filebeat
  volumes:
    - /var/run/docker.sock:/var/run/docker.sock
    - /:/host
  privileged: true
  environment:
    FLUENTD_OUTPUT: elasticsearch #按照您的需要替换 
    ELASTICSEARCH_HOST: ${elasticsearch} #按照您的需要替换
    ELASTICSEARCH_PORT: 9200
  labels:
    aliyun.global: true
```

接下来，您就可以在要收集日志的应用上使用 `aliyun.logs.xxx` 标签了。

## label 说明 {#section_ofp_h51_ydb .section}

启动 tomcat 时，声明了下面两个 label 来告诉 log-pilot 这个容器的日志位置。

```
--label aliyun.logs.catalina=stdout 
--label aliyun.logs.access=/usr/local/tomcat/logs/localhost_access_log.*.txt
```

您还可以在应用容器上添加更多的标签。

-   `aliyun.logs.$name = $path`
    -   变量 `name` 是日志名称，只能包含 0~9、a~z、A~Z 和连字符（-）。
    -   变量 `path` 是要收集的日志路径，必须具体到文件，不能只写目录。文件名部分可以使用通配符，例如，`/var/log/he.log` 和 `/var/log/*.log` 都是正确的值，但 `/var/log` 不行，不能只写到目录。`stdout` 是一个特殊值，表示标准输出。
-   `aliyun.logs.$name.format`：日志格式，目前支持以下格式。
    -   none：无格式纯文本。
    -   json：json 格式，每行一个完整的 json 字符串。
    -   csv：csv 格式。
-   `aliyun.logs.$name.tags`：上报日志时，额外增加的字段，格式为 `k1=v1,k2=v2`，每个 key-value 之间使用逗号分隔，例如 `aliyun.logs.access.tags="name=hello,stage=test"`，上报到存储的日志里就会出现 `name` 字段和 `stage` 字段。

    如果使用 ElasticSearch 作为日志存储，`target` 这个 tag 具有特殊含义，表示 ElasticSearch 里对应的 index。


## 扩展 log-pilot {#section_tfp_h51_ydb .section}

对于大部分用户来说，log-pilot 现有功能足以满足需求，如果遇到没法满足的场景，您可以：

-   到 [https://github.com/AliyunContainerService/log-pilot](https://github.com/AliyunContainerService/log-pilot) 提交 issue。
-   直接改代码，再提 PR。

