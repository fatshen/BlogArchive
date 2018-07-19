# centos6.8 上传文件到amazon s3

## 0.参考

[AWS CLI Cinnabd Reference](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html#directory-and-s3-prefix-operations)

[Possible to sync a single file with aws s3 sync? ](https://forums.aws.amazon.com/thread.jspa?threadID=151139)

[How to Install Python 2.7.15 on CentOS/RHEL 7/6 and Fedora 27/26/25](https://tecadmin.net/install-python-2-7-on-centos-rhel/)

[AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region)

[[Downloading folders from aws s3, cp or sync?](https://stackoverflow.com/questions/27932345/downloading-folders-from-aws-s3-cp-or-sync)](https://stackoverflow.com/questions/27932345/downloading-folders-from-aws-s3-cp-or-sync/27935645)

## 1、安装python 2.7 、 pip和aws工具

### 1.1 安装python 2.7

> pip is already installed if you are using Python 2 >=2.7.9 or Python 3 >=3.4 downloaded from [python.org](https://www.python.org/) or if you are working in a [Virtual Environment](https://packaging.python.org/tutorials/installing-packages/#creating-and-using-virtual-environments) created by [virtualenv](https://packaging.python.org/key_projects/#virtualenv) or [pyvenv](https://packaging.python.org/key_projects/#venv). Just make sure to [upgrade pip](https://pip.pypa.io/en/stable/installing/#upgrading-pip). 

因为pip只支持python 2.7，centos 6.8默认安装的python版本比较旧，需要安装新版本的python，方法详细见参考链接。

### 1.2 安装 pip

```shell
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
sudo python2.7 get-pip.py
```

不指定python版本的话，会有以下出错有类似以下出错:

```
Traceback (most recent call last):
  File "/usr/bin/pip", line 9, in <module>
    load_entry_point('pip==10.0.0b2', 'console_scripts', 'pip')()
  File "/usr/lib/python2.6/site-packages/pkg_resources.py", line 299, in load_entry_point
    return get_distribution(dist).load_entry_point(group, name)
  File "/usr/lib/python2.6/site-packages/pkg_resources.py", line 2229, in load_entry_point
    return ep.load()
  File "/usr/lib/python2.6/site-packages/pkg_resources.py", line 1948, in load
    entry = __import__(self.module_name, globals(),globals(), ['__name__'])
  File "/usr/lib/python2.6/site-packages/pip-10.0.0b2-py2.6.egg/pip/_internal/__init__.py", line 42, in <module>
    from pip._internal import cmdoptions
  File "/usr/lib/python2.6/site-packages/pip-10.0.0b2-py2.6.egg/pip/_internal/cmdoptions.py", line 16, in <module>
    from pip._internal.index import (
  File "/usr/lib/python2.6/site-packages/pip-10.0.0b2-py2.6.egg/pip/_internal/index.py", line 526
    {str(c.version) for c in all_candidates},
                      ^
SyntaxError: invalid syntax
```

### 1.3 安装awscli

```shell
python2.7 /usr/local/bin/pip install awscli  --upgrade --user
```

因为默认的python还是用旧版本，如果直接用pip install awscli ，会有类似以下的出错：

```
/usr/local/bin/pip: line 4: import: command not found
/usr/local/bin/pip: line 5: import: command not found
/usr/local/bin/pip: line 7: from: command not found
/usr/local/bin/pip: line 10: syntax error near unexpected token `('
/usr/local/bin/pip: line 10: `    sys.argv[0] = re.sub(r'(-script\.pyw?|\.exe)?$', '', sys.argv[0])'
```



## 2. 利用aws上传到s3

### 2.1 输入aws  configure进行配置 

```
AWS Access Key ID [None]: XXXXXXX
AWS Secret Access Key [None]: XXXXXXXXXXXXXXXXXXXXXXXX
Default region name [None]: us-east-2
Default output format [None]: text
```

Access Key和Secret Access Key存在~/.aws/config和~/.aws/credentials，必要时可以用vim打开查看，避免因为拷贝了特殊字符，而产生以下出错：

```
fatal error: An error occurred (InvalidAccessKeyId) when calling the ListObjects operation: The AWS Access Key Id you provided does not exist in our records.
```

region字段必须填写，否则会出现以下报错：

```
fatal error: Could not connect to the endpoint URL: "https://xxx.s3.None.amazonaws.com/?prefix=&encoding-type=url"
```

region具体填写内容可以参考s3 console里的链接，比如登录s3 console，链接比是:https://s3.console.aws.amazon.com//xxxx/xxxx/?region=us-east-2，region就填写:us-east-2。region具体内容见参考链接。

### 2.2 用户环境配置

需要把aws加到PATH，如把以下加到~/.bashrc:

```
export PATH=~/.local/bin:$PATH
```

如果root也要需要执行aws，则root的.bashrc也要添加上述语句。

### 2.3 使用sudo相关配置

因为sudo会出剔除一些额外添加的PATH，所以在脚本中添加aws命令，再用sudo使用，会有以下报错：

```
Traceback (most recent call last):
  File "/home/xxx/.local/bin/aws", line 19, in <module>
    import awscli.clidriver
ImportError: No module named awscli.clidriver
```

所有在sudo脚本中需要执行以下两个export:

```shell
export PATH=/home/xxx/.local/bin:$PATH
export PYTHONPATH=/home/xxx/.local/lib/python2.7/site-packages:$PYTHONPATH
...
aws s3 cp xxx.py s3://xxx/xxx/
```

## 3 使用方法

```
# 同步目录
aws s3 sync . s3://xxx
# 同步文件
aws s3 cp xx s3://xx
# 同步文件到子目录，最后一定要加上"/"
aws s3 cp xx s3://xx/xx/
```

其它命令使用方法见参考链接。

