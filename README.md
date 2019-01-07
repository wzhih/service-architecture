# 后端服务架构初想及实现

- 反向代理服务器
- 负载均衡服务器
- 业务处理服务器
    - 应用服务器
    - 消息队列服务器
- 分布式集群存储服务器
    - 缓存服务器，redis，cache（session存储等）
    - 关系型数据库（读写分离，主从）
    - 文件存储服务器（OSS对象存储服务器）

##### 端口分配
当前主机ip：192.168.1.102，所有服务器通过主机端口通信
我是在自己主机上面用docker搭建的，端口分配如下所示
1. 反向代理服务器：`80`
2. 负载均衡服务器：`8000`
3. 业务处理服务器：`8010`,`8011`
4. 存储服务器
    1. redis: `6379`
    2. mysql/mariadb：`3306`
    3. mongo: `27017`



##### 一己之见

软件瓶颈：计算（并发），存储（读写），归根到底是速度的问题，于是有的用空间换取时间，有的用数量换取质量，其实都是一样的做法，除非计算机硬件底层或者其他方面有突破，否则，只能牺牲部分某些东西换取软件高速高质量运行。有点“饮鸩止渴”的意思。

畅想一下未来：忽略外在影响，在最理想的情况下，软件到底是个怎么样的形式？
现在是‘人’，计算与数据，未来有可能是以‘人’为主体，计算、数据都在个体中。有点区块链的感觉呢，只不过数据不上链。

