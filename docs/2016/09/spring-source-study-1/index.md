# Spring源码深度解析－－学习笔记1


## Spring源码深度解析－－构建环境

> 本文是根据《Spring源码深度解析》（编著：郝佳）这本书记录的学习笔记。

### 必备环境变量

- Git
- Java
	- 必须是JDK8update20及更新
	- 必须配置``JAVA_HOME``到``jdk1.8.0``

### 下载源码

```shell
git clone https://github.com/spring-projects/spring-framework.git
```

### 导入到IDEA

> 需要先执行一条命令

```shell
./gradlew cleanIdea :spring-oxm:compileTestJava
```

> 接下来就是漫长的等待。
> 
> 根据打印的日志，以及读取``gradlew``命令文件可知，这是先去下载最新的``gradle``压缩包了，虽然我已经提前下载并且配置好了。
> 
> 下载完压缩包，然后是下载需要的jar包，再次漫长的等待后会看到``SUCCESS``。
> 
> 接下来执行命令

```shell
./gradlew install
```

> 再次漫长等待。。。
> 
> 成功后再次输入命令

```shell
./gradlew build
```

> 我的心，在等待，永远在等待。。。
> 
> 终于结束了。
> 
> 使用IDEA的``Import Project``导入项目，会提示``gradle build``，没关系，这次会快很多，因为先前已经把该下载的jar包都下载了。
> 
> 搞定。
