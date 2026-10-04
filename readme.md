aloha

# Module 0: Programming Best Practices — Git / GitHub 学习总结

## 一、Git 和 GitHub 是什么

- Git 是一个分布式版本控制工具，装在本地电脑上。它记录文件每一次变化，可以随时回到任意一个历史版本。

- GitHub 是一个网站，用来托管 Git 仓库。它给 Git 仓库提供一个远程的存放地点，方便备份、协作和评分。

- Git就是一个本地存档系统，而GitHub是一个云端仓库托管网站。Git 不联网也能用，GitHub 只是让仓库有一个远程副本。


## 二、四个区域

Git 把文件分成四个区域，数据从左向右流动：工作区->暂存区->本地仓库->远程仓库

- 工作区：当前文件夹里直接看到的文件。
- 暂存区：准备提交的改动，用 git add 放进去。
- 本地仓库：commit 之后存在 .git 文件夹里的历史。
- 远程仓库：GitHub 上的仓库，用 git push 上传。git push时记得打开加速器，否则可能无法稳定连接GitHub。


## 三、Git 内部是怎么组织版本的

Git 是一个内容寻址的文件系统，版本历史是一张有向无环图（DAG）。核心对象有四种：

- blob: 一个文件的内容
- tree: 一个目录，记录文件名和对应的 blob
- commit: 一次提交，指向一个 tree 和父 commit
- tag: 给某个 commit 起的别名，比如版本号

每个对象都有一个 SHA-1 哈希，比如 7e4dd5a。内容改了，哈希就完全不同，所以历史不可篡改。

commit 之间通过“父 commit”串起来，形成历史。

分支只是一个指向某个 commit 的指针，非常轻量。创建分支就是新建一个指针文件，不复制任何文件。

HEAD 是“你现在在哪”的指针：HEAD -> main -> 29fef44指向一个分支；或者HEAD -> 7e4dd5a，这叫detached HEAD，直接指向一个 commit。


## 四、常用命令和效果

查看状态：git status，显示当前分支、改动、暂存区状态。

查看分支：git branch，列出本地分支；git branch -a，列出本地和远程分支；git branch --show-current，只看当前分支名。

查看历史：git log --oneline --graph --all，一行一个 commit，显示分支图形和所有分支；git log --oneline -1，只看当前 HEAD 所在的 commit；git show <某个commit的hash>:文件名，查看某个 commit 里某个文件的内容。

暂存和提交：git add <文件>，把文件放进暂存区；git add <文件1> <文件2>，一次加多个；git commit -m "说明"，生成一个 commit，其中-m 后面是提交说明，必须写清楚做了什么。

分支操作：git switch -c for_fun，创建并切换到新分支for_fun，其中-c表示需要创建；git switch main，切换到已有分支main；git merge for_fun，把 for_fun 合并到当前分支。

远程操作：git clone <某个远程仓库的URL>，把远程仓库下载到本地；git push -u origin main，首次推送并绑定上游git push origin main，之后直接推送；git pull，拉取远程更新；git remote -v，查看远程地址

回到历史版本：git checkout <某个commit的hash>，让 HEAD 指向某个 commit，会进入 detached HEAD 状态，不要在这里 commit；git switch main，切回分支main。

查看 HEAD 移动记录：git reflog --date=iso，记录 HEAD 的每次移动，只存在本地。


## 五、merge 的四种结果

| 情况 | Git 行为 |
|------------------|--------------------|
| 当前分支已包含目标分支所有提交 | Already up to date. |
| 当前分支无新提交，目标分支有新提交 | Fast-forward，指针前移 |
| 两边都有各自的新提交 | 生成一个新的 merge commit |
| 两边改了同一文件的同一位置 | CONFLICT，需手动解决 |


## 六、这次学到的经验

1. 改文件后要 git add 再 git commit，才会形成存档。
2. 只有 git push 才会把存档上传到 GitHub。
3. 分支只是指针，创建和切换非常快，放心用。
4. detached HEAD 是 HEAD 直接指向 commit。
5. 不希望git存的内容可以用 .gitignore 忽略。.gitignore也要add、commit、push。
6. commit message 要写清楚做了什么，评分和日后回看都靠它。
7. 国内直连 GitHub 不稳定，可以用加速器。
8. Hugging Face 下载慢，可设置
```
import os
os.environ["HF_ENDPOINT"] = "https://hf-mirror.com"
```
使用国内镜像。