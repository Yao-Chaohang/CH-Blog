---
title: git常用命令
date: 2022-12-18 21:24:29
tags: Git
categories: 开发工具
comments: false
---

###### 本地初始化仓库

```
git init
```

###### 克隆远程仓库

```
git clone 远程仓库地址
```

###### 克隆某个分支的代码到本地

```
git clone -b 分支名 远程仓库地址
```

###### 查看本地git状态

```
git status
```

###### 将某个文件或所有文件添加暂存区

```
git add 文件名 // 将某个文件添加暂存区
git add . // 将所有文件添加暂存区
```

###### 插销对某个文件的修改

```
git checkout -- file
```

###### 提交暂存区的文件

```
git commit -m 备注信息
```

###### 快捷指令查看git常用命令

```
git helper -a
```

###### 逐行查看某个文件的修改历史

```
git blame 文件名 // 查看该文件的修改历史
git blame -L 100,10 文件名 // 从100行开始，到110行 逐行查看文件的修改历史
```

###### 清除

```
git clean -n // 列出打算清除的档案(首先会对工作区的内容进行提示)
git clean -f // 真正的删除
git clean -x -f // 连.gitignore中忽略的档案也删除
git status -sb (sb是 short branch) // 简洁的输出git status中的信息
```

###### 删除放入暂存区文件的方法（已经提交之后的）

```
git rm 文件名 // 将该文件从commit后撤回到add后
git reset HEAD^ --hard // 删除后 可以用git rm 文件名再回撤一步
```

###### 修改文件名以及移动

```
git mv a b // 把a文件名字改成b 并且直接放入git add后的暂存区
git mv b ./demos/ // 把b文件移动到demos文件夹下
```

###### 对比工作区、暂存区、仓库的差异

```
git diff // 查看变更 工作区与暂存区的差异比对
git diff --cached // 暂存区与提交版本的差异
git diff HEAD // 工作区与仓库中最后一次提交版本的差别
git diff 版本哈希值 版本哈希值 // 查看这2个版本哈希之间的区别
或者 git diff HEAD~数字 HEAD~数字
 
git diff tt 就是倒数第5个版本与第一个版本之间的差异
git diff --cached tt 暂存区与倒数第5个版本之间的比对
```

###### 查看提交信息

```
git show HEAD // 查看最后一次提交修改的详细信息 也可以用git show 哈希值 查看对应的内容
git show HEAD^ // 查看倒数第二次的提交修改详细信息
git show HEAD^^ 或者git show HEAD~2 查看前2次变更
git show HEAD 或 git show 哈希值 或者git show tag(标签名) 都可以查看最近一次提交的详细信息
```

###### 查看信息

```
git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
// 获取git log里的树形详细信息 包括hasg 日期 提交信息 提交人等
git log --oneline //拉出所有提交信息 q是退出
git log -5 // 查看前5次的提交记录
git log --oneline -5 // 打印出的日志里面只有哈希值和修改的内容备注
git log 文件名 // 查看该文件的提交
git log --grep // 想过滤看到的内容   过滤日志
git log -n // 查看近期提交的n条信息内容
git log -p // 查看详细提交记录
```

###### 改写历史提交，把多次提交合并起来

```
git rebase -i HEAD~3 变基之后的哈希值与之前的不同 证明变基是重新做的提交 把多次提交合并成了几次提交
```

###### 回滚

```
git commit --amend -m "提交信息" // 回撤上一次提交并与本次工作区一起提交
git reset HEAD~2 --hard // 回撤2步
git reset --files // 从仓库回撤到暂存区
git reset HEAD // 回撤暂存区内容到工作目录
git reset HEAD --soft 回撤提交到暂存区
git reset HEAD --hard // 回撤提交 放弃变更 (慎用)
git reset HEAD^  // 回撤仓库最后一次提交
git reset --soft HEAD^ // 将执行git commit 并没有执行git push到远程仓库的内容 回撤到工作区 并且保存在工作区
git reset --hard HEAD^ // 将执行git commit 并没有执行git push到远程仓库的内容 回撤并且不保存
// 注意 在window电脑端 可能会出现执行git reset --hard HEAD^命令时 提示More? 所以针对windows 我们回撤内容需要键入git reset --hard HEAD^^才可以 如果我们git commit提交2次 想把2次都回撤到工作区保存 可以使用git reset --soft HEAD~2
git reset --hard commitid // 回撤到该次提交id的位置 回撤后本地暂存区可能有内容 本地仓库有要同步的内容 此时 丢弃掉暂存区的内容 并且强制将本地的内容推送至远程仓库 执行下面的命令 git push -u -f origin 分支名 这样就可以完全回撤到提交id的位置
git reset --soft commitid // 回撤到该次提交id的位置 并将回撤内容保存在暂存区
git push -f -u origin 分支名 所有内容都回撤完了 将回撤后的操作强制推送到远程分支
git push origin/分支名 --force 强制将本地回撤后的操作 强制推送到远程分支
```

###### 标签

