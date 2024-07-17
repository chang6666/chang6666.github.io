---
title: 基于Docker搭建MySql主从复制
date: 2021-03-01 08:29:53
categories:
- Docker
- MySql
tags:
- Docker
- MySql
---

> 拉取MySql镜像

```
docker pull mysql
```

> 使用此镜像启动容器，这里需要分别启动主从两个容器

**Master:**

```
docker run -p 3306:3306 --name mysqlMaster -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

**Slave:**

```
docker run -p 3307:3306 --name mysqlSlave -e MYSQL_ROOT_PASSWORD=123456 -d mysql
```

使用`docker ps`命令查看正在运行的容器：

![WX20220215-174513@2x](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081409607.png)

> 配置Master(主)

通过`docker exec -it 5ef0f7741f8f /bin/bash`命令进入到Master容器内部，也可以通过`docker exec -it mysql8 /bin/bash`命令进入。5ef0f7741f8f是容器的id,而mysql8是容器的名称。

-   `cd /etc/mysql`切换到/etc/mysql目录下，然后`vi my.cnf`对my.cnf进行编辑。此时会报出`bash: vi: command not found`，需要我们在docker容器内部自行安装vim。使用`apt-get install vim`命令安装vim
-   执行`apt-get update`，然后再次执行`apt-get install vim`即可成功安装vim。然后我们就可以使用vim编辑my.cnf，在my.cnf中添加配置如下：

```
[mysqld]
## 同一局域网内注意要唯一
server-id=100  
## 开启二进制日志功能，可以随便取（关键）
log-bin=mysql-bin
```

配置完成之后，需要重启mysql服务使配置生效。使用`docker restart mysql8`完成重启。

> 配置Slave(从)

和配置Master(主)一样，在Slave配置文件my.cnf中添加如下配置：

```
[mysqld]
## 设置server_id,注意要唯一
server-id=101  
## 开启二进制日志功能，以备Slave作为其它Slave的Master时使用
log-bin=mysql-slave-bin   
## relay_log配置中继日志
relay_log=edu-mysql-relay-bin 
```

配置完成后也需要重启docker容器，操作和配置Master(主)一致。

> 链接Master(主)和Slave(从)

在Master进入mysql，执行`show master status;`

![WX20220215-180320@2x](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081410270.png)

File和Position字段的值后面将会用到，在后面的操作完成之前，需要保证Master库不能做任何操作，否则将会引起状态变化，File和Position字段的值变化。

在Slave 中进入 mysql，执行`change master to master_host='172.17.0.2', master_user='root', master_password='123456', master_port=3306, master_log_file='mysql-bin.000001', master_log_pos= 1183, master_connect_retry=30;`

**命令说明：**

-   **master_host** ：Master的地址，指的是容器的独立ip,可以通过`docker inspect --format='{{.NetworkSettings.IPAddress}}' 容器名称|容器id`查询容器的ip
-   **master_port**：Master的端口号，指的是容器的端口号
-   **master_user**：用于数据同步的用户
-   **master_password**：用于同步的用户的密码
-   **master_log_file**：指定 Slave 从哪个日志文件开始复制数据，即上文中提到的 File 字段的值
-   **master_log_pos**：从哪个 Position 开始读，即上文中提到的 Position 字段的值
-   **master_connect_retry**：如果连接失败，重试的时间间隔，单位是秒，默认是60秒

在Slave 中的mysql终端执行`show slave status;`用于查看主从同步状态。正常情况下，SlaveIORunning 和 SlaveSQLRunning 都是No，因为我们还没有开启主从复制过程。使用`start slave`开启主从复制过程，然后再次查询主从同步状态`show slave status;`。

![WX20220215-181609@2x](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081410542.png)

SlaveIORunning 和 SlaveSQLRunning 都是Yes，说明主从复制已经开启。此时可以测试数据同步是否成功。

> 测试主从复制

**Master:**

![WX20220215-182042@2x](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081410651.png)

**Slave:**

![WX20220215-182042@2x](https://raw.githubusercontent.com/ChangNingbo/blog_img/main/img202208081410865.png)

> 问题排查

-   Could not find first log file name in binary log index file

1.  首先停止从库同步

```
stop slave;
```

2.  主库中关闭当前的二进制日志文件并创建一个新文件，新的二进制日志文件的名字在当前的二进制文件的编号上加1.

```
flush logs;
```

3.  查看主库状态，主要查看日志文件和位置

```
show master status;
```

4.  回到从库中，执行命令，使日志文件和位置对应主库

```
CHANGE MASTER TO MASTER_LOG_FILE='log-bin.000001',MASTER_LOG_POS=116;
```

5.  启动从库

```
start slave;
show slave status; 
```