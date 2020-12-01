# git 复习笔记

> 根据技术学院bootcamp上git快速入门课程进行学习：https://tech.bytedance.net/videos/6844026183705690126
>
> 笔记仅是个人学习记录笔记，用于个人查询以及复习，并不全面。



## git基础

### 基本操作

1. `git add ` , `git commit -m "description"` , `git status` 

2. 可以用`git log` 和`git show commitid` 查看具体信息

3. `git diff --cached` 查看暂存区文件变动

   `git diff` 查看工作区文件变动

### 分支

- `git checkout -b develop` :创建并切换

- `git branch develop`:只创建不切换

- `git checkout feat01`:切换分支

- 合并分支方法1：

  - 先切换到主分支,`git checkout feat01`

  - `git merge feat02`

    <img src="img/截屏2020-11-25 下午8.41.34.png" alt="截屏2020-11-25 下午8.41.34" style="zoom:50%;" align="left"/>

- 合并分支方法2：(比较建议)

  - 先切换到副分支

  - `git rebase feat01`：其实是将feat02的改动提交合到feat01后面。

  - 然后再切换到`feat01`: `git checkout feat01`

  - `git merge feat02`: 最终合完

    <img src="img/截屏2020-11-25 下午8.42.23.png" alt="截屏2020-11-25 下午8.42.23" style="zoom:50%;" align="left"/>

- 解决冲突：利用文件中的提示，直接vim编辑改动，把`>>HEAD`这类删除解决，再次提交

- 利用提交的哈希值回到某次提交，这时是游离的，没有分支，可能在此提交可能会丢失。

  如何解决：基于当前节点再创建一个分支：`git checkout -b new-head`
  
- `git branch -a`:查看所有分支

- `git branch -r` :查看远程分支



### Git Workflow

1. `git pull` :将同一分支的最新提交拉下来，否则无法`git push` 上去。

2. `git fetch` : 是`git pull` 命令的一个拆解。然后需要再`git merge o/master` 将远程的master更新的内容合到本地的master上。这样再`git push` 就成功了

   <img src="img/截屏2020-11-26 上午10.41.22.png" alt="截屏2020-11-26 上午10.41.22" style="zoom: 33%;" align="left" />

3. `git pull --rebase` :相当于在`git pull` 的时候，不再出现自动生成的merge commit.

4. 常见的回滚：

   - `git reset commithash` : 回到某一次提交，并丢弃之后的提交
   - `git revert` : 将某一次提交的修改恢复回去
   - `git reflog` : 只要commit过的修改，都可以恢复。用此命令可以查看所有git相关操作的记录，并有对应的hash值，搭配reset到哪里都可以

5. 常见的重写历史：

   - `git commit --amend`: 修改最后一次提交
   - `git rebase -i` : 修改多个提交信息、重新排序提交、压缩提交、拆分提交。

