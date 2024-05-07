# git branch

git branch 起头的命令意思是与分支相关的命令，**具体的效果由后面的短线参数确定**

- git branch -f b1 b2 ：表示把分支b1强制移动到b2位置
  - b1：**只能**使用分支名，不可以是 HEAD 和具体提交记录的哈希
  - b2：可以使用分支名、HEAD 和 具体提交记录的哈希

# git checkout

git checkout 特指移动 HEAD 指针

- git checkout b ：表示把 HEAD 指针指向 b 处
  - b：可以是分支名，HEAD 和具体提交记录的哈希。如果是 HEAD 不会有任何变化，如果是分支名会让 HEAD 成为粘附状态，如果是具体提交记录的哈希会让 HEAD 成为分离状态

# git commit

git commit 是对 HEAD 指针的操作。这个命令会产生一个新的提交记录并使其 parent 为 HEAD 当前指向的记录，然后移动 HEAD 到新记录上。【重要】如果 HEAD 为粘附状态，会把粘附的那个分支指针也移动到新记录上

# git reset

同样是针对 HEAD 指针的操作，但是不能在分离的 HEAD 里用 reset

# git revert

同样是针对 HEAD 指针的操作，而且可以在分离的 HEAD 里用 revert