# Routing and Server Load Balancer between services in a cluster {#concept_xjt_zvs_xdb .concept}

Container Service can expose the HTTP service based on domain names by using acsrouting,  and work with health check to enable the automatic Server Load Balancer and service discovery. When one container malfunctions,  routing will automatically remove the container that failed the health check from the backend, which achieves the automatic service discovery.  However, in this way, the service is exposed to the Internet.

Then, how can automatic service discovery and Server Load Balancer be achieved between services in a cluster by using this method?  The routing container of Alibaba Cloud Container Service has the function of Server Load Balancer.  Use the domain name ending with `.local`  to make the container can only be accessed by the other containers in the cluster, and then work with the external\_links  label to implement the inter-service discovery and Server Load Balancer in the cluster.

## Implementation principle {#section_ts3_tcz_xdb .section}

1.  Docker version later than 1.10 supports alias resolution in the container.  In the restservice container that depends and loads on the restserver.local, the `restserver.local` domain name resolves the address of the routing container.  When the restclient service initiates a request, the HTTP request is forwarded to the routing container,  with `HOST` as the request header of restserver.local.
2.  Routing container monitors the health status of the containers configured with `aliyun.routing.port_xxx: restserver.local` label  and mounts the status to the backend of HAProxy. When HAProxy receives the HTTP request with the `restserver.local`  HOST header, the request can be forwarded to the corresponding container.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7108/15423558055564_en-US.png)

## Advantages {#section_lp2_vcz_xdb .section}

-   Compared with the DNS-based method using link or hostname, the inconsistent handling of DNS cache by different clients will delay service discovery,  and the DNS solution which only includes round robin cannot meet the requirements of microservice scenarios.
-   Compared with other microservice discovery solutions, this solution provides a mechanism to achieve unrelated service discovery and Server Load Balancer,  which can be used without any modification on the server side or client application.
-   In decoupling service lifecycle, every microservice can adopt a Docker Compose template for independent deployment and update.  Only a virtual domain name is required to achieve dynamic mutual binding.

## Orchestration example {#section_xs3_tcz_xdb .section}

In the following orchestration example, add the `aliyun.routing.port_80:restserver.local` label  to the restserver service to make sure only the containers in the cluster can access this domain name.  Then, configure `external_links` for the restclient service,  pointing to the restserver.local domain name.  The restclient service can use this domain name to access  the restserver service, and work with health check to implement automatic service discovery.

```
restserver: # Simulate the rest service. 
  image: nginx
  labels:
    aliyun.routing.port_80: restserver.local # Use the local domain name and only the containers in the cluster can access this domain name.
    aliyun.scale: "2" # Expand two instances to simulate the Server Load Balancer.
     aliyun.probe.url: "http://container:80" # Define the container health check policy as http and the port as 80.
    aliyun.probe.initial_delay_seconds: "2" # The health check starts two seconds after the container is started.
     aliyun.probe.timeout_seconds: "2" # The timeout for health check. A container is considered as unhealthy if no result is returned in two seconds.
restclient: # Simulate the rest service consumer.
  image: registry.aliyuncs.com/acs-sample/alpine:3.3
  command: "sh -c 'apk update; apk add curl; while true; do curl --head restserver.local; sleep 1; done'" # Access the rest service and test the Server Load Balancer.
  
  tty: true  
  external_links:  
      - "restserver.local" # Specify the link service domain name.  Make sure that you set external_links. Otherwise, the access fails.
```

The following restclient service logs show that the HTTP request of restclient curl is routed to the containers of different rest services. The container ID   is `053cb232fdfbcb5405ff791650a0746ab77f26cce74fea2320075c2af55c975f` and `b8c36abca525ac7fb02d2a9fcaba8d36641447a774ea956cd93068419f17ee3f`.

```
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066803626Z Server: nginx/1.11.1
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066814507Z Date: Fri, 01 Jul 2016 06:43:49 GMT
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066821392Z Content-Type: text/html
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066829291Z Content-Length: 612
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066835259Z Last-Modified: Tue, 31 May 2016 14:40:22 GMT
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066841201Z ETag: "574da256-264"
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066847245Z Accept-Ranges: bytes
internal-loadbalance_restclient_1 | 2016-07-01T06:43:49.066853137Z Set-Cookie: CONTAINERID=053cb232fdfbcb5405ff791650a0746ab77f26cce74fea2320075c2af55c975f; path=/
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.080502413Z HTTP/1.1 200 OK
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.082548154Z Server: nginx/1.11.1
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.082559109Z Date: Fri, 01 Jul 2016 06:43:50 GMT
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.082589299Z Content-Type: text/html
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.082596541Z Content-Length: 612
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.082602580Z Last-Modified: Tue, 31 May 2016 14:40:22 GMT
internal-loadbalance_restclient_1  2016-07-01T06:43:50.082608807Z ETag: "574da256-264"
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.082614780Z Accept-Ranges: bytes
internal-loadbalance_restclient_1 | 2016-07-01T06:43:50.082621152Z Set-Cookie: CONTAINERID=b8c36abca525ac7fb02d2a9fcaba8d36641447a774ea956cd93068419f17ee3f; path=/
```

