# Linux记录之安装JDK


## 目的
本篇文章介绍如果在Linux操作系统`CentOS7.4`中部署配置`JDK1.8`。

## 准备环境
Linux操作系统：CentOS7.4
### 下载JDK
下载地址在[**清华大学开源软件镜像站**](https://mirrors.tuna.tsinghua.edu.cn)中查找，使用`wget`命令下载
```bash
wget https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/8/jdk/x64/linux/OpenJDK8U-jdk_x64_linux_hotspot_8u282b08.tar.gz
```

## 部署并配置JDK
1. 创建目录
```bash
mkdir -p /usr/local/java
```
2. 解压到创建的目录
```bash
tar -xzvf OpenJDK8U-jdk_x64_linux_hotspot_8u282b08.tar.gz -C /usr/local/java/
```
3. 创建软连接目录
```bash
ln -sf /usr/local/java/jdk8u282-b08/ /usr/local/java/default
```
4. 配置环境变量`JAVA_HOME`
```bash
echo 'export JAVA_HOME=/usr/local/java/default' >> /etc/profile
```
5. 将JDK命令添加到`PATH`环境变量中
```bash
echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile
```
6. 使之前的配置生效
```bash
source /etc/profile
```

## 验证
在任意目录下执行如下命令
```bash
java -version
```

