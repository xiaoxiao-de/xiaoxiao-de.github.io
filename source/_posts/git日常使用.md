### git日常使用
- git是一个开源的分布式版本控制系统
- git有3个工作区 workspace工作区 staging area 暂存区
- local repository 本地仓库 remote repository 远程仓库
##### git基本操作
```
git clone <仓库地址>
拷贝一个git仓库到本地

git branch
用于查看本地仓库分支和远程仓库分支

git remote add [shortname] [url]
添加远程版本库

git fetch
用于从远程获取代码库

git checkout <本地分支名>
切换本地分支

git pull <本地分支名>
用于从远程获取代码并合并本地的版本

git add .
添加当前目录下的所有文件到暂存区

git commit -m [message]
将暂存区内容添加到本地仓库

git push <本地分支名>丨<远程分支名>
用于从将本地的分支版本上传到远程并合并
```
1. 需求开发前的分支拉取流程
2. 需求开发后的分支合并流程
3. 分支合并出现的冲突如何解决
4. 出现线上问题时hotfix分支的操作流程