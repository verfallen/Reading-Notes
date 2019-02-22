# git

## 常用操作

### 初始化

```yaml
# 初始化：
git init
```

### 暂存

```yaml
# 暂存所有(包括为跟踪文件)
git add .

# 暂存所有（不包括未跟踪文件）
git add -u

# 暂存单个文件
git add README.md

# 暂存文件夹
git add src
```

### 提交

```yaml
# 提交并填写说明
git commit -m "description"

# 暂存并提交
git commit -am "commit"

# 重写上次提交：
git commit --amend

# 定位提交
git rev-parse <commit>

# 符号：
#  ~ ：用 ~ 符号数量的堆砌或者~数量的写法定位第几代父 commit
#  ^ : 用 ^ 数量的写法定位第几个父 commit ，用 ^^ 则表示第一个父 commit 的第一个父 commit
```

### 分支

```yaml
# 创建
git branch <branch>

# 查看本地分支
git branch

# 查看本地和远端分支
git branch -a

# 删除分支
git branch -d <branch>

# 强制删除分支
git branch -D | git barnch -df

# 合并
git merge <branch>
```

### 签出

```yaml
# 切换分支
git checkout branch

# 定位commit
git checkout commit

# 创建分支并切换
git checkout -b <branch>

# 撤销文件工作区改动
git checkout -- <file>
```

### 变基

```yaml
# 变基当前分支
git commit <branch>

# 使用变基代替合并：( dev 为当前分支，要合并到 master 分支)
git reabase master
git checkout master
git merge dev

# 裁剪 commit 变基,这里指的就是将 hotfix 到 hotfix分支与dev 分支的最近共同祖先 之间的 commit 裁剪下来复制到 master 分支上
git rebase --onto master dev hotfix

# 放弃变基
git rebase --abort

# 跳过某个冲突的提交
git rebase --skip

# 解决冲突后继续变基
git rebase --continue

# 一个  commit 变基
git cherry-pick <commit>

# 多个 commit 变基
git cherry-pick <c1> <c2>

# 多个连续 commit 变基(不包括c1)
git cherry-pick <c1>...<c3>

# 解决冲突后继续
git cherry-pick --continue

# 放弃 commit 变基
git cheery-pick --abort
```

### 重置

```yaml
# 重置暂存区和工作区
git reset --hard

# 重置暂存区
git reset --mixed | git reset

# 保留暂存区和工作区的改动
git reset --soft

# 文件从暂存撤回工作区
git reset -- <file>

# 文件若干 commit 撤回工作区
git reset <commit>
```

### 撤回

```yaml
#比较工作区和暂存区
git diff --staged

# 撤回提交
git revert commit

# 解决冲突后继续撤回
git revert --continue

# 放弃撤回
git revert --abort
```

### 储藏

```yaml
# 储藏当前工作区和暂存区
git stash

# 查看储藏列表
git stash list

# 恢复到工作区
git stash apply

# 原样恢复
git stash apply --index

# 恢复某一次储藏
git stash apply stash@{1}

# 清理
git stash drop

# 恢复并清理最近此次储藏
git stash pop
```

### 查看

```yaml
# 工作区与暂存diff，暂存区与当前版本diff 以及没有被git追踪的文件
git status

# 暂存区与当前版本diff
git diff --staged | git diff --cached

# 前两者之和
git status -v

# 工作区与暂存区diff
git diff

# git status  和  git diff 之和
git status -vv

# 查看某个git对象
git show <commit>

# 某个文件diff
git diff <file>

# 某个文件暂存区与当前commit 的差异
git diff --staged <file>

# 两个commit 之间差异
git diff <c1> <c2>

# 两个commit 间某个文件
git diff c1:a.md c2:a.md

# 提交历史
git log

# 提交历史显示具体信息
git log -p

# 提交历史显示文件修改统计信息
git log --stat

# 显示提交说明
git log --oneline

# 提交历史图表
git log --graph
```

### 定位 bug

1. 使用二分法定位到可能有 bug 的提交 git bisect <commit>
2. 没有 bug 运行 git bisect good 或 git bisect old
3. 有 bug 运行 git bisect bad 或 git bisect new

```yaml
# 查看某个文件每一行的修改信息
git blame <file>

# 查看文件某几行修改信息
git blame -L 1,5 a.md
git blame -L 1,+4 a.md

# 根据邮箱区分显示文件修改信息
git blame -e a.md
```

### 标签

```yaml
# 创建
git tag <name>

# 创建含附注标签
git tag -a <name>

# 给历史commit 创建标签
git tag -a v.10 36ff0f5

# 查看版本列表
git tag

# 查看标签详情
git show <tagname>

# 删除标签
git tag -d <tagname>

# 删除远端标签
git push origin -d v0.3

# 推送
git push origin <tag name>

# 一次性将标签推送到远端
git push origin --tags
```

### 远程

```yaml
# 查看远端仓库
git remote

# 查看远程仓库详细信息
git remote -v

# 查看某个远程仓库的信息
git remote show <remote-name>

# 拉取更新
git fetch

# 拉取并更新本地分支
git pull

# 推送
git push <remote-name> <branch-name>

# 添加新的远程仓库
git remote add horseshow <url>

# 创建分支并添加本地分支对远端分支的追踪
git checkout -b dev origin/dev 或者 git checkout --track orgin/dev

# 重命名
git remote rename <old-name> <new-name>

# 删除远端仓库
git remote rm horseshoe

# 给本地项目添加仓库地址
git remote add <url>
```
