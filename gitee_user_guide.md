# Gitee usage

## 1 Gitee Key manage

### 1.1.单key管理 
如果需要在新的服务器上下载新的仓库内容或提交代码及内容，需要在自己的服务器上生成ssh公钥，并添加到自己的账户当中。具体可以参考: https://help.gitee.com/enterprise/code-manage/%E6%9D%83%E9%99%90%E4%B8%8E%E8%AE%BE%E7%BD%AE/%E9%83%A8%E7%BD%B2%E5%85%AC%E9%92%A5%E7%AE%A1%E7%90%86/%E7%94%9F%E6%88%90%E6%88%96%E6%B7%BB%E5%8A%A0SSH%E5%85%AC%E9%92%A5

```bash
1. sshkey生成密匙, 这里的 xxxxx@xxxxx.com 只是生成的 sshkey 的名称，并不约束或要求具体命名为某个邮箱
ssh-keygen -t ed25519 -C "xxxxx@xxxxx.com"  
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

###1.2 多key管理
需要有多个账号，或者与平时其他的账号隔离开可以参考: https://help.gitee.com/enterprise/code-manage/%E6%9D%83%E9%99%90%E4%B8%8E%E8%AE%BE%E7%BD%AE/%E9%83%A8%E7%BD%B2%E5%85%AC%E9%92%A5%E7%AE%A1%E7%90%86/Git%E9%85%8D%E7%BD%AE%E5%A4%9A%E4%B8%AASSH-Key

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