```
git tag // 查看列出所有打过的标签名 例如V1.1 V1.11 V1.12 V1.13等
git tag -d 标签名 // 删除对应标签 只是删除了本地的
git push origin :refs/tags/远程标签名 // 删除远程仓库的标签 可以在删除本地标签后 执行这个操作 同步远程
git tag 标签名字 // 在当前仓库打个标签
git tag 标签名 commitid // 给已知提交id的版本打标签 例如git tag v1.1.1 6f8f25fcf57a17e6c72b33f6bca0797fab15ff8b // 给历史提交打V1.1.1的tag标签 这里的commitid可以缩写 缩写成前6位就可以 例如git tag V1.1.1 6f8f25 一样可以给这个提交id打上tag
git tag -l // 过滤tag 例如 git tag -l "V1.1*" // V1.1 V1.11 可以过滤前面是V1.1开头的内容
git show 标签名称 // 查看tag的详细信息 包括commitid 作者信息 日期 内容
git push origin 标签名称 // 同步这个tag到远程服务器 默认tag是打在本地的 这个命令可以把它推到远程
git push origin --tags // 将本地所有tag推送到远程服务器
git pull --tags // 把远程仓库的标签也拉取下来
git tag foo -m "message" // 在当前提交上，打标签foo 并给message信息注释
git tag 标签名 哈希值 -m "message" // 在某个哈希值上打标签并且写上标签的信息
git tag foo HEAD~4 // 在当前提交之前的第4个版本上 打标签foo
 
 
git stash // 把暂存区的内容 暂时放在其他中 使暂存区变空
git stash list // 查看stash了哪些存储
git stash pop // 将stash中的内容恢复到当前目录，将缓存堆栈中的对应stash删除
git stash apply // 将stash中的内容恢复到当前目录，不会将缓存堆栈中的对应stash删除
git stash clear // 删除所有缓存的stash
git reset --hard // 回撤git stash pop的内容
```

###### 分支

```
git branch 分支名 // 新建分支
git branch // 查看当前所有分支
git checkout 分支名 // 检出分支
git checkout -b 分支名 // 创建并切换分支
git checkout commitId 文件名（文件路径下的文件名） 还原这个文件到对应的commitId的版本
（例如src/page/attendance/attendanceSum.vue我想把它还原到2个版本之前 首先git log src/page/attendance/attendanceSum.vue找到对应想要还原的版本
复制版本提交的commitID 然后执行git checkout commitID src/page/attendance/attendanceSum.vue
这样就把attendanceSum.vue这个单个文件 还原到了对应版本）
git branch -v // 查看分支以及提交hash值和commit信息
git merge 分支名 // 把该分支的内容合并到现有分支上
git cherry-pick commitId // 把其他分支的某一次提交内容合并到当前分支 这个在我们平时多分支开发中很常用
git branch -d 分支名 // 删除分支
git branch -D 分支名 // 强制删除 若没有其他分支合并就删除 d会提示 D不会
git branch -m 旧分支名 新分支名 // 修改分支名
git branch -M 旧分支名 新分支名 // 修改分支名 M强制修改 若与其他分支有冲突也会创建(慎用)
git branch -r // 列出远程分支(远程所有分支名)
git branch -a // 查看远程分支(列出远程分支以及本地分支名 远程分支会以remote/origin/分支名这种形式展示 红色标识)
git branch // 查看本地分支
git reflog show --date=iso <branch name> // 查看分支创建时间 例如git reflog show --date=iso origin/feature-PCDEC-6375 输出 88e22885 (HEAD -> feature-PCDEC-6375, origin/feature-PCDEC-6375, origin/EC-master, EC-master) refs/remotes/origin/feature-PCDEC-6375@{2021-07-27 11:31:23 +0800}: fetch: storing head 创建时间就是2021-07-27 11:31:23
git fetch // 更新remote索引
git push -u origin 分支名 // 将本地分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push 也可解决 git建立远程分支关联时出现fatal ... upstram的问题
git push origin --delete 分支名 (将git branch -D 分支名 删掉的分支 同步到远程主机 将origin/分支名的该分支也删除掉)
git remote show origin 查看remote地址，远程分支，还有本地分支与之相对应关系等信息(结合git branch -a使用)
git remote prune origin 删除远程仓库不存在的分支 (git branch -a使用)
git reflog show --date=iso 分支名 // 查看指定分支的创建时间 以及更改记录等
```

###### 仓库迁移

```
// 首先在当前项目主分支先执行git pull 把代码更新为最新
git remote set-url origin <新的仓库名>
git push -u -f origin
git push -u -f origin --all // 把所有分支迁移过去
git push -u -f origin --tags // 把所有tag迁移过去
// 然后去拉取新的仓库代码就可以了 如果新仓库之前拉取过了
重新仓库迁移 里面分支没同步的话 执行 git fetch试一下 同步过来
```

###### 强制推送

```
git push -- force
```

###### 打开git GUI

```
gitk // 用git命令快速打开git GUI
```

###### config以及配置

```
git config --list // 获取config信息
git config --global core.safecrlf false // 去掉git add 命令后 出现的一堆CR LF提示信息
其中CR是回车的意思 LF是换行
git config --global user.name"your name" // 设置username
git config --global user.email"your_email@youremail.com" // 设置邮箱
git config --global credential.helper wincred // 存储凭证 (可用于输入一次用户密码后，不再输入 有时我们已经用SSH key 绑定关联好了 但是每次git提交的时候 还是需要你输入用户名密码 在这个时候 敲入这个命令 将凭证存储起来 用户名密码就不需要再次输入了)
git config --global alias.ci commit // 将commit命令设置别名ci git commit命令将由git ci来代替
```
