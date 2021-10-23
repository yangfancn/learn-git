# Git 分布式版本控制管理工具

## 配置
全局邮箱用户名配置
```linux
git config --global user.email "user@email.com"
git config --global user.name "username"
```

## 常用操作

### 初始化
```linux
git init
```
> 必须进入要初始化的目录再执行，执行完毕会生成  `.git` 目录

### 提交
```linux
# 添加单个文件或多个文件
git add file1
git add file2 file2

# 添加所有更改的文件
git add -A

#提交
git commit -m "本次提交说明"
```

### 查看状态
`git status`
> `on branch` 告知当前处于那个分支
> 
> `Changes not staged for commit:` 改版并未提交的文件
> 
> `Untracked files:` 新增并未提交文件

`git diff filename`

查看文件的具体改动

`git log`
查看更改日志 按提交时间从近到远排序
加 `pretty=oneline` 显示简略信息
`01e2d70672dfd380fcec94290ded6d5a1e4cb266` 这种字符串是唯一的 `commit id` 版本号

### 版本回退
`git reset` 

`HEAD` 表示当前版本
`HEAD^` 表示上个版本
`HEAD^^` 表示上上个版本
`HEAD~100` 表示上一百个版本
所以回退到上个版本的命令是
`git rese --hard HEAD^`

如果想要回到新版本

在知道 `commit id` 的情况下

`git reset --hard 1094a` 版本id只需输入前几位即可

不知道 `commit id` 的情况下

使用 `git reflog` 查看每一次的操作日志 ，获取 `commit id`

### 撤销修改
当修改文件后想要撤回修改

`git checkout -- <file>`

两种情况

1. 文件修改后还没有放到暂存区，即还没有运行 `git add`，此时撤销修改就会回到和版本库一模一样的版本
2. 已添加到暂存区，并且再次更改，此时撤销就会回到暂存区中的状态

> 命令中的 `--` 很重要，没有这个就是切换分支的命令

已经 add 但是未 commit, 并且想将文件回到上个版本的状态
`git reset HEAD <file>` 将暂存区的修改撤销，重新放回工作区
> `git reset` 既可以回退版本，亦可以将暂存区的修改回退到工作区

再运行 `git checkout -- <file>` 丢弃工作区中的修改

* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。

### 删除文件
已经 `add` 过的文件在被删除后
1. 确实需要重版本库中删除文件 `git rm <file>`, `git commit -m ` 提交更改到版本库
2. 误删除 使用 `git checkout -- <file>` 撤销删除

> 必须是已经 add 到暂存区获取已经提交到版本库的文件才可以撤销删除


## 远程仓库
在 github 创建一个远程仓库

同步本地代码到远程仓库

`git remote add origin git@github.com:yangfancn/gitPractise.git`

`origin` 是远程仓库的名字，可以自定义， `yangfancn` 是你的github账户, `gitPractise.git` 是仓库地址

### 推送内容到远程仓库
`git push -u origin master`

第一次推送内容时加了 `-u` 参数，git 会把本地 master 分支内容推送到远程仓库上的新 master 分支，并且还会把两个分支关联起来，以后的推送命令就可以简化为 `git push`

### 解除与远程库的关联
添加库时写错地址或者想删除远程库

`git remote rm <name>` ,删除之前可以使用 `git remote -v` 查看远程库信息
 
 然后根据远程库信息删除 `git remote rm origin`

 > 此操作仅仅是接触本地与远程库的关联，并不是删除，真正的删除需要在 github 上操作

 ### 从远程库上克隆
 `git clone git@github.com:yangfancn/gitPractise.git`


 ## 分支管理
 
 ``` shell
# 创建分支 dev
git branch dev
# 切换分支
git checkout dev
# 创建并切换
git checkout -b dev
# 查看当前分支
git branch
# 合并分支到主分支
git checkout master
git merge dev
# 删除分支
git branch -d dev
# 由于 checkout 切换分支 和撤销文件修改 checkout -- <file> 容易混淆，新版本的给git 提供了 swith 命令来切换分支
# 切换到新分支
git switch -c dev
# 切换到已有分支
git switch master
```

### 解决冲突
当在两个分支中修改了统一文件同一位置，在提交两个分支之后执行合并分支，会报错并提示冲突，此时需要手动合并分支之后在提交，使用  `git status` 可以查看冲突文件，文件中冲突位置会用 `<<<<<<<`, `=======`, `>>>>>>>` 标记


默认合并分支的情况下会尽可能使用 `fast forward` 模式，在这种模式下，删除分支，会丢掉分支信息。如果强制禁用改模式 `--no-ff`， git 在 merger 时生成一个新的commit，这样就可以在分支历史上看出分支信息
> 此模式会产生新的 commit， 所以需要加 `-m "<description>"`

### 分支策略
`master` 应该作为稳定版本，在开发不要直接在master上工作，都应该在 `dev` 分支上干活,然后团队成员都在 `dev` 分支上干活，每个人都有自己的分支，在提交到 `dev` 分支上