+++
title = 'Git'
date = '2024-06-10'
author = 'aiclr'
categories = ['tools']
tags = ['git','vcs']
Summary='Git is the version control system (VCS) designed and developed by Linus Torvalds, the creator of the Linux kernel. '
+++

| 常用 ||
|:---|:---|
| 提交记录 | `git log --pretty=oneline` |
| 仓库状态 | `git status` |
| 追加修改到仓库 | `git add .` |
| 提交commit | `git commit -m "feat: init"` |
| fetch 更新 origin 仓库 的指定分支 | `git fetch origin remote:local` |
| push 提交 本地 develop 分支 到 origin 仓库 develop2 分支 | `git push origin develop:develop2` |
| 查看其他分支文件 | `git show develop:README.md` |
| 刷新以应用新的配置文件 | `git rm --cached -r` & `git reset --hard` |
| 远程分支删除后，本地同步清理 | `git remote prune origin` |

## config

说明：
- 全局配置目录 windows10 `C:\Users\xxx\.gitconfig`
- 当前工作目录文件 `workdir/.git/config`

```sh
# example
git config -h
# 查看 `global` 全部 配置
git config -l
git config -l --global
# 查看 `local` 全部 配置 
git config -l --local
```

user & email 配置文件
```properties
# vim .git/config
[user]
	name = xxx
	email = xxx@xx.com
```

```sh
# 全局 git user
git config --global user.name xxx
git config --global user.email xxx@xx.com
# 单库 git user
git config user.name xxx
git config --local user.name xxx
git config user.email xxx@xx.com
git config --local user.email xxx@xx.com
```

