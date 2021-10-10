# Gradle使用记录


## Gradle使用记录－－来自官网

### 安装

#### 1. 先决条件

> Gradle需要安装JDK或者JRE，版本为7及以上(可以通过``java -version``检查)。
> 
> 配置``JAVA_HOME``环境变量。

#### 2. 下载

	http://www.gradle.org/downloads

#### 3. 解压

#### 4. 配置环境变量

> 配置``GRADLE_HOME``，并且将``GRADLE_HOME/bin``加入到``PATH``中。

#### 5. 运行并测试安装是否成功

```shell
gradle -v
```

#### 6. 配置JVM参数

> 可以配置``GRADLE_OPTS``或者``JAVA_OPTS``，或者两个都可以。
> 
> 这些可以配置到环境变量中，也可以配置到``gradle``或者``gradlew``之前。

### 使用Gradle命令行

#### 1. 执行多个任务

> 创建文件``build.gradle``

```
	task compile << {
    println 'compiling source'
	}
	
	task compileTest(dependsOn: compile) << {
	    println 'compiling unit tests'
	}
	
	task test(dependsOn: [compile, compileTest]) << {
	    println 'running unit tests'
	}
	
	task dist(dependsOn: [compile, test]) << {
	    println 'building the distribution'
	}
```

> 保存退出后，执行命令

```shell
gradle dist test
```

#### 2. 拒绝执行任务

> 使用``-x``参数拒绝某个任务执行。

```shell
gradle dist -x test
```

> 你会看到不止test没有执行，test依赖的compileTest也没有执行。

#### 3. 某个任务失败也继续执行

> 如果使用了``--continue``参数，无论哪个任务失败，其余的任务都会执行。

#### 4. 任务缩略名

> 如果你不知道某个任务的全称，只知道简称也可以，不过简称必须是唯一的。

```shell
gradle di
gradle dist
```

> 上面两条命令会输出相同结果。
> 
> 如果要执行compileTest这个任务，下面的命令都可以

```shell
gradle compileTest
gradle compTest
gradle cT
```

#### 5. 指定执行的构建文件

> 当执行``gradle``命令时，看起来都是在当前路径构建。你可以使用``-b``参数执行构建文件。如果使用``-b``参数，则``settings.gradle``无效。
> 
> 当多个项目构建时，你可以使用``-p``参数代替``-b``参数。

```shell
gradle -q -p subdir hello
```

> ⚠️目前此命令我没有执行成功，还没有查明原因！！！

#### 6. 专注任务执行

> 没看懂

#### 7. 获得构建信息

##### 7.1 列出所选子项目的名单

```shell
gradle -q projects
```

##### 7.2 列出所选主任务的名单

```shell
gradle -q tasks
```

> 可以加上``--all``参数获得列出的任务里的更多信息。

##### 7.3 显示任务使用详细信息

```shell
gradle help --task someTask
```

##### 7.4 列出项目的依赖信息

```shell
gradle -q dependencies [subpro:dependencies]
```

> 由于依赖报告太多，可以使用``--configuration``参数来限制报告。

```shell
gradle -q api:dependencies --configuration testCompile
```

##### 7.5 列出项目构建脚本的依赖

```shell
gradle buildEnvironment
```
