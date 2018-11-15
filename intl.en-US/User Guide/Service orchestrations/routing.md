# routing {#concept_a4r_ftr_xdb .concept}

The routing label configures the access domain name of a service.

Format:

```
aliyun.routing.port_$container_port: [http://]$domain|$domain_prefix[:$context_path]
```

Field explanation:

-   `$container_port`: container port. **Note:** This is not the host port.
-   `$domain`: domain name. Enter a domain name.
-   `$domain_prefix`: domain name prefix. If you enter a domain name prefix, Container Service provides you with a test domain name and the domain name suffix is `.<cluster_id>.<region_id>.alicontainer.com`.
-   `$context_path`: requested service path. You can select services according to the requested path.

**Domain name selection:**

-   If the HTTP protocol is used to expose the service, you can use the internal domain name \(the top-level domain is `alicontainer.com`\) provided by Container Service for testing, or use your own domain name.
-   If the HTTPS protocol is used, you can use only your own domain name. For example, `www.example.com`. You must modify the DNS settings to assign the domain name to the Server Load Balancer service provided by the container cluster.

**Format requirements of the label statement:**

-   Container Service allocates a subdomain name to each cluster, and you only need to provide the domain name prefix to bind the internal domain name. The domain name prefix only indicates a domain name level and cannot be separated with periods \(.\).
-   If you do not specify `scheme`, the HTTP protocol is used by default.
-   The length of the domain name cannot exceed 128 characters. The length of the context root cannot exceed 128 characters.
-   When you bind multiple domain names to the service, use semicolons \(;\) to separate them.
-   A backend service can have multiple ports. These ports are exposed by the container. A port can only be assigned one label. Therefore, a service with multiple ports must be assigned multiple labels.

**Example:**

Use the routing label.

Bind the internal domain name`wordpress.<cluster_id>.<region_id>.alicontainer.com` provided by Container Service and your own domain name`http://wp.sample.com/context` to port 80 of the Web service.

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

The internal domain name that you finally get is `wordpress.cd3dfe269056e4543acbec5e19b01c074.cn-beijing.alicontainer.com`.

After starting the Web service, you can access the corresponding Web services by using the URL: `http://wordpress.cd3dfe269056e4543acbec5e19b01c074.cn-beijing.alicontainer.com` or`http://wp.sample.com/context`.

To support the HTTPS service, upload the HTTPS certificate by using the Server Load Balancer console on the Alibaba Cloud website, and then bind the corresponding cluster to access the Server Load Balancer terminal.

## routing.session\_sticky {#section_oq1_ptr_xdb .section}

By using this feature, you can determine whether to maintain session sticky \(session persistence\) when you set the routing for a routing request. With session persistence, during the session, each request is routed to the same backend container instead of being randomly routed to different containers.

**Note:** 

-   The setting takes effect only when you have configured `aliyun.routing.port_$contaienr_port`.
-   Simple routing session persistence is based on the Cookie mechanism. By default, the maximum expiration time of Cookie is 8 hours and the idle expiration time is 30 minutes.
-   Simple routing session persistence is enabled by default.

The setting methods are as follows:

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

