# Hadoop3集群分布式部署


## 目的
本篇文章介绍了如何安装和配置Hadoop集群，范围从几个节点到几千个节点的超大集群。要使用Hadoop，您可能需要先在单机上安装（请参考[ 单节点安装 ](https://brucemaa.cn/2021/03/hadoop3-standalone-pseudo-distributed-mode-setup/)）。
本篇文章不涉及安全或高可用性等高级主题。
## 准备环境
3台Linux服务器（CentOS7 64位）。
### 配置`host`

在三个服务器中，分别编辑`/etc/hosts`文件，下面为此次示例IP：
```bash
10.4.53.78 bigdata-1
10.4.53.79 bigdata-2
10.4.53.80 bigdata-3
```

### 计划分配
将`bigdata-1`做为主节点，运行`NameNode`和`ResourceManager`，将`bigdata-2`和`bigdata-3`做为从节点，运行`DataNode`和`DataManager`。

### 安装pdsh
`pdsh` 的全称是`parallel distributed shell`，可以在多台服务器上一起执行shell命令。`pdsh`使用前要配置`ssh免密登陆`。
```bash
yum install pdsh
```
如果安装失败，没有找到yum源，查看当前yum源里是否有`epel`，如果没有则需要安装。
```bash
yum repolist
```

### 配置SSH免密登陆
在`bigdata-1`上生成密钥：
```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
```
将生成的公钥同步到`bigdata-2`和`bigdata-3`上
```bash
ssh-copy-id bigdata-2
ssh-copy-id bigdata-3
```
执行同步时，要输入密码确认，执行成功后，则可免密登陆
```bash
ssh bigdata-2
ssh bigdata-3
```

## 配置Hadoop集群
在`bigdata-1`服务器上操作，根据[单节点安装](https://brucemaa.cn/2021/03/hadoop3-standalone-pseudo-distributed-mode-setup/#%E4%B8%8B%E8%BD%BDhadoop3)中下载解压Hadoop3。
### 配置Hadoop环境变量
1. `$HADOOP_HOME/etc/hadoop/hadoop-env.sh`
```bash
# 配置JAVA_HOME
export JAVA_HOME=/usr/local/java/default
```
### 配置dfs文件
1. `$HADOOP_HOME/etc/hadoop/core-site.xml`
```xml
<configuration>
    <property>
        <!--指定 namenode 的 hdfs 协议文件系统的通信地址-->
        <name>fs.defaultFS</name>
        <value>hdfs://bigdata-1:9000</value>
    </property>
    <property>
        <!--指定 hadoop 集群存储临时文件的目录-->
        <name>hadoop.tmp.dir</name>
        <value>/opt/hadoop/tmp</value>
    </property>
</configuration>
```
2. `$HADOOP_HOME/etc/hadoop/hdfs-site.xml`
```xml
<configuration>
  <property>
<!--datanode 节点数据（即数据块）的数量-->
    <name>dfs.replication</name>
    <value>2</value>
  </property>
  <property>
<!--SecondaryNameNode的地址-->
    <name>dfs.namenode.secondary.http-address</name>
    <value>bigdata-1:9868</value>
  </property>
  <property>
<!--namenode 节点数据（即元数据）的存放位置，可以指定多个目录实现容错，多个目录用逗号分隔-->
    <name>dfs.namenode.name.dir</name>
    <value>/opt/hadoop/dfs/name</value>
  </property>
  <property>
<!--datanode 节点数据（即数据块）的存放位置-->
    <name>dfs.datanode.data.dir</name>
    <value>/opt/hadoop/dfs/data</value>
  </property>
</configuration>
```
3. `$HADOOP_HOME/libexec/hadoop-config.sh`
```bash
# 以下在Hadoop3中必须配置，否则启动失败，在Hadoop2中无需配置。
HDFS_DATANODE_USER=root
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
```

### 配置yarn文件
1. `$HADOOP_HOME/etc/hadoop/yarn-site.xml`
```xml
<configuration>
<!--配置 NodeManager 上运行的附属服务。需要配置成 mapreduce_shuffle 后才可以在 Yarn 上运行 MapReduce 程序-->
 <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.nodemanager.env-whitelist</name>
    <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
  </property>
  <property>
<!--resourcemanager 的主机名-->
    <name>yarn.resourcemanager.hostname</name>
    <value>bigdata-1</value>
  </property>
</configuration>
```
2. `$HADOOP_HOME/etc/hadoop/mapred-site.xml`
```xml
<configuration>
<!--指定 mapreduce 作业运行在 yarn 上-->
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
  <property>
<!--制定 mapreduce 作业运行时依赖的classpath路径-->
    <name>mapreduce.application.classpath</name>
    <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
  </property>
</configuration>
```
3. `$HADOOP_HOME/libexec/yarn-config.sh`
```bash
# 以下在Hadoop3中必须配置，否则启动失败，在Hadoop2中无需配置。
YARN_RESOURCEMANAGER_USER=root
YARN_NODEMANAGER_USER=root
```

### 配置从节点信息
1. `$HADOOP_HOME/etc/hadoop/workers`
> ⚠️ Hadoop2里的文件名为slaves
```xml
bigdata-2
bigdata-3
```

## 分发Hadoop程序
使用`pdcp`将`bigdata-1`服务器上所有的配置分发到其他服务器，`pdcp`是安装`pdsh`时附带安装的命令，用来传输文件。
```bash
# 分别创建java目录
pdsh -w bigdata-[2-3] mkdir /usr/local/java
# 分别创建hadoop目录
pdsh -w bigdata-[2-3] mkdir /usr/local/hadoop
# 并行传输Hadoop，因为是个目录，所以加 -r
pdcp -r -w bigdata-[2-3] /usr/local/hadoop/hadoop-3.3.0 /usr/local/hadoop/
# 并行传输Java
pdcp -r -w bigdata-[2-3] /usr/local/java/jdk8u282-b08 /usr/local/java/
# 并行传输hosts文件
pdcp -w bigdata-[2-3] /etc/hosts /etc/hosts
# 并行传输环境变量配置文件
pdcp -w bigdata-[2-3] /etc/profile /etc/profile
# 使环境变量生效
pdsh -w bigdata-[2-3] source /etc/profile
```

## 格式化
在`bigdata-1`上执行namenode格式化命令：
```bash
$HADOOP_HOME/bin/hdfs namenode -format
```

## 启动集群
1. 在`bigdata-1`上执行启动hdfs命令：
```bash
$HADOOP_HOME/sbin/start-dfs.sh
```
启动成功后，执行`jps`命令查看启动的进程，可以在`bigdata-1`上看到`NameNode`和`SecondaryNameNode`进程，在`bigdata-2`和`bigdata-3`上看到`DataNode`。

2. 在`bigdata-1`上执行启动yarn命令：
```bash
$HADOOP_HOME/sbin/start-yarn.sh
```
启动成功后，执行`jps`命令查看新增的进程，可以在`bigdata-1`上看到新增了`ResourceManger`进程，在`bigdata-2`和`bigdata-3`上新增了`NodeManager`进程。

### 其他命令
1. 停止hdfs命令
```bash
$HADOOP_HOME/sbin/stop-dfs.sh
```
2. 停止yarn命令
```bash
$HADOOP_HOME/sbin/stop-yarn.sh
```
3. 启动全部(依次启动hdfs和yarn)
```bash
$HADOOP_HOME/sbin/start-all.sh
```
4. 停止全部(依次停止hdfs和yarn)
```bash
$HADOOP_HOME/sbin/stop-all.sh
```

## Web UI

1. ResourceManager访问地址
> http://bigdata-1:8088

2. NameNode访问地址
> http://bigdata-1:9870
> 
> ⚠️ 在Hadoop2中，访问地址是 http://bigdata-1:50070

3. SecondaryNameNode访问地址
> http://bigdata-1:9868
> 
> ⚠️ 在Hadoop2中，访问地址是 http://bigdata-1:50090

4. NodeManager访问地址
> http://bigdata-2:8042

5. DataNode访问地址
> http://bigdata-2:9864
> 
> ⚠️ 在Hadoop2中，访问地址是 http://bigdata-2:50075
