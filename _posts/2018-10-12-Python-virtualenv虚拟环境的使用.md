---
layout:     post
title:      "使用virtualenv搭建Python虚拟环境"
subtitle:   "virtualenv, virtualenvwrapper"
date:       2018-10-12 12:00:00
author:     "baiyf"
header-img: "img/wooden.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Tools
---

#  使用virtualenv搭建Python虚拟环境

工作的时候，经常会需要用到一些包的不同版本，总是uninstall/install效率必然很低，所以需要使用`virtualenv`来隔离python环境，解决包冲突的问题

下面以`centos`系统为例介绍一下`virtualenv`及其管理工具`virtualenvwrapper`的使用方法

## virtualenv

### 安装

可以使用pip安装也可以使用yum安装

```shell
yum install python-virtualenv
# 或
pip install virtualenv
```

### 创建虚拟环境

使用virtualenv命令创建虚拟环境：`virtualenv [虚拟环境名]`

```shell
mkdir pyenv #创建虚拟环境仓库
cd pyenv
virtualenv --python /usr/local/python work
```

执行后会在本地产生一个同名文件夹 ，`--python`参数用于指定使用的python版本

另外，可以使用`--system-site-packages`参数来之定是哟个系统环境下的global site packags

```shell
virtualenv --system-site-packages work
```

### 启动与退出虚拟环境

```shell
cd work/
source bin/activate
```

此时命令行前面会出现一个小括号`(work)`，表示进入了`work`虚拟环境

退出虚拟环境

```shell
deactivate
```

## virtualenvwrapper

`virtualenvwrapper`是一种`virtualenv`虚拟环境的管理工具

### 安装

安装同样有两种方式

```shell
yum install python-virtualenvwrapper
# 或
pip install virtualenvwrapper
```

### 配置

查找这个配置脚本的位置，因为你环境里python的安装目录很可能与网上的教程不同

```shell
find / -name virtualenvwrapper.sh
```

设置环境变量，把下面两行加到`~/.bashrc`里

```shell
export WORKON_HOME=/home/pyenv #虚拟环境目录
source /**上一步找到的path**/virtualenvwrapper.sh
```

到这里virtualenvwrapper的配置就完成了，可以开始检验一下了~

### 虚拟环境管理

**创建虚拟环境：**`mkvirtualenv [虚拟环境名]`

```shell
mkvirtualenv paddle
mkvirtualenv tensorflow
```

mkvirtualenv同样使用`—python`参数来之定python版本

**列出所有虚拟环境：**

```shell
lsvirtualenv -b
```

**启动/切换虚拟环境：**

```python
workon paddle
workon tensorflow
```

**进入到当前虚拟环境目录：**

```shell
cdvirtualenv
```

**删除虚拟环境：**

```shell
rmvirtualenv paddle
rmvirtualenv tensorflow
```

**复制虚拟环境：**

```shell
cpvirtualenv work develop # 复制work为develop
```

`lssitepackages`列出当前环境所有site-packages内容，`cdsitepackages`清楚环境内所有第三方包