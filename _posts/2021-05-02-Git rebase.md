#### https://git-scm.com/book/zh/v2/Git-分支-变基
```bash

# rebase操作能把本地未push的分叉提交历史整理成直线
# merge 操作合并分支会让两个分支的每次提交都按提交时间（不是push时间）排序
# 并且会将两个分支最新的commit点合并成新的commit，最终的分支树呈现非整条线性直线的形式
# rebase实际上是将当前执行rebase分支的所有基于原分支提交点之后的commit打散成一个个的patch
# 并重新生成新的commit hash值，然后再基于原分支目前最新的commit点进行提交
# 即：将特定分支最开始的提交部分移动到某分支的特定部分 ...

# ------------------------------------------------ Example

# 当前所在HEAD为master分支的"e0ea545"提交位置 ...
# 下方最新的提交"e0ea545"是本地创建内容并提交后再执行pull操作获取远端更新后得到的merge图形
$ git log --graph --pretty=oneline --abbrev-commit
# *   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:Exaple/test  # 当前位置
# |\  
# | * f005ed4 (origin/master) set exit=1    # 第2次Pull之后同步进来的远端操作 (此时造成了分叉)
# * | 582d922 add author                    # 第1次pull之后在本地执行的commit信息 ...
# * | 8875536 add comment                   # 第1次pull之后在本地执行的commit信息 ...
# |/                                        # 第1次pull操作 ...
# * d1be385 init hello
# ...

# 此时执行如下命令将输出很多操作
# 注：若有冲突就解决冲突，解决后直接 git add . 再 git rebase --continue 即可
$ git rebase

# 查看效果 (原本分叉的提交现在变成直线了)
$ git log --graph --pretty=oneline --abbrev-commit
# * 7e61ed4 (HEAD -> master) add author
# * 3611cfe add comment
# * f005ed4 (origin/master) set exit=1
# * d1be385 init hello
# ...

# Git把本地的提交挪动位置放到了f005ed4 (origin/master) set exit=1之后，这样提交历史就成了直线
# rebase操作前后的最终提交内容是一致的，但本地的commit内容已经变化了，它的修改不再基于 d1be385
# 而是基于 f005ed4 (origin/master) set exit=1 (把分叉的提交历史整理成直线，因此看上去更直观)
# 最后通过push操作把本地分支推送到远程，此时远程分支的提交历史也将是一条直线 ...

```
