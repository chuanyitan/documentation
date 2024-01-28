# repo多仓库管理工具 #

[toc]

`Android`使用`Git`作为基础的代码管理工具。`Google`开发了`Gerrit`进行代码审核以便更好的对代码进行集中式管理，还开发了`repo`命令行工具，对Git部分命令进行封装，用于管理`Android`项目，可将百多个`Git`仓库有效的进行组织。`repo`并不是用来取代Git，而是用`Python`对`Git`进行了一定的封装，简化了对多个`Git`版本仓库的管理。

参考资料：

- repo仓库管理： https://gitee.com/simpost/repo_tutorial （本文档摘自此处）
- Git多项目管理：https://www.jianshu.com/p/284ded3d191b
- Git-Repo多仓库管理：https://www.jianshu.com/p/eaf783b3230c

## 一、`repo`安装

`repo`工具的`git-repo`仓库获取方式为：

```
git clone https://github.com/esrlabs/git-repo.git
```

1. 获取`git-repo`源码到本地路劲，比如`/opt/git-repo`；
2. 拷贝`git-repo`源码中的repo工具到`/home/user/bin/rep目三、录，并赋予可执行权限；
3. 修改`/home/user/bin/repo`脚本中的`REPO-URL`链接指向本地的`/opt/git-repo`即可正常使用了

### 1.1 repo的国内镜像安装

<font color="#dd0000" size="4">参考Gitee repo官方工具使用方法 ，直接参考该文档 [Repo 工具使用介绍](https://help.gitee.com/enterprise/code-manage/%E9%9B%86%E6%88%90%E4%B8%8E%E7%94%9F%E6%80%81/Repo%20%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E4%BB%8B%E7%BB%8D)， 前题需要配置好python的环境， 安装好python3的环境，并配置好默认python使用的python3工具。</font><br />


## 二、repo命令使用

### 2.1 初始化

```
mkdir repo_dir && cd repo_dir
repo init -u url -m manifest-file
```

`repo init`命令会在当前目录下安装`repository`，并在当前目录创建一个目录：`.repo`；`-u`参数指定一个`URL`，`repo`将从这个URL指定的`repository`中取得`manifest`文件；`-m`参数可以选择`repository`中的某一个特定的`manifest`文件，如果不指定，则为默认的`default.xml`文件。

### 2.2 同步git仓库

```
repo sync
repo sync --force-sync # 强制同步整个仓库
```

### 2.3 将所有repo控制的git仓库`checkout`到远程`master`分支

```
repo forall -c git checkout -b master remotes/origin/master
```

### 2.4 清除并还原本地修改

```
repo forall -c 'git clean -df; git checkout .'
```

### 2.5 查看工程中所有仓库的修改状态

```
repo status
```

### 2.6 将某个时间段所有仓库修改记录显示出来

```
repo forall -c git log --name-status --since="2017-02-12" --until="2017-03-01"
```

### 2.7 上传修改

```
repo upload [project-list]
```

如上命令用于上传修改的代码。如果你本地的代码有所修改，那么在运行`repo sync`的时候，会提示你上传修改的代码，所有修改的代码分支会上传到`Gerrit`上，`Gerrit`受到上传的代码，会转换为一个个变更，从而可以让人们来`review`修改的代码。

### 2.8 查看差异

```
repo diff [project-list]
```

用于显示提交的代码和当前工作目录代码之间的差异。

### 2.9 执行command命令

```
repo forall -c command
```

对所有的项目执行一个`command`命令，这个命令相当好用。

## 三、配置文件详解

配置文件以下面语句开头：

```
<?xml version="1.0" encoding="UTF-8"?>
```

整个文档由`manifest`标签包裹，它是配置的顶层元素：

```
<manifest>
...
</manifest>
```

其中主要包含`remote`、`default`和`project`三个元素：

- `remote`元素用于设定远程服务器的属性，可以为多个，其相关属性如下：
    - `name`属性用于设置远程服务器名，用于`git fetch`、`git remote`等操作；
    - `alias`属性可以覆盖之前定义的`remote name`，`name`必须是固定的，但是`alias`可以不同，可以用来指向不同的`remote url`；
    - `fetch`属性是所有`project`的`git url`的前缀；
    - `review`属性用于指定`gerrit`服务器，用于`repo upload`操作；

```
<remote name="origin" fetch="gerrit.dd.net" review="http://gerrit.dd.net"/>
```

- `default`元素用于设定所有`project`的默认属性值：
    - ·属性用于指定为哪个`remote`元素设定默认`project`属性值；
    - `revision`属性你用于设置git仓库的分支名，如`master`或`refs/heads/master`；
    - `sync-j`属性设定sync操作时的并行线程数；
    - `sync-c`：如果设置为true，则只同步指定的分支(`revision` 属性指定)，而不是所有的ref内容；
    - `sync-s`：如果设置为true，则会同步git的子项目；

```
<default revision="master" remote="origin" sync-j="4" />
```

- `project`元素用于指定要`clone`的`git`仓库：
    - `name`属性与`remote`的`fetch`属性一起拼接成项目的`git`仓库的`url`；
    - `path`属性指定`clone`出来的git仓库在本地的路径，如果没有配置则与`name`一样；
    - `remote`：定义`remote name`，如果没有定义的话就用`default`中定义的`remote name`；
    - `resivion`属性用于指定获取git的提交点，可以是固定的`branch`，也可以是明确的`commit`哈希值；
    - `groups`：列出project所属的组，以空格或者逗号分隔多个组名；所有的`projec`t都自动属于"all"组；每一个`project`自动属于`name:'name'`和`path:'path'`组；例如，它自动属于`default,name:monkeys,and path:barrel-of`组；如果一个`project`属于`notdefault`组，则：`repo sync`时不会下载；
    - `sync_c`：如果设置为true，则只同步指定的分支(`revision` 属性指定)，而不是所有的ref内容；
    - `sync_s`： 如果设置为true，则会同步git的子项目；
    - `upstream`：在哪个git分支可以找到一个`SHA1`；用于同步`revision`锁定的`manifest`(`-c`模式)；该模式可以避免同步整个ref空间；
    - `annotation` ：可以有0个或多个`annotation`，格式是`name-value`，`repo forall`命令是会用来定义环境变量；

```
<project path="fanxiao/fanxiaotest1" name="MA/Application/app-a" revision="master" />
<project path="fanxiao/fanxiaotest2" name="MA/Application/app-b" revision="52cf9185ff1d" />
<project path="fanxiao/fanxiaotest3" name="fanxiaotest" revision="master"/>
```
- `include`元素：
    - `name`：另一个需要导入的`manifest`文件名字；可以在当前的路径下添加一个`another_manifest.xml`，这样可以在另一个`xml`中添加或删除`project`；
- `remove-project`：从内部的`manifest`表中删除指定的`project`；经常用于本地的`manifest`文件，用户可以替换一个`project`的定义。


有时候`manifest`中还会包含`manifest-server`元素。它的`url`属性用于指定`manifest`服务的`URL`，通常是一个`XML RPC`服务。它要支持一下`RPC`方法：
- `GetApprovedManifest`(`branch`, `target`)：返回一个`manifest`用于指示所有`projects`的分支和编译目标；`target`参数来自环境变量`TARGET_PRODUCT`和`TARGET_BUILD_VARIANT`，组成`$TARGET_PRODUCT-$TARGET_BUILD_VARIANT`
- `GetManifest(tag)` ：返回指定tag的`manifest`

## 四、常见问题

### 4.1 如何指定xml配置文件？

在`repo init`命令时，通过`-m name.xml`来指定`manifest`的名字，比如：

```
repo init -u https://github.com/AaronKonishi/repo-study.git -m manifest.xml
```

当不使用`-m`指定`manifest`时，默认使用`default.xml`。

### 4.2 如何指定固定分支？

在`default`元素中，可以为`remote`指定默认的属性，其中的`revision`可以指定默认的分支：

```
<default revision="master" remote="origin" sync-j="4" />
```

在具体的`project`元素中，也可以使用`revision`指定固定的分支，分支可以是`tag`、`branch`，或者某次固定的提交：

```
<project path="fanxiao/fanxiaotest1" name="MA/Application/app-a" revision="master" />
<project path="fanxiao/fanxiaotest2" name="MA/Application/app-b" revision="52cf9185ff1d" />
```
### 4.3 如何指定多个远程？

可以在`*.xml`中指定多个`remote`元素，但是只能有一个`default`元素，然后在`project`元素中，通过`remote`属性指定要使用哪一个`remote`的`fetch`。比如：

```
<?xml version="1.0" encoding="UTF-8"?>

<manifest>
<remote name="gitee" fetch="https://gitee.com/simpost/" />
<remote name="github" fetch="https://github.com/AaronKonishi/" />
<default revision="master" remote="github" />
<project path="source/git-repo" name="git-repo" remote="github" />
<project path="source/repo-study" name="repo-study" remote="github" />

<project path="source/EFSM" name="EFSM" remote="gitee" revision="master" />
</manifest>
```
