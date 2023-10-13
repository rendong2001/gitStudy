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

----

```markdown
git pull 和 git fetch：

git pull 和 git fetch 这两个命令都可以用于下载远程仓库。可以认为git fetch是这两者中更加安全的那个，即便下载了远程的内容，但也不会更新你本地仓库的版本状态，以保证你本地当前代码完好无损。反观git pull命令则是一个更加激进的命令，它会下载当前正在工作的分支对应的远程内容，并且在下载成功之后马上执行一个git merge命令，为新下载下来的远程内容创建一次merge commit。此时如果你有正在进行中的工作还没准备好进行合并，这些行为可能会造成代码冲突，然后马上进入合并代码过程中解决冲突的流程
git fetch命令与git pull命令行为类似，但被认为是更安全、非破坏性的更新同步命令
```

```markdown
git merge 和 git rebase
git merge: 有两个分支，master 分支和 test 分支，test 分支是基于 master 分支在B处的提交节点创建的，在创建后 master 分支又经过迭代提交了两次，从C到D节点，test 分支也基于B往前继续更新了两次，到了F节点。两者从B开始就走向了分叉
git rebase: 当执行 rebase 操作时，git 会从两个分支的共同祖先开始提取待变基分支(master)上的修改，然后将待变基分支指向基分支(test)的最新提交，最后将刚才提取的修改应用到基分支的最新提交的后面。
```

![1697215696642](C:\Users\86182\AppData\Roaming\Typora\typora-user-images\1697215696642.png)

从图中可以看到，这里生成了一个新的提交G，是怎么生成的呢？ `merge` 命令 它会把两个分支的最新快照（`F、E` 和 `D、C`）以及二者最近的共同祖先（`B`）进行三方合并，合并的结果是生成一个新的快照`G`（并提交）

![1697215732814](C:\Users\86182\AppData\Roaming\Typora\typora-user-images\1697215732814.png)

在`master (待变基分支)`上执行`git rebase test(基分支)`时,git就会从两者的共同祖先B开始，提取 master 分支上的修改，也就是 C,D 两个 commit ,提取到之后 git 会先保存起来，然后将master 分支指向 test 分支最新提交的节点，也就是F节点，然后把提取到的 C,D 接到F后面，在这个过程当中，会删除原来的C,D commit 记录，生成新的C‘，D'，虽然C',D'和原来的C,Dcoommit的内容是一样的，但是 commit id 是不同的

rebase 操作如果用一句话进行解释就是改变基底。master 分支原来的基底是A，现在变成了以 test 分支最新的提交F做为新的基底了

## 恢复文件

未 add ：git checkout 文件名

已 add 未 commit ：git checkout commitId 文件名

已 commit ：git checkout commitId 文件名	git log 日志依然存在(不建议使用)

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

## 分支管理

查看所有分支：git branch -a	包括远程仓库的分支

向远程仓库推送：git push 远程仓库名称 分支名称	git push 远程仓库名称 --all  推送本地所有分支

删除远程仓库分支：git push 远程仓库名称 -d 远程分支名称

## 标签

查看标签：git tag	git tag --online

查看标签的详细信息：git show 标签名称

轻量化创建标签：git tag 标签名称

注释标签：git tag  标签名称 -a -m 备注信息

为指定提交打标签：git tag 标签名称 commitId

为指定提交打标签（另加注释信息）：git tag 标签名称 -a -m 备注信息 commitId		a：annotate（注释）

删除标签（本地）：git tag -d 标签名称

删除标签（远程）：git push 远程仓库名称 -d  标签名称

推送标签：git push 远程仓库名称 标签名称		在后边追加标签名称可推送多个标签

推送所有标签：git push 远程仓库名称 --tags

## 代码回滚

删除工作区改动代码，撤销 commit，撤销 add ：git reset --hard HEAD~n

工作区改动代码不变，撤销 commit，撤销 add：git reset --mixed HEAD~n

工作区改动代码不变，撤销 commit，不撤销 add：git reset --soft HEAD~n

**回退代码**

撤销 add 操作：git restore --staged 文件名

撤销工作区的修改内容：git restore 文件名

## 附：

### 创建 github 托管的web网站

通过 new organization 创建组织，要部署的仓库，其仓库名的格式为 "账户名称.github.io" 或 "组织名称.github.io"

### git 工作图

![img](https://cdn.nlark.com/yuque/0/2023/png/29743347/1688915507846-67474eeb-5b8c-4cf4-a1ff-76c2665b755f.png)