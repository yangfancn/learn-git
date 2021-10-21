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

> `Changes not staged for commit:` 改版并未提交的文件

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
