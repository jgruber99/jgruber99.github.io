# Hadoop3单机伪分布式部署


## 背景

{{< admonition note "背景" true >}}
最近由于工作需要，转入大数据开发，由于之前只是片面的了解，近期开始系统的学习。
从Hadoop开始，之后会继续学习Hive、HBase、Storm、Spark、Flink、CarbonData等知识。
{{< /admonition >}}

## 目的

本篇文章介绍了如何设置和配置单节点Hadoop3安装，以便您可以使用Hadoop MapReduce和Hadoop分布式文件系统（HDFS）快速执行简单的操作。

## 前期准备

### 操作系统

Linux，CentOS7.4，关闭防火墙，或者打开对应端口

```bash
systemctl stop firewalld
systemctl disable firewalld
```

### 必要软件

在Linux操作系统中必须包含的软件：

1. 必须安装`Java`，推荐的Java版本描述位置在[ HadoopJavaVersions][1]。
2. 如果要使用启动和停止脚本，则必须安装`ssh`并且必须运行`sshd`才能使用管理远程Hadoop守护程序的脚本。

### 安装软件

如果你的环境没有安装必要的软件，则需要安装。

1. 安装`ssh`

```bash
yum install -y sshd
systemctl start sshd
```

- 配置ssh密钥，免密码登陆本服务器。
执行如下命令生成密钥：
```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
```
命令完成后会在`~/.ssh/`路径下生成两个文件，`id_rsa`密钥文件，`id_rsa.pub`对应的公钥文件。
将公钥文件的信息追加到`~/.ssh/authorized_keys`文件中，如果没有则新建。
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
之后要修改权限属性
```bash
chmod 0600 ~/.ssh/authorized_keys
```

- 验证是否配置成功
```bash
ssh localhost
```
第一次验证的时候会提示输入`yes`或`no`，直接输入`yes`即可。
如果输入了`yes`之后回车，可以进入，就表示配置成功。

2. 安装`Java`

在此我们安装`Java8`，下载地址使用[**清华大学开源软件镜像站**][2]。

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/8/jdk/x64/linux/OpenJDK8U-jdk_x64_linux_hotspot_8u282b08.tar.gz
```

安装Java并配置`JAVA_HOME`

```bash
mkdir -p /usr/local/java
tar -xzvf OpenJDK8U-jdk_x64_linux_hotspot_8u282b08.tar.gz -C /usr/local/java/
ln -sf /usr/local/java/jdk8u282-b08/ /usr/local/java/default
echo 'export JAVA_HOME=/usr/local/java/default' >> /etc/profile
echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile
source /etc/profile
```

## 下载Hadoop3

```bash
wget https://mirrors.bfsu.edu.cn/apache/hadoop/common/stable/hadoop-3.3.0.tar.gz
```

## 准备启动Hadoop

1. 解压下载的Hadoop压缩包到指定位置

```
mkdir -p /usr/local/hadoop
tar -xzvf hadoop-3.3.0.tar.gz -C /usr/local/hadoop
ln -sf /usr/local/hadoop/hadoop-3.3.0 /usr/local/hadoop/default
```

2. 配置`JAVA_HOME`
进入到Hadoop的根目录下，编辑环境变量配置文件`/etc/hadoop/hadoop-env.sh`
```bash
export JAVA_HOME=/usr/local/java/default
```

## 伪分布式运行（单节点运行）
### 配置
进入到`/usr/local/hadoop/default`目录下
1. 编辑`etc/hadoop/core-site.xml`文件
```xml
<configuration> 
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://localhost:9000</value>
	</property>
</configuration>
```

2. 编辑`etc/hadoop/hdfs-site.xml`文件
```xml
<configuration>
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
</configuration>
```
### 运行HDFS
1. 格式化
```bash
bin/hdfs namenode -format
```

2. 开启`NameNode`进程和`DataNode`进程
```bash
sbin/start-dfs.sh
```
运行此命令后，在`Hadoop2`时直接成功，在`Hadoop3`时报错。
`HDFS_NAMENODE_USER`和`HDFS_DATANODE_USER`和`HDFS_SECONDARYNAMENODE_USER`没有被定义，所以需要再去配置。
编辑`$HADOOP_HOME/libexec/hadoop-config.sh`，在文件的上方插入如下配置
```bash
HDFS_DATANODE_USER=root
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
```
重新执行启动dfs命令。
启动后，可以通过`Java`的`jps`命令，查看当前运行的Java进程，会看到
```bash
19056 DataNode
20466 Jps
18920 NameNode
19304 SecondaryNameNode
```
表示启动成功（进程ID不一定相同）。
3. 在浏览器上访问`NameNode`
> http://localhost:9870/
>
> ⚠️ 在Hadoop2中，访问地址是 http://localhost:50070
4. 在HDFS中创建目录
```bash
bin/hdfs dfs -mkdir /user
```
5. 停止HDFS进程
```bash
sbin/stop-dfs.sh
```
### 运行YARN
1. 编辑配置文件
`etc/hadoop/mapred-site.xml`
```xml
<configuration> 
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

`etc/hadoop/yarn-site.xml`
```xml
<configuration> 
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value> 
	</property>
	<property>
        <name>yarn.nodemanager.env-whitelist</name>
	<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>
```
2. 启动`ResourceManager`和`NodeManager`
```bash
sbin/start-yarn.sh
```
运行此命令后，在`Hadoop2`时直接成功，在`Hadoop3`时报错。
`YARN_RESOURCEMANAGER_USER`和`YARN_NODEMANAGER_USER`没有被定义，所以需要再去配置。
编辑`$HADOOP_HOME/libexec/yarn-config.sh`，在文件的上方插入如下配置
```bash
YARN_RESOURCEMANAGER_USER=root
YARN_NODEMANAGER_USER=root
```
重新执行启动yarn命令。
启动后，可以通过`Java`的`jps`命令，查看当前运行的Java进程，会看到
```bash
19056 DataNode
20466 Jps
18920 NameNode
19304 SecondaryNameNode
19999 ResourceManager
20143 NodeManager
```
表示启动成功（进程ID不一定相同）。
3. 在浏览器上访问`ResourceManager`
> http://localhost:8088/
4. 停止YARN进程
```bash
sbin/stop-yarn.sh
```

### TIPS
如果每次启动或者关闭都要执行两个脚本，太麻烦了，所以Hadoop自带了执行全部的脚本。
启动
```bash
sbin/start-all.sh
```
停止
```bash
sbin/stop-all.sh
```

[1]:	https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions "HadoopJavaVersions"
[2]:	https://mirrors.tuna.tsinghua.edu.cn
