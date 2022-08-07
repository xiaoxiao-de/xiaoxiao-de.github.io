---
title: git命令
date: 2022/05/15 20:46:25
tags: [GIT]
categories: 技术
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
##### 安装完git之后第一件是就是设置自己的用户名和邮箱地址

```
git config --global user.name “用户名"
git config --global user.email "邮箱"
```

- 如果是使用了--global选项,那么命令只需要运行一次,即可用户生效
- 配置完用户名和邮箱后会被写入到c盘的用户文件夹的.gitcongih文件中可以自行查看

##### git快速查看全局配置信息

```
查看所有配置 git config --list --global
指定查看配置项 git config user.name 或 user.email
```

##### 获取帮助信息

```
git help config 或 git config -h
```

##### 初始化git仓库 

```
git init
```

- 输入命令后会创建一个名.git的隐藏目录,
- 这个.git目录就是当前项目的git仓库
- 里面包含了初始化必要文件

##### 查看文件状态的命令

```
git status 或 加s表示精简
```

##### 一次性将文件加入暂存区的命令

```
 git add . 或 加入暂存区 git add '文件名'
```

##### 将暂存区的文件提交到git厂库命令

```
git commit  -m “提交消息”
```

##### 克隆远程仓库

```
git clone 远程仓库地址
```

##### 如果需要从暂存去中移除对应大的文件使用 

```
git reset HEAD "移除文件”
```

##### 跳过使用暂存区域 直接跳过暂存区,将工作区中的修改直接提交到git仓库

```
git commit -a -m “描述消息"
```

##### 从工作区和git仓库移除文件

```
git rm -f "文件名"   // 只删除git仓库  git rm --cached "文件名"
```

##### 查看提交历史

```
git log
```

##### 回退到指定版本在一行上展示所有的提交历史

```
git log --pretty=online
```

##### 根据指定的提交ID回退指定版本

```
git reset --hard <CommitID>
```

##### 再次根据最新的提交ID,跳转到最新的版本

```
git reset --hard <CommitID>
```

##### 快速生成SSHkey----远程仓库配置使用

```
ssh-keygen -t rsa -b 4096 -C "邮箱"
```

- 会在c盘用户文件夹.ssh目录生成id_rsa和id_rsa.pub
- 使用记事本打开id_rsa.pub文件,复制里面的文本内容
- 在浏览器中登入github 点击头像-> settings(设置)->进行秘钥配置
- 将id_rsa.pub 文件中的内容 粘贴到key对应的文本框中
- 填写一个名称用来标识key 

##### 检测sshkey是否配置成功

```
ssh -T git@github.com
```

上述命令执行成功后,可能会看到提示消息 输入yes即可

##### 查看分支

```
git branch *带表当前所在分支
```

##### 创建分支

```
git branch 分支名称 创建完分支后当前还是在主分支
```

##### 切换分支 

```
git  checkout 分支名称
```

##### 快速创建一个分支 并立即切换到新分支 

```
git checkout -b 分支名称
```

##### 合并分支 首先切换到主分支 在主分支上使用 

```
git merge 分支名称
```

##### 删除分支 

```
git branch -d 分支名称
```

遇到冲突时的分支合并如果在两个不同的分支中,对同一个文件进行了不同的修改,git就没法干净的合并它们此时需要手动解决冲突,打开包含冲突的文件

```
git add .
git commit -m ""
```

##### 将本地分支推送到远程仓库,第一次将本地分支推送到远程仓库需要使用

```
git push -u 远程仓库名字 本地分支名称：远程分支名称
实际案例:
git push -u origin payment:pay
```

第一次推送分支需要带-u参数,此后可以直接使用git push推送代码到分支
此后可以直接使用git push 推送代码到远程分支

##### 查看远程仓库分支列表

```
git remote show '远程仓库名称'
```

##### 跟踪分支 从远程仓库中 把远程分支下载到本地仓库

```
git checkout -b 本地分支名称 远程仓库名称/远程分支名称
示例: git checkout -b payment origin/pay
```

##### 查看远程分支最新代码下载到本地对应文件中

```
可以使用git pull
```

##### 删除远程分支

```
git push '远程仓库名称(origin)' --delete '远程分支名称'
```

##### 查看本地密钥

```
cat ~/.ssh/id_rsa.pub
```

##### 已有账号创建私钥

```
ssh-keygen -t rsa -C"邮箱"
```

##### git上拉取指定分支

```
git clone -b 分支名 '克隆地址'
```

