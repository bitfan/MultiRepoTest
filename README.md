# 同一个git客户端下使用多个账号
目前想把GitHub上的项目同时在gitee上保留一份拷贝，就存在同一个项目，推送到两个仓库的问题。
由于之前用GitHub，已经配置过git客户端。具体如何配置git客户端，可以参考：[初次运行 Git 前的配置](https://gitee.com/help/articles/4107)
由于是两个不同仓库，可以将提交邮箱设置为同一个邮箱。

## 生成和使用SSH Keys
使用如下命令生成ssh key：
```bash
ssh-keygen -t rsa -C your@email.com
```
由于两个仓库使用同一个email，所以可以用同一个ssh key。这个命令会在`~/.ssh`文件夹下生成两个文件：`id_rsa`和`id_rsa.pub`。
把`id_rsa.pub`的内容贴到两个仓库的SSH key设置里。（GitHub网站在个人的Settings→SSH and GPG Keys）。

## 测试push到两个仓库
首先，在两个仓库网站分别创建项目，本测试项目叫MultiRepoTest。
然后在合适的文件夹运行如下命令：
```bash
mkdir MultiRepoTest
cd MultiRepoTest
git init
touch README.md
git add README.md
git commit -m "first commit"
```
现在，设置两个remote地址（在创建项目时，可以获得HTTPS或SSH地址，可以分别拷贝两个SSH地址）：
```bash
git remote add origin git@github.com:xxxx/MultiRepoTest.git
git remote set-url --add origin git@gitee.com:xxxx/MultiRepoTest.git
git remote -v
```
可以看到：
```
origin  git@github.com:xxxx/MultiRepoTest.git (fetch)
origin  git@github.com:xxxx/MultiRepoTest.git (push)
origin  git@gitee.com:xxxx/MultiRepoTest.git (push)
```
此时，可以发布到两个仓库：
```bash
git push -u origin master
```
此时，可以在两个仓库中都可以看到提交的文件。
