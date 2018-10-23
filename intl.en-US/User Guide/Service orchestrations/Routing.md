# Routing {#concept_a4r_ftr_xdb .concept}

Configure the access domain name of a service.

Format:

```
aliyun.routing.port_$container_port: [http://]$domain|$domain_prefix[:$context_path]
```

Field explanation:

-   `$container_port`: The container port. **Note:** This is not the host port.
-   `$domain`: The domain name. Enter your own domain name.
-   `$domain_prefix`: The domain name prefix. If you enter a domain name prefix, Container Service provides you with a domain name for test and the domain name suffix is `.<cluster_id>.<region_id>.alicontainer.com`.
-   `$context_path`: The request service path. You can select and distinguish different services according to the request path.

**Domain name selection:**

-   If HTTP protocol is used to expose the service, you can use the internal domain name \(the top-level domain is `alicontainer.com`\) provided by Container Service for test, or use your own domain name.
-   If HTTPS protocol is used, you can use only your own domain name. For example, `www.example.com`. Modify the DNS settings to specify the domain name to the Server Load Balancer service provided by the container cluster.

**Format requirements of the label statement:**

-   Container Service allocates subdomain names for each cluster, and you only need to provide the domain name prefix to bind the internal domain name. The domain name prefix only indicates a domain name level and cannot be separated with periods \(.\).
-   If you do not specify `scheme`, the HTTP protocol is used by default.
-   The length of the domain name cannot exceed 128 characters. The length of the context root cannot exceed 128 characters.
-   When you bind multiple domain names to the service, use semicolons \(;\) to separate them.
-   A backend service can have several ports. Here, the port refers to the port exposed by the container. A port can only use one label for statement and a service with several ports need to state several labels.

**Example:**

Use the routing label.

Bind the internal domain name `wordpress.<cluster_id>.<region_id>.alicontainer.com` provided by Container Service and your own domain name `http://wp.sample.com/context` to port 80 of the Web service.

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

The internal domain name you finally get is `wordpress.cd3dfe269056e4543acbec5e19b01c074.cn-beijing.alicontainer.com`.

After starting the Web service, you can access corresponding Web services by using the URL: `http://wordpress.cd3dfe269056e4543acbec5e19b01c074.cn-beijing.alicontainer.com` or`http://wp.sample.com/context`.

To support HTTPS service, upload the HTTPS certificate by using the Server Load Balancer console on the Alibaba Cloud website. Then, bind the corresponding cluster to access the Server Load Balancer endpoint.

## routing.session\_sticky {#section_oq1_ptr_xdb .section}

This feature enables you to determine whether to maintain session sticky \(session persistence\) when you set the routing for a routing request. With session persistence, during the session, the request is routed to the same backend container instead of being randomly routed to different containers for each request.

**Note:** 

-   The setting takes effect only when you have configured `aliyun.routing.port_$contaienr_port`.
-   Simple route session persistence is based on the Cookie mechanism. By default, the maximum expiration time of Cookie is 8h and the idle expiration time is 30m.

The setting method is as follows:

-   Enable session persistence

    `aliyun.routing.session_sticky: true`

-   Disable session persistence

    `aliyun.routing.session_sticky: false`


Example of a template orchestration file:

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

