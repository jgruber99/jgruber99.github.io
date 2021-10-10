# Linux并行管理工具pdsh


## 背景
在做大数据平台部署，或者集群部署时，通常需要多个服务器部署相同组件，并且相同配置。之前都是在其中一个服务器上操作完成后，使用`scp`命令将配置好的组件打包串行（one by one）传输到其他的服务器上，然后再去对应的服务器上解压。或者使用`iTerm2`这类工具快捷键`⇧ ⌘ i`实现多窗口同时输入。

## 目的
`pdsh`是一个并行分布式shell(_**Parallel Distributed shell**_)，可以同时在多个Linux服务器上执行相同shell命令，并且安装后还附带了`pdcp`命令，可以同时将文件传输给多台服务器。
因为`pdsh`默认使用`ssh`远程，所以使用之前必须至少保证操作的服务器对远程的服务器可以免密登陆，具体的配置可以参考[这篇文章](https://brucemaa.cn/2017/04/linux-command-record-ssh)。

## 安装
### yum安装
使用`yum`安装之前，要确保已经安装了`epel`源，否则会找不到。
```bash
yum install epel-release
```
然后直接安装即可
```bash
yum install pdsh
```
### 编译安装
源码地址：[https://github.com/chaos/pdsh](https://github.com/chaos/pdsh)

## 使用
1. pdsh
```bash
# pdsh -h
Usage: pdsh [-options] command ...
-S                return largest of remote command return values
-h                输出帮助菜单并退出
-V                输出版本信息并退出
-q                列出配置信息
-b                按下 ^C 立即停止所有并行的任务
-d                开启额外的调试信息
-l user           执行远程命令的用户（默认当前登陆用户）
-t seconds        设置超时时间（默认10秒）
-u seconds        设置命令超时时间
-f n              同时远程目标节点的个数
-w host,host,...  指定远程命令的目标节点列表
-x host,host,...  排除远程命令的目标节点列表
-R name           执行rcmd模块的名字，默认ssh
-M name,...       选择一个或多个misc模块来先初始化
-N                返回信息中不现实hostname标签
-L                列出所有加载的模块信息并退出
available rcmd modules: ssh,exec (default: ssh)
```

2. pdcp
```bash
# pdcp -h
Usage: pdcp [-options] src [src2...] dest
-r                递归复制文件
-p                保留修改时间和权限模式
-e PATH           指定远程计算机上pdcp的路径
-h                输出帮助菜单并退出
-V                输出版本信息并退出
-q                列出配置信息
-b                按下 ^C 立即停止所有并行的任务
-d                开启额外的调试信息
-l user           执行远程命令的用户（默认当前登陆用户）
-t seconds        设置超时时间（默认10秒）
-u seconds        设置命令超时时间
-f n              同时远程目标节点的个数
-w host,host,...  指定远程命令的目标节点列表
-x host,host,...  排除远程命令的目标节点列表
-R name           执行rcmd模块的名字，默认ssh
-M name,...       选择一个或多个misc模块来先初始化
-N                返回信息中不现实hostname标签
-L                列出所有加载的模块信息并退出
available rcmd modules: ssh
```

## 例子
1. 查看各个服务器的时间
```bash
pdsh -w bigdata-[1-4] date
```
2. 查看各个服务器的Java版本
```bash
pdsh -w bigdata-[1-4] "source /etc/profile;java -version"
```
3. 传输文件到各个服务器
```bash
pdcp -w bigdata-[1-4] /etc/profile /etc/profile
```
4. 传输目录到各个服务器
```bash
pdcp -r -w bigdata-[1-4] /path/ /path/
```

## misc
1. 配置machines
```bash
export WCOLL=/opt/pdsh/nodes
```
在`/opt/pdsh/nodes`文件中添加其他服务器地址，即可省略`-w` 参数。
2. 配置group
```bash
export DSHGROUP_PATH=/opt/pdsh/group
```
`/opt/pdsh/group`是个目录，在目录中可以添加多个文件，用于远程服务器的分组，执行命令时通过`-g`参数指定文件名，就会在对应的服务器组执行命令。（编译的时候需要添加group的misc）

