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
  
## stash

当我们在自己分支上工作的时，如果有临时bug类的急性修复事件，我们需要切到指定分支上修复。当我们co到目标分支时，有两个因素要注意：

1. 本地有修改
2. 本地有提交

实践发现，当两者皆具备时，git会尝试checkout，当本地的变化会因为co而被修改，则co失败，提醒用户做保存。而当只占据一个因素的时候，是可以co成功，此时，修改会保留在working zone里，提交停留在原地，不会变化。

因此我们需要对修改做保存，待bug修复完后，恢复。

这时我们通常可以：

* clone一份新的代码下来
* git stash当前分支的本地修改，然后git checkout

stash过程相当于为本地的修改，做一次提交（多次stash的话，则相当于线性多次提交），当pop时，则将对应的提交点，还原到本地修改空间。

## cherry-pick

当提交错了分支，我们可以记录一下已经提的commit ref，然后切到想要提交的分支下，使用`git cherry-pick [commit ref]`。它会将制定的commit复制一份，作为当前分支的提交，制定的commit不被变化。


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

8.如果历史提交出现`Merge branch 'master' of https://github.com/rogerAce/MasterGit`，说明有人在本地对两个分流，但都属于master的分支，进行了合并。这时候有可能会出现两个一模一样的奇怪的提交（如0724cc2d0f6c175758e36245889a6d2e71c036a1...020dd1f57102220c0a0bb1669e6e957eb93115ce），但是git是认可的。这有可能是因为本地执行了git rebase -i修改了本地提交过的分支，然后分流，又合并，导致的，应该尽量避免。

### [动画做题][1]积累
* git rebase -i HEAD~4 --aboveAll 可以取过去的4次提交（也可以不是当前分支上）的中的任意某几次，重新建立一串commit。可以修改本地的，没有提交到远端的commit内容
* git commit --amend 可以修改commit注释



[1]:http://pcottle.github.io/learnGitBranching/ "null"
[撤销、回滚提交]:http://gitbook.liuhui998.com/4_9.html



