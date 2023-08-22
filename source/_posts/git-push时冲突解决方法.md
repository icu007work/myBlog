---
title: git-push时冲突解决方法
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva1.sinaimg.cn/large/87c01ec7gy1fsnqqm5vu5j21kw0w0aon.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/17eaa9871718b.jpg
tags:
  - git
  - 随笔
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
abbrlink: 80c83deb
date: 2022-08-26 09:10:27
authorAbout:
authorDesc:
keywords:
description:
---

## 一、冲突原因

### 1.1 多人同时修改同一文件

```
 liaochongrong@urovo002-07:/home/ssd7/lcr_work/SQ45S$ git cherry-pick -n 4a7c5af22778e57440fdf09921b25e1a40a6d5e0
 
 error: 不能应用 4a7c5af... Product:SQ45S
 提示：冲突解决完毕后，用 'git add <路径>' 或 'git rm <路径>'
 提示：命令标记修正后的文件
 
 liaochongrong@urovo002-07:/home/ssd7/lcr_work/SQ45S$ git status
 位于分支 Pie_SQ45S_Release
 您的分支与上游分支 'origin/Pie_SQ45S_Release' 一致。
 未合并的路径：
   （使用 "git reset HEAD <文件>..." 以取消暂存）
   （使用 "git add <文件>..." 标记解决方案）
 
     双方修改：   build/buildprop.mk
 
 尚未暂存以备提交的变更：
   （使用 "git add <文件>..." 更新要提交的内容）
   （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

 
 
 
```

### 1.2 缺少change-id

```
 liaochongrong@urovo002-07:/home/ssd7/lcr_work/SQ45S$ git push origin HEAD:refs/for/Pie_SQ45S_Release
 对象计数中: 4, 完成.
 Delta compression using up to 64 threads.
 压缩对象中: 100% (4/4), 完成.
 写入对象中: 100% (4/4), 656 bytes | 0 bytes/s, 完成.
 Total 4 (delta 3), reused 0 (delta 0)
 remote: Resolving deltas: 100% (3/3)
 remote: Counting objects: 4, done
 remote: Processing changes: refs: 1, done    
 remote: ERROR: [19bc4b1] missing Change-Id in commit message footer
 remote: 
 remote: Hint: To automatically insert Change-Id, install the hook:
 remote:   gitdir=$(git rev-parse --git-dir); scp -p -P 29418 liaochongrong@192.168.8.215:hooks/commit-msg ${gitdir}/hooks/
 remote: And then amend the commit:
 remote:   git commit --amend
 remote: 
 To ssh://192.168.8.215:29418/NoUcode/SQ45S
  ! [remote rejected] HEAD -> refs/for/Pie_SQ45S_Release ([19bc4b1] missing Change-Id in commit message footer)
 error: 无法推送一些引用到 'ssh://192.168.8.215:29418/NoUcode/SQ45S'

 
 
 
```

![差异](https://m.360buyimg.com/babel/jfs/t1/61562/20/19916/87101/630746a4E781cd8f2/30ce7c00b6bce0a7.png)

## 二、解决方案

### 2.1 多人修改

- 通过 `git status`命令找到双方共同修改的文件，然后编辑文件，把多余的代码去掉即可。
- 可通过 `git diff 文件目录`来查看显示已写入暂存区和已经被修改但尚未写入暂存区文件的区别

![差异](https://m.360buyimg.com/babel/jfs/t1/132834/9/26175/55311/63074527Ebe1d9bb2/5731e9aaa5eb446d.png)

- 如图，删除标识代码，此冲突即可解决。

### 2.2 缺少change-id

- 其实这个冲突的解决方法，git已经提示过我们了。就是这一段

```
 remote: Hint: To automatically insert Change-Id, install the hook:
 remote:   gitdir=$(git rev-parse --git-dir); scp -p -P 29418 liaochongrong@192.168.8.215:hooks/commit-msg ${gitdir}/hooks/
 remote: And then amend the commit:
 remote:   git commit --amend

 
 
 
```

- 所以我们只需要执行 `gitdir=$(git rev-parse --git-dir); scp -p -P 29418 liaochongrong@192.168.8.215:hooks/commit-msg ${gitdir}/hooks/`以及 `git commit --amend`这两行命令即可。
- 当然如果不放心还可以reset 已有的提交：`git reset --soft 3bf39e60e2cad62f3ada0414f3cef64f386ccce3`，最后那一串为 commit id，reset完了之后，再重新进行提交操作。操作完成后即可看到change-id已经生成了。
