# Hive环境部署


## 目的
本篇文章是记录在Hadoop部署完成后，如何部署配置Hive。

## 准备环境
1. Hadoop运行环境，可以是[单机伪分布式部署](https://brucemaa.cn/2021/03/hadoop3-standalone-pseudo-distributed-mode-setup/)，可以是[分布式集群部署](https://brucemaa.cn/2021/03/hadoop3-cluster-distributed-mode-setup/)，或者[高可用部署](https://brucemaa.cn/2021/03/hadoop3-cluster-distributed-ha-mode-setup/)。
2. 选择两个服务器部署Hive，一个用于部署服务端，一个用于部署客户端，服务端是至少运行一个Hadoop节点。
3. 下载hive
```bash
root@bigdata-3:# wget https://mirrors.tuna.tsinghua.edu.cn/apache/hive/stable-2/apache-hive-2.3.8-bin.tar.gz
```
3. 解压Hive
```bash
root@bigdata-3:# mkdir -vp /usr/local/hive
root@bigdata-3:# tar -xzvf apache-hive-2.3.8-bin.tar.gz -C /usr/local/hive
root@bigdata-3:# ln -sf /usr/local/hive/apache-hive-2.3.8-bin /usr/local/hive/default
```
4. MySQL存储元数据
运行一个关系型数据库实例，用于存储Hive的元数据。每种关系型数据库都有版本要求：
`MySQL`版本必须最小`5.6.17`，
`PostgreSQL`版本最小`9.1.13`，
`Oracle`版本最小`11g`，
`MS_SQL_SERVER`版本最小`2008.R2`。
确定好后，将数据库驱动复制到hive的lib目录下`/usr/local/hive/default/lib`。
本篇文章选择MySQL数据库，并部署在了`bigdata-4`服务器。

## 开始配置服务端
1. 环境变量`/etc/profile`
```bash
export HIVE_HOME=/usr/local/hive/default
```
2. 进入到目录`$HIVE_HOME/conf`，选择配置文件
```bash
root@bigdata-3:# cp hive-env.sh.template hive-env.sh
root@bigdata-3:# cp hive-default.xml.template hive-site.xml
```
3. `$HIVE_HOME/conf/hive-env.sh`
```bash
HADOOP_HOME=/usr/local/hadoop/default
export HIVE_AUX_JARS_PATH=/usr/local/hive/default/lib
```
4. `$HIVE_HOME/conf/hive-site.xml`
```xml
<configuration>
  <property>
    <name>hive.metastore.warehouse.dir</name>
    <value>/user/hive/warehouse</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://bigdata-4:3306/hadoop_hive?createDatabaseIfNotExist=true</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.cj.jdbc.Driver</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>root</value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>example</value>
  </property>
</configuration>
```
5. 初始化元数据，Hive旧版本会运行时自动初始化
```bash
root@bigdata-3:# $HIVE_HOME/bin/schematool -dbType mysql -initSchema
```
6. HDFS上创建存储路径
这个在进行hive操作时，也会自动创建。
```bash
root@bigdata-3:# $HADOOP_HOME/bin/hdfs dfs -mkdir -p /user/hive/warehouse
```
## 运行服务端
1. 启动hive
```bash
root@bigdata-3:# $HIVE_HOME/bin/hive --service metastore
```
hive是前台启动，后台启动需要`nohup ... &`。
## 配置客户端
客户端选择`bigdata-4`服务器，并且使用`hiveserver2`启动。
1. 客户端配置`$HIVE_HOME/conf/hive-site.xml`
```xml
  <property>
    <name>hive.metastore.uris</name>
    <value>thrift://bigdata-3:9083</value>
  </property>
```
2. 启动hiveservier2替换hive
```bash
root@bigdata-4:# nohup $HIVE_HOME/bin/hiveserver2 &
```
或者
```bash
root@bigdata-4:# nohup $HIVE_HOME/bin/hive --service hiveserver2 &
```

## 使用beeline连接hiveserver2
```bash
root@bigdata-4:# bin/beeline -u jdbc:hive2://bigdata-4:10000
```


