### MySql主从服务器

#### master节点

进入master容器
> docker exec -it master bash

连接mysql
> mysql -u root -p

建立同步账户(用户名：`slave`  密码：`666666` 允许链接IP：`%`)
授予复制权限
```sql
create user 'slave'@'%' identified BY '666666';
grant replication slave on *.* to 'slave'@'%';
```

刷新授权表
```sql
flush privileges;
```

查看记录File和Position的值
```sql
show master status;
```


#### slave节点

进入slave容器
> docker exec -it slave bash

连接mysql
> mysql -u root -p

进行slave服务器授权(`master_log_file`,`master_log_pos`是刚刚记录的File和Position的值)
```sql
change master to master_host='192.168.1.101', master_user='slave', master_password='666666',master_log_file='mysql-bin.000027',master_log_pos=183;
```

查看server_id,不同节点id，不能一样
```sql
show variables like 'server_id';

```
如果id默认为1，可通过以下命令修改
```sql
set global server_id=2;
```

启动
```sql
start slave;
```

```sql
show slave status;
```
Slave_IO_Running和Slave_SQL_Running均为Yes，则主从连接正常