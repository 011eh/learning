# 案例

> MySQL Dockerfile

```dockerfile
FROM mysql
VOLUME /var/lib/mysql
VOLUME /var/log/mysql
VOLUME /etc/mysql
ADD my.cnf /etc/mysql
ENV MYSQL_ROOT_PASSWORD=[你的密码]
EXPOSE 3306
```



> 主机配置文件

```
[mysqld]
server-id = 1
binlog-do-db = test
binlog-ignore-db = mysql, information_schema, performance_schema
```



> 从机配置文件

```
[mysqld]
server-id = 2
replicate-do-db = test
replicate-ignore-db = mysql, information_schema, performance_schema
```



> 执行

```sql
# 主机执行
## 从机设置 source_log_file 需要
show master status;

create database test char set utf8;
use test;
create table test (
    id   int auto_increment primary key,
    info varchar(100)
);
insert into test(info)
values ('info');

# 从机执行
change replication source  to source_host = 'mysql0',source_user = 'root',source_password = 'password',source_log_file = 'binlog.000005',source_log_pos = 157,source_connect_retry = 20;
start replica;
show replica status;
```



```yaml
version: "3"
services:
  master:
    build:
      context: ./master
      dockerfile: Dockerfile
    image: 011eh/mysql-m
    container_name: mysql0
    ports:
      - "3306:3306"
    volumes:
      - ./cluster/master/data:/var/lib/mysql
      - ./cluster/master/logs:/var/log/mysql
      - ./cluster/master/conf:/etc/mysql
    networks:
      - mysql_cluster

  slave:
    build:
      context: ./slave
      dockerfile: Dockerfile
    image: 011eh/mysql-s
    container_name: mysql1
    ports:
      - "3307:3306"
    volumes:
      - ./cluster/slave/data:/var/lib/mysql
      - ./cluster/slave/logs:/var/log/mysql
      - ./cluster/slave/conf:/etc/mysql
    networks:
      - mysql_cluster

networks:
  mysql_cluster:
```

