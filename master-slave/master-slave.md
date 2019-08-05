### MySql主从服务器

使用GTID进行主从复制

### mysql配置文件里面已经配置好了

- master
    ```sql
    ### 主从配置 ###
    # 不同节点的server-id不一样
    server-id=1 
    log-bin=mysql-bin
    gtid_mode=ON
    enforce-gtid-consistency=true
    skip_slave_start
    ```
- slave
    ```sql
    ### 主从配置 ###
    # 不同节点的server-id不一样
    server_id=2
    gtid_mode=ON
    enforce-gtid-consistency=true
    skip_slave_start
    ```

#### master节点

进入master容器
```bash
docker exec -it master bash
```

连接mysql
```bash
mysql -u root -p
```

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

~~查看记录File和Position的值~~
```sql
show master status;
```


#### slave节点

进入slave容器
```bash
docker exec -it slave bash
```

连接mysql
```bash
mysql -u root -p
```

进行slave服务器授权
```sql
CHANGE MASTER TO
MASTER_HOST = '10.0.75.1',
MASTER_PORT = 3306,
MASTER_USER = 'slave',
MASTER_PASSWORD = '666666',
MASTER_AUTO_POSITION = 1;
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
**有时候`Slave_IO_Running`一直是Conecting，slave服务器授权可以试试用root账号，有时候是权限不够**