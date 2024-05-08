# 本地篇

## git branch

git branch 起头的命令意思是与分支相关的命令，**具体的效果由后面的短线参数确定**

- git branch -f b1 b2 ：表示把分支b1强制移动到b2位置
  - ## b1：**只能**使用分支名，不可以是 HEAD 和具体提交记录的哈希
  - b2：可以使用分支名、HEAD 和 具体提交记录的哈希

## git checkout

git checkout 特指移动 HEAD 指针

- git checkout b ：表示把 HEAD 指针指向 b 处
  - b：可以是分支名，HEAD 和具体提交记录的哈希。如果是 HEAD 不会有任何变化，如果是分支名会让 HEAD 成为粘附状态，如果是具体提交记录的哈希会让 HEAD 成为分离状态

## git commit

git commit 是对 HEAD 指针的操作。这个命令会产生一个新的提交记录并使其 parent 为 HEAD 当前指向的记录，然后移动 HEAD 到新记录上。【重要】如果 HEAD 为粘附状态，会把粘附的那个分支指针也移动到新记录上

## git reset

同样是针对 HEAD 指针的操作，但是不能在分离的 HEAD 里用 reset

## git revert

同样是针对 HEAD 指针的操作，而且可以在分离的 HEAD 里用 revert

# 远程篇

## git clone

在本地创建一个远程仓库的拷贝

## 远程分支

在 clone 一个远程库以后，产生的本地仓库在**每一个**分支都有一个对应的**远程分支**。由于远程分支的特性导致其拥有一些特殊属性。

远程分支反映了远程仓库(在你上次和它通信时)的**状态**。这会有助于你理解本地的工作与公共工作的差别 —— 这是你与别人分享工作成果前至关重要的一步.

远程分支有一个特别的属性，在你切换到远程分支时，自动进入分离 HEAD 状态。Git 这么做是出于不能直接在这些分支上进行操作的原因, 你必须在别的地方完成你的工作, （更新了远程分支之后）再用远程分享你的工作成果。

【重要】远程分支具有命名规范，必须是<remote name>/<branch name>的格式，但是'/'并非意味着目录分级，而只是一种字符串辅助记法而已，同样的还有 <remote name>/<organization name>/<project name>/<feature name> 等等，都只是字符串记法而已，**git 的分支之间没有层级关系**

- 使用 `git checkout origin/branchname' 切换到远程分支会怎么样？
  - 答：HEAD 位置变到 origin/branchname 上，但是并**不会**产生粘附，因此后续的 **commit** **不会**影响到 origin/branchname，它只有在**远程仓库中相应的分支更新**了以后才会更新

## git fetch

### git fetch 做了些什么

`git fetch` 完成了仅有的但是很重要的两步:

- 从远程仓库下载本地仓库中缺失的提交记录
- 更新远程分支指针(如 `o/main`)

`git fetch` 实际上将本地仓库中的远程分支更新成了远程仓库相应分支最新的状态。

如果你还记得上一节课程中我们说过的，远程分支反映了远程仓库在你**最后一次与它通信时**的状态，`git fetch` 就是你与远程仓库通信的方式了！希望我说的够明白了，你已经了解 `git fetch` 与远程分支之间的关系了吧。

`git fetch` 通常通过互联网（使用 `http://` 或 `git://` 协议) 与远程仓库通信。

### git fetch 不会做的事

`git fetch` 并不会改变你本地仓库的状态。它不会更新你的 `main` 分支，也不会修改你磁盘（即工作区）上的文件。

理解这一点很重要，因为许多开发人员误以为执行了 `git fetch` 以后，他们本地仓库就与远程仓库同步了。它可能已经将进行这一操作所需的所有数据都下载了下来，但是**并没有**修改你本地的文件。我们在后面的课程中将会讲解能完成该操作的命令 :D

所以, 你可以将 `git fetch` 的理解为单纯的下载操作。

【补充】远程仓库中没有 HEAD 指针

---

git fetch 命令：

```ruby
$ git fetch <远程仓库名> //这个命令将某个远程主机的更新全部取回本地
```

如果只想取回特定分支的更新，可以指定分支名：

```ruby
$ git fetch <远程仓库名> <分支名> //注意之间有空格
```

最常见的命令如取回`origin` 主机的`master` 分支：

```ruby
$ git fetch origin master
```

取回更新后，会返回一个`FETCH_HEAD` ，指的是某个branch在服务器上的最新状态，我们可以在本地通过它查看刚取回的更新信息：

```shell
$ git log -p FETCH_HEAD
```

【重要】

```$ git fetch <远端仓库名> <远端分支名> ``` 不需要指定要拉入的本地分支名，因为本地分支是并且一定是并且只能是远端分支名对应的本地远程分支名（即固定的下游分支，如远程的 master 之于本地的 origin/master分支）

【重要】

`git fetch` = `get fetch origin` = 将本地的远程分支与真正的对应的远端分支同步，并下载需要的记录结点

## git pull

git pull origin b1:b2 = git fetch origin b1 + git switch b2 + get merge origin/b1

## git push

【重要】*注意 —— `git push` 不带任何参数时的行为与 Git 的一个名为 `push.default` 的配置有关。它的默认值取决于你正使用的 Git 的版本，但是在教程中我们使用的是 `upstream`。 这没什么太大的影响，但是在你的项目中进行推送之前，最好检查一下这个配置。*

git push 修改远端分支，也同步修改本地的远程分支，因为这也算一次“通信”