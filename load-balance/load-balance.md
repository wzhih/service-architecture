### 负载均衡服务器

1. 使用Nginx做负载均衡

    负载服务器ip设置，修改nginx/conf/server.conf文件中的`loads`即可

    本来负载均衡的调度算法打算使用`nginx-upstream-fair`的，打算看到这个模块是2012年写的，然后就没更新过，有点担心，所以用了`least_conn`算法。

    以后如果要改的话，应该是写DockerFile，自己编译`fair`。

2. 使用Haproxy做负载均衡

    负载服务器设置，修改haproxy/conf/haproxy.conf文件中的`backend`即可


3. LVS（这个没去了解）

PS：今天跟朋友聊过，说可以直接购买阿里云等云服务器的负载均衡服务，采用集群部署，可实现会话同步，以消除服务器单点故障，提升冗余，保证服务的稳定性。背靠大厂还是挺可靠的。

##### 基础架构说明

阿里云当前提供四层（TCP协议和UDP协议）和七层（HTTP和HTTPS协议）的负载均衡服务。

- 四层采用开源软件LVS（Linux Virtual Server）+ keepalived的方式实现负载均衡，并根据云计算需求对其进行了个性化定制。
- 七层采用Tengine实现负载均衡。Tengine是由淘宝网发起的Web服务器项目，它在Nginx的基础上，针对有大访问量的网站需求，添加了很多高级功能和特性。