crlf & lf ***跨平台脚本之灾*** \
[GitHub reference](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings) \
[git reference](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
- `core.autocrlf false` If you’re a Windows programmer doing a Windows-only project, then you can turn off this functionality, recording the carriage returns in the repository by setting the config value to false
- `core.autocrlf true` If you’re on a Windows machine, set it to true this converts **LF** endings into **CRLF** when you check out code
- `core.autocrlf input` If you’re on a Linux or macOS system that uses **LF** line endings, then you don’t want Git to automatically convert them when you check out files; however, if a file with CRLF endings accidentally gets introduced, then you may want Git to fix it. You can tell Git to convert **CRLF** to **LF** on commit but not the other way around by setting core.autocrlf to input: This setup should leave you with **CRLF** endings in Windows checkouts, but **LF** endings on macOS and Linux systems and in the repository.

***主要应对 shell 脚本、bat 批处理文件等对换行符敏感的语言***

***不建议直接 copy 文件；手动创建文件 copy 文件内容相对稳妥***

***当添加配置文件或使用参数配置后，并不会生效。配置文件方式重新clone代码即可生效，`git config` 删掉除 `.git` 的全部文件,手动下载仓库代码，然后解压把代码放进去即可***

windows 安装时默认会设置为 `true`
![crlf_lf.gif](img/crlf_lf.gif)
```sh
# 配置 core.autocrlf

## **全局**
### linux & mac
git config --global core.autocrlf input
### windows
git config --global core.autocrlf true

## **单库**
### linux & mac
git config --local core.autocrlf input
### windows
git config --local core.autocrlf true
```
配置文件
```properties
# 全局 windows C:\Users\xxx\.gitconfig
[core]
	autocrlf = true

# 单库 .git/config
[core]
	autocrlf = true
```

***The `.gitattributes` file must be created in the root of the repository and committed like any other file.***

.gitattributes 
```properties
#
# https://help.github.com/articles/dealing-with-line-endings/
#
# Set the default behavior, in case people don't have core.autocrlf set.
# text=auto Git will handle the files in whatever way it thinks is best. This is a good default option
# * text=auto
# text eol=crlf Git will always convert line endings to CRLF on checkout. You should use this for files that must keep CRLF endings, even on OSX or Linux.
# * text eol=crlf
# text eol=lf Git will always convert line endings to LF on checkout. You should use this for files that must keep LF endings, even on Windows.
* text eol=lf
# Explicitly declare text files you want to always be normalized and converted
# to native line endings on checkout.
*.c text
*.h text
*.cpp text
*.hpp text
*.cmake text
*.sh text
*.md text
*.java text
*.py text
*.sql text
*.xml text
*.yml text
*.yaml text
*.properties text
*.gradle text
*.pom text
# Declare files that will always have CRLF line endings on checkout.
*.sln text eol=crlf
*.bat text eol=crlf
# Denote all files that are truly binary and should not be modified.
*.png binary
*.jpg binary
*.jpeg binary
```

刷新以应用新的配置文件
```shell
git rm --cached -r
git reset --hard
```

## tag

注意：***tag name*** 与 ***branch name*** 不能相同 \
[参考文档](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
```sh
# 列出标签
git tag
# 查找标签0.0.开头的标签，需要-l或者--list
git tag -l "0.0.*"
# 创建轻量标签 不需要-a,-s,-m选项，只需要提供标签名字
git tag 0.0.1
# 创建附注标签 可以被校验，包含打标签者的`name`，`email`，日期地址，标签信息，并且可以使用GNU Privacy Guard（GPG）签名验证
git tag -a 0.0.2 -m "0.0.2版本"
# 对某一提交码打标签
git tag -a 0.0.3 e37da745df4102b68b81d0ec681ba28e910d2344
# 查看信息
git show 0.0.1
# 推送单个tag到远程仓库
git push origin 0.0.1
# 推送全部标签到远程仓库
git push origin --tags
# 删除标签
git tag -d 0.0.1
# 删除远程仓库上的标签
git push origin --delete 0.0.1
```

## push

注意：`gitlab` 默认将 ***master*** 分支设置为 ***protect*** 不允许强制 `push -f` \
[参考文档](https://git-scm.com/docs/git-push)
```sh
# 当前分支与远程分支已关联
git push
# 推送 HEAD 当前分支 到远程已关联的分支
git push origin HEAD
# 推送 local_develop 分支到已关联的分支
git push origin local_develop
# 推送 HEAD 当前分支到远程 remote 分支
git push origin HEAD:remote
# 推送 local_develop 分支到远程 remote_develop 分支
git push origin local_develop:remote_develop
# 强制提交 风险极高 本地commit会强制覆盖远程commit
git push -f origin develop
```

## gpg

```sh
# 新增 gpg

# 根据提示输入`用户名`，`邮箱`，`密钥长度4096`,`过期时间`，`是否信任`，`github` 要注意邮箱要是 github 验证过的邮箱
gpg --full-generate-key

# 列出本地存储的所有gpg密钥信息
gpg --list-keys

# 打印公钥字符串，复制添加到git仓库
gpg --armor --export xxxx(pub key)

# 全局使用此gpg
git config --global user.signingkey {key_id}

# `-S` 表示这次提交需要使用GPG密钥签名
git commit -S -m "xxx"

# 当前仓库每次commit 时自动要求签名
git config --local commit.gpgsign true
```

### gpg 过期处理

- `gpg --list-keys` 列出本地存储的所有gpg密钥信息
- `gpg --edit-key xxxxxxx` 进入编辑模式 
    - `gpg>expire` 更新过期日期
    - `gpg>trust`  添加信任模式
    - `gpg>save`   保存
- `gpg --armor --export xxxx(pub key)` 打印公钥字符串，删除旧的，复制添加到git仓库即可


## ssh

添加 ssh 私钥 **github 需要设置密码**[参考](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

### win10 powershell

```sh
# rsa
ssh-keygen -t rsa -b 4096  -C "xxx@xx.com"

# ed25519
ssh-keygen -t ed25519 -C "xxx@xx.com"

# ed25519_sk
ssh-keygen -t ed25519-sk -C "your_email@example.com"

# ecdsa-sk
ssh-keygen -t ecdsa-sk -C "your_email@example.com"

# clip 复制到粘贴板 
cat ~/.ssh/xxx.pub | clip
```

### linux

```sh
# rsa
ssh-keygen -t rsa -b 4096  -C "xxx@xx.com"

# ed25519
ssh-keygen -t ed25519 -C "xxx@xx.com"

# ed25519_sk
ssh-keygen -t ed25519-sk -C "your_email@example.com"

# ecdsa-sk
ssh-keygen -t ecdsa-sk -C "your_email@example.com"


cat ~/.ssh/xxx.pub
```

### 多 ssh 配置 `vim ~/.ssh/config`

```text
Host git.aiclr.cn
HostName git.aiclr.cn
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/gitlab_rsa
Host aiclr.github
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/github_aiclr_rsa
Host aiclr2.github
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/github_aiclr2_rsa
```

- `Host` 替换掉真实域名 `git clone git@aiclr.github:aiclr/aiclr.github.io.git`
- `HostName` 真实域名 `git clone git@github.com:aiclr/aiclr.github.io.git`

### 检测
```shell
ssh git@git.parkere.cn
ssh git@caddyRen
ssh git@bougainvilleas
```

## reflog

[参考文档](https://git-scm.com/docs/git-reflog)

```sh
# 查看本地git操作日志 
git reflog
# 回滚到 提交码 02a3260
git reset --hard 02a3260
# 远程提交回退 
git push origin HEAD --force
```

## rebase

[参考文档](https://git-scm.com/docs/git-rebase)
 
### 将b1的基线变更为b2

```sh
#当前在b1分支
git rebase b2

# 当前不在b1分支
git rebase b2 b1
```

***仅推荐在本地分支push前合并未提交到远程的 commit，不熟悉rebase机制 易造成 git 提交顺序日期混乱***\
***有冲突，解决冲突（建议使用idea解决冲突）,将解决冲突后的文件 add 加入仓库，rebase 每个 commit 都可能会有冲突，依次解决所有 commit 的冲突***

### 冲突

```shell
git add .
#(继续)
git rebase --continue
#(跳过)
git rebase --skip
#(取消 rebase)
git rebase --abort
```

### 合并多个未提交远程 commit

注意: \
***仅限本地分支进行*** \
***切勿对已提交的 commit 合并*** \
***可使用 [reflog](#reflog) 进行回退***

```sh
# 1. 在当前分支的起始点（如提交码为12345678），rebase时以此节点为基础 rebase
git rebase -i 12345678
# 2. 自动进入 vim 编辑模式，可以看到 12345678 到当前的所有 commit 信息
# 3. 第一行 pick commit，其余行 squash。wq 保存退出vim
# 4. 编辑 git message 作为合并后的 commit message，wq保存
# 5. 有冲突手动解决后执行
git add .
git --continue
# 6. 直到所有冲突解决完成，则选择的 commit 会合并为一个 commit
```

## merge

[参考文档](https://git-scm.com/docs/git-merge)\
***merge 之前 两个分支都要先 commit***

```sh
# 当前分支 develop 将 temp 分支合并到develop
git merge temp
# 产生冲突（IDEA工具更方便处理冲突）
# 手动处理冲突代码后执行
git add 冲突文件
git commit -m "feat: merge temp into develop"
```

## init

### local

```shell
# 初始化不指定分支名称 默认创建 master 分支
git init -b <branch-name> --initial-branch=<branch-name>

git init -b develop
git config --local user.email "xxx@xx.com"
git config --local user.name "caddy"
git status
git add README.md
git commit -m "feat: init"
git log --pretty=oneline

# 添加远程分支
# 可以添加多个远程仓库 比如一份代码需要提交到两个仓库
git remote add origin https://xxx/temp.git
git remote
git fetch origin

# gitlab 默认将 master 分支设置为 protect 不允许强制 push
# git push -f 强制提交 风险极高 本地commit会强制覆盖远程commit
git push -f origin develop
```

### gitlab

#### Create a new repository

```shell
git clone URL
cd testcaddy
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

#### Push an existing folder

```shell
cd existing_folder
git init
git remote add origin URL
git add .
git commit -m "Initial commit"
git push -u origin master
```

#### Push an existing Git repository

```shell
cd existing_repo
git remote rename origin old-origin
git remote add origin URL
git push -u origin --all
git push -u origin --tags
```

## commit

```sh
# 修改 commit message

# 未提交 远程 
git commit --amend

# 已提交 远程
git reset --soft HEAD~1
git commit -m "new message"
git push -f origin branchname
```

## show

```sh
# 查看其他分支文件
git show develop:README.md
```

## clone

```sh
git clone git@github.com:aiclr/aiclr.github.io.git
git clone https://github.com/aiclr/aiclr.github.io.git
git clone git@github.com:aiclr/aiclr.github.io.git -b develop
```

## branch

```sh
# 查
## 查看本地分支
git branch
## 查看远程分支
git branch -r
## 查看全部分支
git branch -a
## 查看分支详情
git branch -v -a

# 增
## 创建分支但不切换
### 以当前分支为基线创建 temp 分支
git branch temp
### 以 base 为基线创建 temp 分支, base 可以为 tag 或 提交码
git branch temp base

## 仅切换
git checkout temp

## 创建并切换
### 以当前分支为基线创建 temp 分支,并切换到 temp 分支
git checkout -b temp
### 以base为基线创建 temp 分支并切换到 temp,base 可以为 tag 或 提交码
git checkout -b temp base
### 以远程仓库 origin 内的 develop 分支为基线创建 develop 分支，并切换到 develop 分支
git checkout -b develop origin/develop

# 删
## 远程分支删除后，本地同步清理
git remote prune origin
## 删除本地 temp 分支:
### `-d`delete fully merged branch
### `-D` delete branch (even if not merged)
git branch -d temp

## 删除远程 `master` 分支
git push origin --delete master

# 改
## Rename your local `master` branch into `main` with:
git branch --move master main
## 将当前分支与远程 main 分支绑定
git push --set-upstream origin main
```

## [推荐阅读 git-flow 相关博客](https://www.cnblogs.com/wish123/p/9785101.html)

分支名称规范
- `master` or `main` 主分支
- `hotfix`
    - 生产环境版本紧急缺陷修复发布分支
    - 由 master 分支 fork
    - 可 merge 到 develop、master,完全 merge 后可删除分支
- `release`
    - 灰度发布分支、公测发布分支
    - 由 develop fork
    - 可 merge 到 master，完全merge后可移除当前分支
    - 除了修复 bug 提交外不可 commit 其他内容，
- `develop`
    - 开发主轴
    - 不建议直接 commit
    - 由 master 或 hotfix 分支 fork
    - 可 merge 到 release
    - 可从 release、hotfix、feature 分支merge
- `feature`
    - 新功能分支
    - 可 merge 到 develop,完全 merge 后可删除分支

commit message type
- `feat` 新功能
- `fixBug` 修复bug
- `docs` 文档相关
- `style` 代码格式化相关
- `refactor` 重构
- `perf` 性能优化
- `test` 测试相关
- `build` 构建系统或包依赖相关
- `ci` CI配置，脚本文件相关
- `chore` c库或测试文件相关
- `revert` commit 回退