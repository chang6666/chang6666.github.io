---
title: ElasticSearch的安装与简单配置
date: 2022-02-01 08:29:53
categories:
- ElasticSearch
tags:
- ElasticSearch
---

一、启动单节点

~~~shell
bin/elasticsearch -E node.name=node0 -E cluster.name=elastic -E path.data=node0_data
~~~

二、安装插件

~~~shell
bin/elasticsearch-plugin install analysis-icu
~~~

三、 查看插件

~~~shell
bin/elasticsearch-plugin install analysis-icu
~~~

四、查看安装的插件

~~~shell
GET http://localhost:9200/_cat/plugins?v
~~~

五、启动多节点

~~~shell
bin/elasticsearch -E node.name=node0 -E cluster.name=elastic -E path.data=node0_data
bin/elasticsearch -E node.name=node1 -E cluster.name=elastic -E path.data=node1_data
bin/elasticsearch -E node.name=node2 -E cluster.name=elastic -E path.data=node2_data
bin/elasticsearch -E node.name=node3 -E cluster.name=elastic -E path.data=node3_data
~~~

六、查看集群

~~~shell
GET http://localhost:9200
~~~

七、查看nodes

~~~shell
GET _cat/nodes
GET _cluster/health
~~~

八、添加访问密码

1. 编辑elasticsearch.yml

   ~~~
   network.host: 0.0.0.0
   http.port: 9200
   discovery.seed_hosts: ["0.0.0.0"]
   cluster.initial_master_nodes: ["node-1"]
   
   xpack.security.enabled: true
   xpack.license.self_generated.type: basic
   xpack.security.transport.ssl.enabled: true
   ~~~

2. 设置es密码

   ~~~shell
   ./elasticsearch-setup-passwords interactive
   ~~~

   按照提示设置密码即可