# git学习

## 命令

查看 git 配置：git config --list

查看版本库的状态：git status

查看分支：git branch

切换分支：git checkout ~

创建并切换到新分支：git checkout -b ~

仅创建分支：git branch

删除分支：git branch -d/D ~		-d删除已经合并过的分支、-D强制删除(不管是否合并过)

重命名分支：git branch -m 原分支 新分支

合并分支：git merge

查看日志：git log    按Q退出

查看更精简的日志：git log --oneline

显示关联远程库的名称：git remote show

远程仓库名称重命名：git remote rename 旧名称 新名称

移除已关联的远程仓库：git remote remove 远程库名称

查看文件修改了哪些内容：git diff 文件名.后缀

提交修正：git commit --amend	对前一次的提交信息做修改，并且不会增加新的提交记录

## 关联远程

![img](https://cdn.nlark.com/yuque/0/2023/png/29743347/1688795285889-a2f2d835-a75f-423d-8785-3058d7d15a30.png)

git remote add origin https://github.com/rendong2001/gitStudy.git

拆解：使用 git remote add 命令，将远程库和本地关联到一起，origin(远程库名称)，https://github.com/rendong2001/gitStudy.git(远程库地址)

git push -u origin main

拆解：向远程推送，-u将本地分支上传并合并到远程分支，origin(远程库名称) main(要推送的分支)，之后提交命令可以简化为 git push

**可以关联多个远程**

![img](https://cdn.nlark.com/yuque/0/2023/png/29743347/1688797231920-c3b163d5-17c6-4f7d-a1a0-6c9493b12044.png)

git remote add origin https://gitee.com/rendongc/git-study.git

解析：执行这个命令管理 gitee 的远程库，但是库名不能再用 origin 了，因为 github 的远程库名已经是这个

git push -u origin "master"

解析：那么再次向 gitee 远程推送的时候，就能使用这个命令。而是 git push 远程仓库名 要推送的分支

## 日志

**显示**

条数显示：git log -n

单行显示：git log --online

图表显示：git log --graph

显示更改摘要：git log --stat

显示更改位置：git log --patch 或者 git log -p

**筛选**

按日期：git log --after='2023-07-09'	git log --before='2023-08-20'	git log --before='30 day ago'

git log --before='4 week ago'		git log --before='1 month ago'

按作者：git log --author='mingmengshaung'	可以是用户名或者邮箱，这个是模糊匹配

按提交信息：git log --grep='xxx'	同上模糊匹配

使用 git log 时，选项可以自由组合，例如：git log --author='mingmengshaung'  --online

**引用日志**

查看引用日志：git reflog

普通日志是版本库的一部分，可以随着版本库被 push

引用日志只保存在本地