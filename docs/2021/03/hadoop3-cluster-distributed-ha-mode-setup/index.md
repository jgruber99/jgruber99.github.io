# Hadoop3分布式高可用部署


## 目的
本篇文章介绍如何使用Zookeeper来部署Hadoop的高可用配置。

## 准备环境
4个虚拟机
```bash
10.4.53.78 bigdata-1
10.4.53.79 bigdata-2
10.4.53.80 bigdata-3
10.4.53.81 bigdata-4
```
### 分配环境
1. Zookeeper集群，在`bigdata-2`，`bigdata-3`，`bigdata-4`上部署。具体可以参考[这篇文章](https://brucemaa.cn/2021/03/linux-install-zk-cluster-mode-setup/)。
2. Hadoop分布式集群参考[这篇文章](https://brucemaa.cn/2021/03/hadoop3-cluster-distributed-mode-setup/)，2个`NameNode`节点管理HDFS，2个`ResourceManager`节点管理资源，3个`JournalNode`节点用于`NameNode`通信，3个`DataNode`节点用于干活，3个`NodeManager`跟着`DataNode`。

 ||bigdata-1|bigdata-2|bigdata-3|bigdata-4|
|-|:-:|:-:|:-:|:-:|
Zookeeper||✅|✅|✅|
NameNode|✅|✅|||
ResourceManager|||✅|✅|
DataNode||✅|✅|✅|
NodeManager||✅|✅|✅|
JournalNode|✅|✅|✅||

## 配置HDFS高可用
在`bigdata-1`服务器上修改`$HADOOP_HOME/etc/hadoop`下的配置文件
1. core-site.xml
```xml
<configuration>
  <!-- 指定 namenode 的 hdfs 协议文件系统的通信地址的别名 -->
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://mycluster</value>
  </property>
  <!-- 指定 hadoop 集群存储临时文件的目录 -->
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/opt/hadoop/ha</value>
  </property>
  <!-- ZooKeeper 集群的地址 -->
  <property>
    <name>ha.zookeeper.quorum</name>
    <value>bigdata-2:2181,bigdata-3:2181,bigdata-4:2181</value>
  </property>
  <!-- ZKFC 连接到 ZooKeeper 超时时长 -->
  <property>
    <name>ha.zookeeper.session-timeout.ms</name>
    <value>10000</value>
  </property>
</configuration>
```
2. hdfs-site.xml
```xml
<configuration>
  <!-- 指定 HDFS 副本的数量 -->
  <property>
    <name>dfs.replication</name>
    <value>3</value>
  </property>
  <!-- namenode 节点数据（即元数据）的存放位置，可以指定多个目录实现容错，多个目录用逗号分隔 -->
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/opt/hadoop/ha/nn/data</value>
  </property>
  <!-- datanode 节点数据（即数据块）的存放位置 -->
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/opt/hadoop/ha/dn/data</value>
  </property>
  <!-- 集群服务的逻辑名称 -->
  <property>
    <name>dfs.nameservices</name>
    <value>mycluster</value>
  </property>
  <!-- NameNode ID 列表-->
  <property>
    <name>dfs.ha.namenodes.mycluster</name>
    <value>nn1,nn2</value>
  </property>
  <!-- nn1 的 RPC 通信地址 -->
  <property>
    <name>dfs.namenode.rpc-address.mycluster.nn1</name>
    <value>bigdata-1:8020</value>
  </property>
  <!-- nn2 的 RPC 通信地址 -->
  <property>
    <name>dfs.namenode.rpc-address.mycluster.nn2</name>
    <value>bigdata-2:8020</value>
  </property>
  <!-- nn1 的 http 通信地址 -->
  <property>
	<!--Hadoop2中的端口默认是50070-->
    <name>dfs.namenode.http-address.mycluster.nn1</name>
    <value>bigdata-1:9870</value>
  </property>
  <!-- nn2 的 http 通信地址 -->
  <property>
	<!--Hadoop2中的端口默认是50070-->
    <name>dfs.namenode.http-address.mycluster.nn2</name>
    <value>bigdata-2:9870</value>
  </property>
  <!-- 访问代理类，用于确定当前处于 Active 状态的 NameNode -->
  <property>
    <name>dfs.client.failover.proxy.provider.mycluster</name>
    <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
  </property>
  <!-- NameNode 元数据在 JournalNode 上的共享存储目录 -->
  <property>
    <name>dfs.namenode.shared.edits.dir</name>
    <value>qjournal://bigdata-1:8485;bigdata-2:8485;bigdata-3:8485/mycluster</value>
  </property>
  <!-- Journal Edit Files 的存储目录 -->
  <property>
    <name>dfs.journalnode.edits.dir</name>
    <value>/opt/hadoop/ha/jnn/data</value>
  </property>
  <!-- 配置隔离机制，确保在任何给定时间只有一个 NameNode 处于活动状态 -->
  <property>
    <name>dfs.ha.fencing.methods</name>
    <value>sshfence</value>
  </property>
  <!-- 使用 sshfence 机制时需要 ssh 免密登录 -->
  <property>  
    <name>dfs.ha.fencing.ssh.private-key-files</name>
    <value>/root/.ssh/id_rsa</value>
  </property>
  <!-- SSH 超时时间 -->
  <property>  
    <name>dfs.ha.fencing.ssh.connect-timeout</name>
    <value>30000</value>
  </property>
  <!-- 开启NN故障自动转移 -->
  <property>
    <name>dfs.ha.automatic-failover.enabled</name>
    <value>true</value>
  </property>
</configuration>
```

配置hive时需要`core-site.xml`添加如下配置，不然`root`用户无法访问hive。
```xml
<property>
    <name>hadoop.proxyuser.root.hosts</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.root.groups</name>
    <value>*</value>
</property>
```

## 配置YARN高可用
修改`$HADOOP_HOME/etc/hadoop`下的配置文件
1. yarn-site.xml
```xml
<configuration>
    <!--配置 NodeManager 上运行的附属服务。需要配置成 mapreduce_shuffle 后才可以在 Yarn 上运行 MapReduce 程序。-->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <!-- 启用 RM HA -->
    <property>
       <name>yarn.resourcemanager.ha.enabled</name>
       <value>true</value>
    </property>
    <!-- ZooKeeper 集群的地址 -->
    <property>
        <name>yarn.resourcemanager.zk-address</name>
        <value>bigdata-2:2181,bigdata-3:2181,bigdata-4:2181</value>
    </property>
    <!-- RM 集群标识 -->
    <property>
        <name>yarn.resourcemanager.cluster-id</name>
        <value>my-yarn-cluster</value>
    </property>
    <!-- RM 的逻辑 ID 列表 -->
    <property>
        <name>yarn.resourcemanager.ha.rm-ids</name>
        <value>rm1,rm2</value>
    </property>
    <!-- RM1 的服务地址 -->
    <property>
        <name>yarn.resourcemanager.hostname.rm1</name>
        <value>bigdata-3</value>
    </property>
    <!-- RM2 的服务地址 -->
    <property>
        <name>yarn.resourcemanager.hostname.rm2</name>
        <value>bigdata-4</value>
    </property>
    <!-- 是否启用日志聚合 (可选) -->
    <property>
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
    </property>
    <!-- 聚合日志的保存时间 (可选) -->
    <property>
        <name>yarn.log-aggregation.retain-seconds</name>
        <value>86400</value>
    </property>
    <!-- RM1 Web 应用程序的地址 -->
    <property>
        <name>yarn.resourcemanager.webapp.address.rm1</name>
        <value>bigdata-3:8088</value>
    </property>
    <!-- RM2 Web 应用程序的地址 -->
    <property>
        <name>yarn.resourcemanager.webapp.address.rm2</name>
        <value>bigdata-4:8088</value>
    </property>
    <!-- 启用自动恢复 -->
    <property>
        <name>yarn.resourcemanager.recovery.enabled</name>
        <value>true</value>
    </property>
    <!-- 用于进行持久化存储的类 -->
    <property>
        <name>yarn.resourcemanager.store.class</name>
        <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
  </property>
</configuration>
```

2. mapred-site.xml
```xml
<configuration>
  <!--指定 mapreduce 作业运行在 yarn 上-->
  <property>
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
  </property>
  <property>
    <name>mapreduce.application.classpath</name>
    <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
  </property>
</configuration>
```

## 分发配置文件
将配置文件分发到所有Hadoop集群节点
```bash
root@bigdata-1:# cd $HADOOP_HOME/etc/hadoop
root@bigdata-1:# pdcp -w bigdata-[2-4] core-site.xml $PWD
root@bigdata-1:# pdcp -w bigdata-[2-4] hdfs-site.xml $PWD
root@bigdata-1:# pdcp -w bigdata-[2-4] yarn-site.xml $PWD
root@bigdata-1:# pdcp -w bigdata-[2-4] mapred-site.xml $PWD
```

## 第一次启动
1. 启动Zookeeper集群
```bash
root@bigdata-1:# pdsh -w bigdata-[2-4] "source /etc/profile;zkServer.sh start"
```
2. 在3台虚拟机上分别启动 `JournalNode`
```bash
root@bigdata-1:# pdsh -w bigdata-[1-3] "source /etc/profile;$HADOOP_HOME/sbin/hadoop-daemon.sh start journalnode"
```
3. 在`bigdata-1`格式化`NameNode`
```bash
root@bigdata-1:# $HADOOP_HOME/bin/hdfs namenode -format
```
4. 在`bigdata-1`启动`NameNode`
```bash
root@bigdata-1:# $HADOOP_HOME/sbin/hadoop-daemon.sh start namenode
```
5. 在`bigdata-2`同步`NameNode`元数据，因为`NameNode`是通过`JournalNode`通信的，所以先启动的`JournalNode`
```bash
root@bigdata-2:# $HADOOP_HOME/bin/hdfs namenode -bootstrapStandby
```
6. 在任意一台`NameNode`上使用以下命令来初始化 ZooKeeper 中的 HA 状态
```bash
root@bigdata-1:# $HADOOP_HOME/bin/hdfs zkfc -formatZK
```
7. 启动dfs
```bash
root@bigdata-1:# $HADOOP_HOME/sbin/start-dfs.sh
```
这里启动的时候会提示1个NameNode 和3个JournalNode 都已经启动了，不需要担心。等下次再启动的时候，直接来执行这个命令就可以了。
8. 在`bigdata-3`上启动YARN
```bash
root@bigdata-3:# $HADOOP_HOME/sbin/start-yarn.sh
```
9. 当前配置，YARN的standby节点不会自动启动，所以还需要手动启动。
```bash
root@bigdata-4:# $HADOOP_HOME/sbin/yarn-daemon.sh start resourcemanager
```

至此，Hadoop分布式高可用部署启动完成。
