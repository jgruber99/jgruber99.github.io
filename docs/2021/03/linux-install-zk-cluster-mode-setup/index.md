# Linux下部署Zookeeper


## 目的
本篇文章主要介绍如何部署Zookeeper和Zookeeper集群。

## 单机部署
### 准备环境
1. 一台Linux服务器，本实例是CentOS7.4。
2. 部署JDK，可以查看[Linux下安装JDK](https://brucemaa.cn/2021/03/linux-install-java8/)。
3. 去**[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn)**下载zookeeper
```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/stable/apache-zookeeper-3.5.9-bin.tar.gz
```

### 解压
1. 创建zookeeper放置的路径
```bash
mkdir -vp /usr/local/zookeeper
```
2. 解压到指定路径
```bash
tar -xzvf apache-zookeeper-3.5.9-bin.tar.gz -C /usr/local/zookeeper/
```
3. 配置软链接
```bash
ln -sf /usr/local/zookeeper/apache-zookeeper-3.5.9-bin /usr/local/zookeeper/default
```
### 配置
1. 配置环境变量，在`/etc/profile`文件添加如下配置
```bash
export ZOOKEEPER_HOME=/usr/local/zookeeper/default
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```
2. zookeeper配置文件
```bash
cp $ZOOKEEPER_HOME/conf/zoo_sample.cfg $ZOOKEEPER_HOME/conf/zoo.cfg
```
3. zoo.cfg
```Toml
# 每个刻度的毫秒数
tickTime=2000
# 初始的刻度数
# 同步阶段可能需要
initLimit=10
# 发送请求和获得确认之间可以确认身份的滴答声数量
syncLimit=5
# 快照存储的目录。
# 不要使用/ tmp进行存储，这里/ tmp只是示例。
dataDir=/opt/zookeeper/data
dataLogDir=/opt/zookeeper/logs
# 客户端将连接的端口
clientPort=2181
# 客户端连接的最大数量。
# 如果您需要处理更多客户，请增加此数量
#maxClientCnxns=60
#
# 在打开自动清除功能之前，请务必阅读管理员指南的维护部分。
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# 要保留在dataDir中的快照数
#autopurge.snapRetainCount=3
# 清除任务间隔（以小时为单位）
# 设置为“ 0”以禁用自动清除功能
#autopurge.purgeInterval=1

# 从3.5.0版本开始，zookeeper增加了AdminServer，提供HTTP接口 http://localhost:8080/commands
# Java系统变量：zookeeper.admin.enableServer
# 设置为“ false”以禁用AdminServer。 默认情况下，AdminServer是启用的。
admin.enableServer=true
# Java系统变量：zookeeper.admin.serverPort
# 嵌入式Jetty服务器侦听的端口。 默认为8080
admin.serverPort=8080
# Java系统变量：zookeeper.admin.commandURL
# 相对于根URL列出和发布命令的URL。 默认为“/commands”。
admin.commandURL=/commands
```
### 启动
```bash
zkServer.sh start
```
### 验证
```bash
zkServer.sh status
```
### 停止
```bash
zkServer.sh stop
```

## 集群部署
为了保证集群高可用，zookeeper集群的节点数量最好是奇数，最少要有3个节点。本篇文章使用3个服务器进行搭建，主机名分别为zoo1，zoo2，zoo3，并且在`/etc/hosts`文件中配置对应的IP。

以之前部署的单节点是在zoo1服务器上为例，首先确保zookeeper已经停止运行。
```bash
zkServer.sh stop
```

### 安装`pdsh`
`pdsh`的部署可以参考[这篇文章](https://brucemaa.cn/2021/03/linux-command-record-pdsh/)。
### 修改zookeeper的配置文件
1. 在`zoo.cfg`文件增加如下配置
```Toml
# server.1 这个1是服务器的标识，可以是1到255之间任意有效数字，不同节点不能相同，这个标识要写到dataDir目录下面myid文件里
# 指定集群间通讯端口和选举端口
server.1=zoo1:2888:3888
server.2=zoo2:2888:3888
server.3=zoo3:2888:3888
```

### 部署配置其他节点
1. 在其他节点创建目录
```bash
pdsh -w zoo[2,3] mkdir -vp /usr/local/zookeeper
```
2. 将配置好的zookeeper传输到其他节点
`pdcp`是安装`pdsh`时附带安装，用于传输文件。
```bash
pdcp -r -w zoo[2,3] /usr/local/zookeeper/apache-zookeeper-3.5.9-bin /usr/local/zookeeper/
```
3. 节点配置软链接
```bash
 pdsh -w zoo[2,3] ln -sf /usr/local/zookeeper/apache-zookeeper-3.5.9-bin /usr/local/zookeeper/default
```
4. 节点配置环境变量
```bash
pdcp -r -w zoo[2,3] /etc/profile /etc/profile
```
5. 配置`myid`
```bash
pdsh -w zoo[1-3] mkdir -vp /opt/zookeeper/data
pdsh -w zoo1 "echo 1 > /opt/zookeeper/data/myid"
pdsh -w zoo2 "echo 2 > /opt/zookeeper/data/myid"
pdsh -w zoo3 "echo 3 > /opt/zookeeper/data/myid"
```

### 启动集群
```bash
pdsh -w zoo[1-3] "source /etc/profile;zkServer.sh start"
```

### 查看集群状态
```bash
pdsh -w zoo[1-3] "source /etc/profile;zkServer.sh status"
```

