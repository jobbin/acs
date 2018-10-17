# routing {#concept_a4r_ftr_xdb .concept}

设置该服务的访问域名。

格式：

```
aliyun.routing.port_$container_port: [http://]$domain|$domain_prefix[:$context_path]
```

名词解释：

-   `$container_port`: 容器端口，**注意** 该处不是主机的端口。
-   `$domain`: 域名，需要用户填写自己的域名。
-   `$domain_prefix`: 域名前缀，如果填写域名前缀，容器服务会提供给您一个测试用的域名，域名后缀是`.<cluster_id>.<region_id>.alicontainer.com`。
-   `$context_path`: 请求的服务路径，即可以根据请求的路径来选择区分不同的服务。

**绑定域名的选择：**

-   如果使用 HTTP 协议暴露服务，可以使用容器服务提供内部域名（顶级域为 `alicontainer.com`），供您测试使用，也可以使用您提供的域名。
-   如果使用 HTTPS 协议，那么仅支持配置您提供的域名，例如 `www.example.com`。您需要修改 DNS 设置将域名指定到容器集群提供的负载均衡服务上。

**标签声明的格式要求：**

-   容器服务为每一个集群分配了子域名，绑定内部域名只需要给出域名的前缀，域名前缀仅表示域名的一级，不能使用点号（.）进行分隔。
-   如果您不指定 `scheme`，则默认使用 HTTP 协议。
-   域名的长度不能超过 128 个字符，context root 的长度不能超过 128 个字符。
-   绑定多个域名到服务时，域名之间用分号（;）隔开。
-   一个后端服务可以有多个端口，该端口指的是容器暴露的端口，一个端口只能使用一条 label 进行声明，带有多个端口的服务需要声明多个 label。

**示例：**

使用 routing 标签。

将容器服务提供的内部域名 `wordpress.<cluster_id>.<region_id>.alicontainer.com` 绑定到 Web 服务的 80 端口，并且将您提供的自有域名 `http://wp.sample.com/context` 绑定到 Web 服务的 80 端口。

```
web:
  image: wordpress:4.2
  links:
    - db:mysql
  labels:
    aliyun.routing.port_80: wordpress;http://wp.sample.com/context
db:
  image: mysql
  environment:
    - MYSQL_ROOT_PASSWORD=password
```

最终您得到的内部域名为 `wordpress.cd3dfe269056e4543acbec5e19b01c074.cn-beijing.alicontainer.com`。

Web 服务运行之后，您可以通过 `http://wordpress.cd3dfe269056e4543acbec5e19b01c074.cn-beijing.alicontainer.com` 或者 `http://wp.sample.com/context` 访问相应的 Web 服务。

如果您需要支持 HTTPS 服务，需要自行通过阿里云官网负载均衡管理控制台上传 HTTPS 证书，并绑定相应的集群对外访问负载均衡端点。

## routing.session\_sticky {#section_oq1_ptr_xdb .section}

设置 routing 在做请求路由的时候，是否保持 session sticky，即会话保持。其效果是，在某个会话时间内，请求一直路由到同一个后端的容器，而不是每次请求都随机路由到不同的容器。

**说明：** 

-   只有当您已经设置了 `aliyun.routing.port_$container_port` 时，该设置才能起作用。
-   简单路由会话保持基于Cookie机制，默认Cookie最大过期时间8h，空闲过期时间30m。
-   简单路由默认已开启会话保持机制。

其设置方法如下：

-   开启会话保持

    `aliyun.routing.session_sticky: true`

-   关闭会话保持

    `aliyun.routing.session_sticky: false`


模板编排文件示例：

```
web:
  image: wordpress:4.2
  links:
    - db:mysql
  labels:
    aliyun.routing.port_80: wordpress;http://wp.sample.com/context
    aliyun.routing.session_sticky: true
db:
  image: mysql
  environment:
    - MYSQL_ROOT_PASSWORD=password
```

