# Python使用记录-版本控制pyenv


## pyenv的使用记录

### pyenv的[仓库地址][pyenvGitHub]

```
https://github.com/yyuu/pyenv
```
	
### 配合``pyenv-virtualenv``创建虚拟环境

```	
https://github.com/yyuu/pyenv-virtualenv
```
	
### 安装pyenv

```
brew install pyenv
```

### 配置pyenv

#### 配置工作目录

```
export PYENV_ROOT=/usr/local/var/pyenv
```

#### 添加环境变量

```shell
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
```

### 安装pyenv-virtualenv

```
brew install pyenv-virtualenv
```

### 配置pyenv-virtualenv

#### 添加环境变量

```shell
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

### 重新加载SHELL环境，使环境变量生效

```
exec $SHELL
```
	
### 查看pyenv命令

```
pyenv commands
```

### 查看所有可安装Python版本

```
pyenv install --list
```

### 安装指定版本

```
pyenv install 2.7.12
```

### 本地指定使用已安装的版本

```
pyenv local 2.7.12
```
	
> 执行👆命令后，会在当前目录生成一个``.python-version``文件，用于指定当前目录中所使用的python版本。
> 
> 如果当前目录没有``.python-version``文件，则会向上级目录查找，当前目录所使用的python版本，与上级目录指定的版本相同。
> 
> 如果直到根目录都没有找到``.python-version``文件，则使用``global``设置的版本

### 指定全局版本

```
pyenv global system
```
	
> ``system``为系统默认python版本，非``pyenv``安装版本。
	
	
	
	
[pyenvGitHub]: https://github.com/yyuu/pyenv "仓库地址"
