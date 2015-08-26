MasterGit关键命令
============

## 优秀链接
* [图解git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html)
* [动画做题][1]

##基本的命令##
* git branch
* git merge
* git rebase
* git checkout: to move the ***asterisk*** to any branch
* git cherry-pick: 复制被捡的提交到到本地，作为一个提交
* git reset
* git add
  - -p提交部分

##一些标识##
* HEAD: 当前工作区
* HEAD^: ***^*** means the first parent of the HEAD.
* branch~2: **~2** means the the parent of parent of the branch.

##到处移动##
1. use `git checkout C1` to detach HEAD. use `git checkout branch` to attach back. 只是移动HEAD。
> Detaching HEAD just means attaching it to a commit instead of a branch. This is what it looks like beforehand:**HEAD -> master -> C1**

2. git branch -f master HEAD~3 : 把master标识移到了父三代上
3. git reset --hard commitref : 把master标识移到了指定commitref，同时移动HEAD。
What's the differents bewteen `git branch -f` and ` git reset --hard`?

### git reset细节

git reset --hard <commit_id>

```
--mixed 回退commit，也回退index files信息
--soft 回退commit，但不会退index files信息
--hard 完全回到指定commit_id版本，本地修改被撤销
```

* 只是用来查看过去一个历史状态的代码，或者处理本地错误提交的代码。不是用来做回滚用。
* 如果要回滚以往一次提交，使用git revert commitref
* 如果要回滚以往一次提交的某个文件，使用git co commitref a.file

##配置##
在~/.gitconfig中可以添加如下配置：

* 需要配置的东东
  - alias.lola = log --graph --decorate --pretty=oneline --abbrev-commit --all
  - alias.lola = log --graph --decorate --pretty=format:"%h - %an, %ar : %s" --abbrev-commit --all
  - alias.st=status
  - alias.ci=commit
  - alias.co=checkout
  - alias.br=branch
  - alias.dc=dcommit
  - alias.rb=rebase
  

通过命令的方式也可：

* git config --global user.name="roger"
* git config --global user.email="rogerace@github.com"
* git config --global alias.st status

##追溯记录##

* git fsck --unreachable
* git reflog | less
* more .git/logs/HEAD 
* git log -p --stat 列出每次提交的代码变更
* [使用图形化工具查阅提交历史](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)
  - gitk命令：它是基于/usr/bin/wish的
  - sourceTree

## 其他
### 平时积累

1.git push origin HEAD --force

2.删除本地分支：`git branch -d <branchName>`

3.删除远程分支：`git push origin --delete <branchName>` 或 `git push origin :<branchName>`

4.查看本地branch信息：git branch -v

5.git新建与合并分支：参考[链接](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)git co -b issue315相当于：

* git branch issue315
* git co issue315

6.[git fetch & pull](http://blog.csdn.net/liangxiaozhang/article/details/8281047)

7.整理分支

* git remote show origin 显示远程分支信息 还可以查看哪些本地分支是无效的，是没有更新到最新的
* git branch -r 显示上次fetch后，目前本地保存的远程分支信息
* git remote prune origin / git fetch -p 可以清楚无效分支（无效分支是指fetch到本地，但是被其他管理者删除的远程分支）
* git branch -m devlop develop 修改名字

8.test

### [动画做题][1]积累
* git rebase -i HEAD~4 --aboveAll 可以取过去的4次提交（也可以不是当前分支上）的中的任意某几次，重新建立一串commit。可以修改本地的，没有提交到远端的commit内容
* git commit --amend 可以修改commit注释



[1]:http://pcottle.github.io/learnGitBranching/ "null"
[撤销、回滚提交]:http://gitbook.liuhui998.com/4_9.html



