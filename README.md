# 后端服务架构初想及实现

- [反向代理服务器](proxy-server/proxy-server.md)
- [负载均衡服务器](load-balance/load-balance.md)
- 业务处理服务器
    - [应用服务器](service/service.md)
    - 消息队列服务器
- [分布式集群存储服务器](data-storage/data-storage.md)
    - 缓存服务器，redis，cache（session存储等）
    - 关系型数据库（[读写分离，主从](master-slave/master-slave.md)）
    - 文件存储服务器（OSS对象存储服务器）

##### 端口分配
当前主机ip：192.168.1.102，所有服务器通过主机端口通信

我是在自己主机上面用docker搭建的，端口分配如下所示
1. 反向代理服务器：`80`
2. 负载均衡服务器
    - nginx: `8000`
    - haproxy: `8001`
3. 业务处理服务器：`8010`,`8011`......
4. 存储服务器
    - redis: `6379`
    - mysql/mariadb：`3306`,`3307`,`33070`
    - mongo: `27017`

##### 架构初想图

![image](后端架构初想图.png)

##### 其他东西

使用composer docker镜像安装或更新的命令如下：
```
docker --rm -it -v $PWD:/app composer install --ignore-platform-reqs --no-scripts
```

要把docker容器里的某个文件/文件夹复制出来，可以使用以下命令

```
docker run --name tmp-nginx -d nginx
docker cp tmp-nginx:/etc/nginx/nginx.conf /host/path/nginx.conf
docker rm -f tmp-nginx
```

##### 一己之见

软件瓶颈：计算（并发），存储（读写），归根到底是速度的问题，于是有的用空间换取时间，有的用数量换取质量，其实都是一样的做法，除非计算机硬件底层或者其他方面有突破，否则，只能牺牲部分某些东西换取软件高速高质量运行。有点“饮鸩止渴”的意思。

畅想一下未来：忽略外在影响，在最理想的情况下，软件到底是个怎么样的形式？

现在是‘人’，计算与数据，未来有可能是以‘人’为主体，计算、数据都在个体中。有点区块链的感觉呢，只不过数据不上链。

