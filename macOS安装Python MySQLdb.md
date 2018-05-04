# macOS安装Python MySQLdb

## 0. 参考

[Mac OS X - EnvironmentError: mysql_config not found](https://stackoverflow.com/questions/25459386/mac-os-x-environmenterror-mysql-config-not-found)



## 1. 背景

```python
import MySQLdb
```

出错:

```shell
ImportError: No module named MySQLdb
```



##1. 解决步骤

###1.1 安装mysql

```shell
brew install mysql
```

把以下export加到~/.bash_profile

```shell
export PATH="$PATH:/usr/local/bin/mysql"
```

### 1.2 安装pip

```shell
sudo easy_install pip
```

###1.3 安装MySQL-Python

```
sudo pip install MySQL-Python
```



