# Gitee usage

[toc]

## 1 Gitee Key manage

### 1.1.单key管理 
如果需要在新的服务器上下载新的仓库内容或提交代码及内容，需要在自己的服务器上生成ssh公钥，并添加到自己的账户当中。具体可以参考: [key manage](https://help.gitee.com/enterprise/code-manage/%E6%9D%83%E9%99%90%E4%B8%8E%E8%AE%BE%E7%BD%AE/%E9%83%A8%E7%BD%B2%E5%85%AC%E9%92%A5%E7%AE%A1%E7%90%86/%E7%94%9F%E6%88%90%E6%88%96%E6%B7%BB%E5%8A%A0SSH%E5%85%AC%E9%92%A5)

```bash
1. sshkey生成密匙, 这里的 xxxxx@xxxxx.com 只是生成的 sshkey 的名称，并不约束或要求具体命名为某个邮箱
ssh-keygen -t ed25519 -C "tanchuanyi@qq.com"  
# Generating public/private ed25519 key pair...

2. 完成三次回车，即可生成 ssh key。通过查看 ~/.ssh/id_ed25519.pub 文件内容，获取到你的 public key
cat ~/.ssh/id_ed25519.pub
# ssh-ed25519 AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....

3. 部署到公钥中
复制生成后的 ssh key，登录gitee, 进到自己的主页的设置中， 通过仓库主页「仓库设置」->「部署公钥管理」->「添加部署公钥」 ，添加生成的 public key 添加到仓库中。

4. 添加本机SSH可信列表
在终端（Terminal）中输入：
ssh -T git@gitee.com

首次使用需要确认并添加主机到本机 SSH 可信列表。若返回 Hi XXX! You've successfully authenticated, but Gitee.com does not provide shell access. 内容，则证明添加成功。
```

### 1.2 多key管理
需要有多个账号，或者与平时其他的账号隔离开可以参考: [mutil key manange](https://help.gitee.com/enterprise/code-manage/%E6%9D%83%E9%99%90%E4%B8%8E%E8%AE%BE%E7%BD%AE/%E9%83%A8%E7%BD%B2%E5%85%AC%E9%92%A5%E7%AE%A1%E7%90%86/Git%E9%85%8D%E7%BD%AE%E5%A4%9A%E4%B8%AASSH-Key)

1. 与上面的指令类似

```bash
 ssh-keygen -t ed25519 -C "Gitee aaa outlook" -f ~/.ssh/gitee_aaa_outlook_ed25519

将~/.ssh/gitee_aaa_outlook_ed25519.pub内容加入到网页中的设置的公共key管理中。

 vi ~/.ssh/config

Host gitee
    User git
    Hostname gitee.com
    Port 22
    IdentityFile ~/.ssh/gitee_aaa_outlook_ed25519

ssh -T gitee
# Hi aaa(@aaa)! You've successfully authenticated, but GITEE.COM does not provide shell access.

```

### Note
```
拉代码，提交操作

git clone git@gitee.com:tanchuanyi/documentation.git
or git clone gitee:tanchuanyi/documentation.git
git commit -s  #编辑修改内容     
git commit --amend #修改上一条提交内容
git push #提交

其中默认git commit -s 使用的是gun nano的编辑器进行编辑，其中操作ctrl+o 保存   crtl+x退出。
可以修改使用vim进行提交信息的编译，使用 git config --global core.editor "vim"。
```

# 2.Repo usage


## 2.1 repo环境搭建



<font color="#dd0000" size="4">参考Gitee repo官方工具使用方法 ，直接参考该文档 [Repo 工具使用介绍](https://help.gitee.com/enterprise/code-manage/%E9%9B%86%E6%88%90%E4%B8%8E%E7%94%9F%E6%80%81/Repo%20%E5%B7%A5%E5%85%B7%E4%BD%BF%E7%94%A8%E4%BB%8B%E7%BB%8D)， 前题需要配置好python的环境， 安装好python3的环境，并配置好默认python使用的python3工具。</font><br />

或参考：获取 [tsinghua 源 repo](https://mirrors.tuna.tsinghua.edu.cn/help/git-repo/)

```bash

$ repo --version
<repo not installed>
repo launcher version 2.40
       (from /home/tanyi/bin/repo)
git 2.17.1
Python 3.8.0 (default, Dec  9 2021, 17:53:27)
[GCC 8.4.0]
OS Linux 4.10.0-28-generic (#32~16.04.2-Ubuntu SMP Thu Jul 20 10:19:48 UTC 2017)
CPU x86_64 (x86_64)
Bug reports: https://issues.gerritcodereview.com/issues/new?component=1370071

sudo apt install python3.8

sudo apt install libpython3.8-dev

将Python3.8设置为默认Python解释器
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 100

sudo update-alternatives --install /usr/bin/python  python /usr/bin/python2.7 200

sudo update-alternatives --config python

python --version
```

## 2.2 manifest仓库使用

### 2.2.1 创建仓库


```bash
git init --bare manifest.git

$ git init --bare manifest.git
Initialized empty Git repository in /mnt/share/gitee/repo_test/manifest.git/

repo_test$ tree  -L 2
.
└── manifest.git
    ├── branches
    ├── config
    ├── description
    ├── HEAD
    ├── hooks
    ├── info
    ├── objects
    └── refs
```


### 2.2.2 创建default.xml

manifest的仓库 https://gitee.com/tanchuanyi/manifest， 在仓库中提交default.xml 和 test.xml
简单使用如此章，具体的详细和更多用法，参考：[repo多仓库管理工具](https://gitee.com/tanchuanyi/documentation/blob/master/docPage/GiteeManageDoc/Repo_Detail_Usage.md)

* 使用默认的default.xml拉取代码

```bash
repo init -u git@gitee.com:tanchuanyi/manifest.git
```

```xml
<!--default.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote  name="gitee"
           fetch="git@gitee.com:tanchuanyi" 
           autodotgit="true" /> <!--fetch=".." 代表使用 repo init -u 指定的相对路径 也可用完整路径，example:https://gitee.com/MarineJ/manifest_example/blob/master/default.xml-->
  <default revision="master"
           remote="gitee" />
  <project path="Uconfig" name="Uconfig"  />
  <project path="documentation" name="documentation" />
</manifest>
```

* 使用指定test.xml拉取代码

```bash
repo init -u git@gitee.com:tanchuanyi/manifest.git -m test.xml
```

```xml
<!--test.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote  name="gitee"
           fetch="git@gitee.com:tanchuanyi" 
           autodotgit="true" />
  <default revision="master"
           remote="gitee" sync-j="2" />
  <project path="Uconfig" name="Uconfig" groups="gitee" revision="master" />
  <project path="documentation" name="documentation" groups="gitee" revision="master" />
</manifest>
```

###2.2.3 repo sync拉代码

```bash
$ repo sync -j12

$ tree -alL 2
.
├── documentation
│   ├── docPage
│   ├── .git
│   ├── gitee_user_guide.md
│   ├── .gitignore
│   ├── LICENSE
│   ├── README.en.md
│   └── README.md
├── .repo
│   ├── manifests
│   ├── manifests.git
│   ├── manifest.xml
│   ├── project.list
│   ├── project-objects
│   ├── projects
│   ├── repo
│   ├── .repo_config.json
│   └── .repo_fetchtimes.json
└── Uconfig
    ├── configs
    ├── .git
    ├── .gitee
    ├── Makefile
    ├── Makefile.common
    ├── README.md
    ├── tools
    └── Uconfig

```
