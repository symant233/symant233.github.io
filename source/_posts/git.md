---
title: Git学习记录
date: 2020-04-05 20:32:20
tags:
 - git
categories:
---

## 0x0 前言

没系统学过git，老是有一些问题就谷歌，解决完问题又没时间记，感觉不是个办法。正好看到一篇敲详细的文章（感谢作者整理），所以决定重头学习一遍并且把**难点**记录下来。第一次写文章🔰，如果文章有错或者有其它问题**欢迎评论指正**！

***
## 0x1 Git概述

### Git的四个组成部分

详情见[来源](https://juejin.im/post/5e79ea975188255e277a3892#heading-3) 
// `stash`区域应该算是一个独立的部分，不受这些命令干扰。

![](https://user-gold-cdn.xitu.io/2020/4/2/17139c44acec736d?w=797&h=174&f=png&s=78087)


### Git中的四类对象

> `Blob`（块）对象，`Tree`（树）对象，`Commit`（提交）对象，`Tag`（标签）对象。 ----[来源](https://juejin.im/post/5e79ea975188255e277a3892#heading-5)

![](https://user-gold-cdn.xitu.io/2020/4/2/17139d5cb4235c46?w=500&h=338&f=png&s=118010)

每一个对象都会有一个对应的 hash 值 (SHA-1)。
```bash
git hash-object <file> # 计算blob的hash
git cat-file [-p|-t] <hash> # -p查看对象内容 -t查看对象类型
```
`blob`一听就知道是个二进制，它只存文件内容。文件名等信息的就在`tree`里，很像操作系统的目录表结构。而树对象不仅可以引用`blob`，还能引用`tree`构成一个多层目录结构，这点像很像文件夹。

```bash
git ls-tree <hash> # 查看树对象内容
```

一个`commit`中只记录一个树，鉴于树的套娃结构，`commit`里对应的是最根部的树对象。`commit`会有一个很短的值，那其实不是完整的 hash 值，只是取前几位作为了缩写（只要唯一就行）。

***
## 0x2 Git配置
### git config
- `system`: win 端好像都是`C:/Program Files/Git/mingw64/etc/gitconfig`
- `global`: win 端地址`C:/Users/<username>/.gitconfig`
- `local`: 项目中`.git/config`

三种级别配置文件，就近覆盖生效。`local > global > system` 一般都用 global

```bash
# 配置代理：
git config --global http.proxy 'http://127.0.0.1:1080' # socks5:// 好像也可以
git config --global --unset http.proxy  # 取消代理
# 设置别名如：
git config --global alias.st 'status -s -u' # 之后可用 git st
```
这样配置在文件中是这样：（我的`.gitconfig`）
```bash
[github]
    user = <github username>
    token = <personal access token>
[http]
    proxy = http://127.0.0.1:1080
[alias]
    st = status -s -u
    cm = commit -m
    lg = log --graph --oneline --abbrev-commit
```
> 注意：设置了 2FA 两步验证则无法使用`[user]`密码登录，必须提供 token 作为凭据。


### .gitignore
Github 官方有专门的一个 [仓库](https://github.com/github/gitignore) 给出各语言的模板。下方匹配模式摘自 [来源](https://juejin.im/post/5e79ea975188255e277a3892#heading-12)。
- `*`：零个或多个任意字符
- `[abc]`：只匹配括号内中的任意一字符
- `[0-9]`：范围内任一字符
- `?`：任意一字符
- `**`：任意的中间目录
```
# .gitignore
!index.d.ts    # 除了这个文件
dir/           # 忽略该文件夹下所有
dir2/**/*.o    # 所有dir2下子文件中的.o文件
```
已经被 tracked 的文件即使加入`.gitignore`也无法解除追踪，[StackOverflow](https://stackoverflow.com/questions/11451535/gitignore-is-ignored-by-git#11451731)：
```!
先 commit 你的当前更改再进行操作，否则更改会丢失！在项目根目录执行：
```
```bash
git rm -r --cached . ; git add .
```

***
## 0x3 Git提交
### git add
```bash
git add .             # 所有更改文件放入暂存区
git add <pathspec>    # 可使用匹配字符'./index.js'或如'*.js', 'dir/*'
git add -u|--update   # 添加更新的 不包括新增文件(untracked)
git add -A|--all      # 包括新增文件
git add -i            # 交互模式
```
unstage 取消暂存：
```bash
git reset -q <file>     # unstage file
```
### git commit
```bash
git commit -m "<msg>"           # 提交已暂存更改
git commit -a|-all -m "<msg>"   # 提交所有更改(含未暂存)
git commit -p|-patch            # 交互模式
```
### git tag
`tag`对像有两种，轻量标签直接引用 commit，附加标签创建 tag 对象，实质都是对`commit`的引用。
对重要的提交打上 Tag，可以方便找到对应 commit ，如 release 新版本时。
```bash
# 创建 tag
git tag <version>                  # 轻量级标签（lightweight）
git tag -a <version> -m "<msg>"    # 含附注标签（annotated）
git tag -a <version> <commit_id>   # 追加标签 id 可为前7位简写
# 删除 tag
git tag -d <version>               # 删除本地 tag
git push origin -d <version>       # 删除远程 tag，命令中 tag 可以省略
```
> 如果想要使用 GPG 签名，将 -a 换成 -s 即可，验证签名用 -v 。加 -f 强制覆盖。

另外，git push 的时候默认不会把标签推送到远程仓库，如果想把标签页推送到远程仓库，可以：
```bash
git push origin <version>    # 推送某标签远程
git push origin --tags       # 推送所有 refs/tags 下的标签
```
***
## 0x4 Git分支
### 分支概述
分支并不是 Git 对象，和轻量级的 tag 对象类似，是对一个commit的引用。

当 branch 更新后会指向最新的 commit 。而 tag 则不会（除非`--force`）。
- 本地分支：`.git/refs/heads/`
- 远程分支：`.git/refs/remotes/`
- HEAD：`.git/HEAD` 指向**当前**工作区的**本地分支**

> 每个 commit 对象有一个 parent 属性值指向父 commit 的哈希值，由此可得到变化路径。

- 场景0：多人合作项目是私有项目，无法 fork，每个人对各自的任务创建分支。
- 场景1：master 要拿来持续部署/发行正式版，必须保证 master 的可靠稳定，创建 dev 分支专门接受其余各分支的 pr，经测试后再推送到 master。
- 场景2：大版本更新，备份一个之前版本到分支，因为要迁移所以旧版本会仍然有人在用。如果旧版本出现问题，可以直接到对应分支进行修复。

### git branch
```bash
git branch -a -v  # 查看所有分支（远程+本地，显示每个分支最新 commit）
git branch -m 老分支名 新分支名   # 分支重命名

git branch <branch>         # 创建新分支
git push origin <branch>    # 推送分支到远端

git branch -d <branch>      # 删除分支，如有未提交更改使用 -D 强行删除
git push origin -d <branch> # 删除远程分支（老版本需全写 delete）
```
```bash
git checkout <branch>            # 切换分支
git checkout -t origin/<branch>  # 创建远程分支的本地分支
```
恢复误删分支 [StackOverflow](https://stackoverflow.com/questions/3640764/can-i-recover-a-branch-after-its-deletion-in-git#3640806)：
```bash
git reflog   # 检索出被删分支哈希
git checkout -b <branch> <hash>
```

### git stash
切换分支时有未提交更改，而更改未完成不想直接提交，可以使用**储藏**来暂时保存更改。
储藏是一个栈结构，`stash@{0}`指向最新的储藏，`stash@{1}`则为上上个储藏，以此类推。
```bash
git stash (push)   # 储藏当前未提交更改，含已暂存（push 可省略）
git stash list     # 查看储藏栈
# 以下命令后都可接'<stash>'，如'stash@{2}'，未提供则默认使用最新'stash{0}'
git stash show     # 查看存储更改
git stash apply    # 只应用储藏
git stash drop     # 丢弃对应储藏
git stash pop      # 弹出，等同于 apply+drop
```

### git merge

<div align=center><img src="https://user-gold-cdn.xitu.io/2020/4/4/17143bc9ba03a78f?w=956&h=846&f=png&s=62043" style="zoom:60%" /></div>

右侧为 **快速合并**（Fast-Forward-Merge），要求被提交分支无其它 commit。[图源](https://nvie.com/posts/a-successful-git-branching-model/#incorporating-a-finished-feature-on-develop)

```bash
git merge -ff                # 快速合并（默认）
git merge -ff-only           # 只有快速合并的情况才合并
git merge --no-ff            # 不使用快速合并
git merge -n/-stat <branch>  # 合并分支，合并后显示/不显示前后的不同状态
git merge -e <branch>        # 合并前调用编辑器，可自行编写 commit 或使用 -m "<msg>"
```
> 使用`--squash`将合并压缩为单个 commit。（见 merge reference [--squash](https://git-scm.com/docs/git-merge#Documentation/git-merge.txt---squash)）

### git rebase
看到 [这篇文章](https://juejin.im/post/5e84797ce51d4546ff6fe3ac#heading-2) 写的不错，可以去看看。下面是我从 [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN) 做的练习。

![](https://user-gold-cdn.xitu.io/2020/4/5/171494be1997176a?w=1215&h=413&f=png&s=87831)

在 dev 分支上想合并到主分支，使用`git rebase master`将 dev 分支变基到 master 上。此时还未做完，因为 master 还未指向最新 commit。需要切换到 master 再使用`git rebase dev`。如果操作中有 conflict 则需手动解决冲突后 `git rebase --continue`。

> 注意原先的 c2 commit 并不是丢失了，它仍存在历史中。

```bash
git rebase A_branch B_branch  # 在B分支上执行 git rebase A_branch
git rebase -i <branch>        # 进入交互模式
git rebase --abort            # 中断变基
```
交互模式只需更改左侧`pick`为想要的命令然后保存退出，其后每一步都会有提示。如果搞砸了可以`git reset --hard origin/<branch>`从远端覆盖变更。

***
## 0x5 Git远程
### git remote
```bash
# 本地初始化 git
git init
# 添加远程仓库地址（默认称为为 origin）
git remote add origin https://github.com/<user>/<repo>.git 
# 将 master 与 origin/master 关联
git push -u origin master
# -u：首次关联远程分支与本地分支，后续提交不需要。
```
- `origin` 只是一个远程仓库链接地址的别名，特别在它是默认的，还可添加其他远程地址。
- `upstream` 指向 fork 的原项目地址。

若远程还有其他分支，如`origin/dev`，这种远程地址是不可更改的（只在同步的时候更改指向的 commit）。如果需要对该分支进行更改则需要建立**本地**的跟踪/关联分支：
```bash
git checkout -b dev origin/dev
git checkout -t origin/dev  # 同上，全写 --track
```

### git fetch
```bash
git fetch origin  # 拉取远程到本地 但是不会改变工作区
git merge origin/<branch>  # 还需要手动将远程更改合并到本地
git pull # 等于 fetch + merge，可能会产生冲突。
```

### git push
```bash
git push (origin)                  # 推送所有更改（默认）到 origin
git push -u origin <branch>        # 将当前工作分支绑定远程 branch
git push origin <tag>|<branch>     # 将本地 tag 或 branch 推送远端
git push origin -d <tag>|<branch>  # 删除远程 tag 或 branch
```

***
## 0x6 Git回退
HEAD一般指向当前分支的最顶端，但也可指向特定的 revision，此时称之为
[detached HEAD](https://git-scm.com/docs/git-checkout#_detached_head)。

- `HEAD^n` 指向上n个提交记录，也可省略n，`HEAD^^`表示上两个提交记录。
- `HEAD~n` 同上但不可使用多个`~`。链式操作如 `HEAD^3~2^^`

### git reset
```bash
# unstage file
git reset -q <file>
git reset --|HEAD <file>
# 回退到上一个 commit
git reset HEAD^  # 上一个 commit 所有文件 unstage
git reset --soft|mixed|hard HEAD^
```
- soft：将回退的 commit 更改退回到暂存区中，工作区不变。
- mixed：默认，将已有的暂存区取消暂存，将回退的更改退回工作区。
- hard：丢弃回退的更改以及暂存区工作区更改。

### git revert
若推送上远程后再用 reset 则产生冲突，revert 将撤销更改作为一个新的 commit。
```bash
git revert HEAD # 撤销最近的一个 commit（即当前指向）
git revert HEAD~n # 撤销前第 n 个提交
git revert <commit_id> # 只撤销该提交，后续提交不影响
git revert <oldest_commit_id>..<latest_commit_id> # 撤销范围提交
```
```!
已经推送到远程的提交，是无法彻底撤回的（历史记录仍在）。
```

### git checkout
```bash
# 丢弃未暂存更改（无法找回）
git checkout -q <file>
git checkout --|HEAD <file>
```

***
## 0x7 Git查询
### git diff
```bash
git diff                    # 工作区与暂存区差异
git diff <branch>           # 工作区与某分支差异，远程分支这样写：remotes/origin/分支名
git diff HEAD               # 工作区与HEAD指针指向的内容差异
git diff <commit id> <file> # 工作区某文件当前版本与历史版本的差异
git diff --stage            # 工作区文件与上次提交的差异(1.6 版本前用 --cached)
git diff <tag>              # 查看从某个版本后都改动内容
git diff 分支A 分支B        # 比较从分支A和分支B的差异(也支持比较两个TAG)
git diff 分支A...分支B      # 比较两分支在分开后各自的改动
```
> 注：如果只想统计哪些文件被改动，多少行被改动，可以添加--stat参数。搬运自[来源](https://juejin.im/post/5e79ea975188255e277a3892#heading-15)

### git log
```bash
git log --graph          # 分支历史情况
git log --oneline        # 一行精简输出
git log --abbrev-commit  # 缩短哈希长度
```
> `git reflog`：Reference logs, or "reflogs", record when the tips of branches and other references were updated in the local repository.  ----[git-scm](https://git-scm.com/docs/git-reflog)

***
## 0x8 其它
```
git cherry-pick <commit——id>  # 取特定 commit 追加到当前分支
gitk  # 图形化界面
git config color.ui true  # 彩色的 git 输出
git config format.pretty oneline  # 显示历史记录时，只显示一行注释信息
```


参考：
- coder-pig  [《吐血整理》一篇文章教你学废Git版本管理](https://juejin.im/post/5e79ea975188255e277a3892)
- Scott Chacon & Ben Straub [Pro Git 中译版](https://0532.gitbooks.io/progit/content/)
- Git 官方文档（git-scm）  [reference](https://git-scm.com/docs)
- Vincent Driessen [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

推荐：
- 腾讯IMWeb团队 [十分钟了解git那些“不常用”命令](https://juejin.im/post/5e84797ce51d4546ff6fe3ac) （`git rebase`）
- 图形化 Git 分支练习 [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)
- atlassian [Git Workflows and Tutorials](https://github.com/oldratlee/translations/tree/master/git-workflows-and-tutorials)
<div style="text-align: right">抓狸（@symant233）<b>禁止非授权转载</b></div>

***