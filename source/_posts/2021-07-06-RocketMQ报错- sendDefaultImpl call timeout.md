---
title: RocketMQ报错- sendDefaultImpl call timeout
date: 2021-07-06 08:29:53
categories:
- RocketMQ
tags:
- RocketMQ
---

> springboot发送mq消息报错org.apache.rocketmq.remoting.exception.RemotingTooMuchRequestException: sendDefaultImpl call timeout

可能原因一：端口未开放

可能原因二：broker已经启动成功，可能注册的ip有误

针对原因二可用以下方法解决

1.  编辑broker.conf,添加如下行

```
brokerIP1=服务器ip
```

2.  启动broker,通过-c加载自定义配置文件

```
nohup sh bin/mqbroker -n xx.xx.xx.xx:9876 -c /rocketmq-4.9.3/conf/broker.conf &
```

重启broker后，再次连接rocketmq，程序已经能正常连接了。