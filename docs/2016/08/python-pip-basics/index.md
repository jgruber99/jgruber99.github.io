# Python使用记录-pip基础命令


## pip基础命令记录--来源自[官方文档][pip.pypa]

### 在线安装第三方库

```
pip install SomePackage    ＃ 最新版本
pip install SomePackage==1.0.4    # 指定版本
pip install SomePackage>=1.0.4    # 最低限度版本
```
	
> 同时会安装依赖的其他第三方库

### 需要安装的列表文件

```
pip install -r requirements.txt
```

### 导出已安装的第三方库列表

```
pip freeze > ./requirements.txt
```

### 统一将第三方库安装到用户目录下

```
pip install --user SomePackage
```

### 移除第三方库

```
pip uninstall SomePackage
```

### 查看当前安装的第三方库

```
pip list
```

### 查询需要升级的库

```
pip list --outdated
```

### 升级库

```
pip install --upgrade SomePackage
```

### 查看库的详细信息

```
pip show SomePackage
```

### 查看库的安装信息

```
pip show --files SomePackage
```

### 在线查找第三方库

```
pip search "query"
```
	
	




[pip.pypa]: https://pip.pypa.io/en/stable/

