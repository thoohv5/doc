# Git

# 介绍

依赖CI服务器
分布式版本控制系统

## Git vs SVN

1. 独立性 脱机环境历史版本管理
2. 安全性 中央仓库(开发者仓库同步)
3. 多支性 强大的分支管理

## 工作区和暂存区

![https://i.loli.net/2021/03/16/FGik5eN9MgDlW7O.png](https://i.loli.net/2021/03/16/FGik5eN9MgDlW7O.png)

把文件往Git版本库里添加，分两步执行：
1. 用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
2. 用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

## https和git

Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快
使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令

### 免密登录

```go
git config --global user.name thooh
git config --global user.email rol@thooh.com

cd ~/.ssh
ssh-keygen -t rsa -C "rol@thooh.com"
cat ~/.ssh/id_rsa.pub
ssh-add id_rsa
ssh -T git@gitee.com
```

# 命令

## 常用命令

```go
# 代码rebase
git reset [<mode>] [<commit>]
<mode>
--mixed 版本库=>暂存区=>工作区
--soft  版本库=>暂存区
--hard  x版本库，x暂存区，工作区
--keep  x版本库，暂存区=>工作区

# 设置远程分支
git push --set-upstream origin <branch>

# 代码rebase
git rebase -i --root

		pick：保留该commit（缩写:p）
		reword：保留该commit，但我需要修改该commit的注释（缩写:r）
		edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
		squash：将该commit和前一个commit合并（缩写:s）
		fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
		exec：执行shell命令（缩写:x）
		drop：我要丢弃该commit（缩写:d）

# 提交历史记录/分支合并图
git log [--pretty=oneline]/git log --graph
```

### git merge 和 git rebase

```go
# 合并指定分支到当前分支[Fast forward]
git merge [--no-ff] <name> -m mark

# 合并变基
git rebase <name>

```

### git rm

```go
git mv README.md README 
-----------------------
mv README.md README 
git rm README.md
git add README
```

## 忽略已提交的文件

只针对已经commit过且有改动的文件 (因为rm的是cached列表中的文件, cached列表即修改列表)

```go
git rm -r --cached <filename>
vim .gitignore
git add .
git commit -m "mark"
git push
```

# git flow

![https://i.loli.net/2021/03/16/TEHRe4gfj3c9Zmw.png](https://i.loli.net/2021/03/16/TEHRe4gfj3c9Zmw.png)

# git commit format

```go
<header>(<type>[<scope>]: <subject>)
// 空一行
<body>
// 空一行
<footer>

explain:
<header>

<type>
feat 新功能（feature）
fix 修补bug
docs 文档（documentation）
style 格式（不影响代码运行的变动）
refactor 重构（即不是新增功能，也不是修改bug的代码变动）
test 增加测试
chore 构建过程、辅助工具的变动
perf 提高性能

<scope>
影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

<subject>
目的的简短描述，不超过50个字符。
以动词开头，使用第一人称现在时，比如 change，而不是 changed 或 changes
第一个字母大写
结尾不加句号

<body>
解决问题的方案
解决问题的影响点

<footer>
不兼容变动
关闭Issue
```

[写出好的 commit message](https://ruby-china.org/topics/15737)