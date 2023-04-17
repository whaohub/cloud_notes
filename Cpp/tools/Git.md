# Git

[toc]

## git clone时遇到问题

证书校验有问题 ：server certificate verification failed. CAfile: none CRLfile: none
Linux 解决方法:加环境变量

```
export GIT_SSL_NO_VERIFY=1
```

## git基本命令操作

```shell
git config --global credential.helper store  //配置每次输入帐号密码问题,只在第一次输入即可

git init    //初始化git

git add filename  //添加文件

git commit -m "breaf" //提交到本地仓库

git push origin master   //上传到远程仓库地址，并输入账户密码

git push --force-with-lease，使用此参数推送，如果远端有其他人推送了新的提交，那么推送将被拒绝。

git push -u origin master --将本地仓库的master分支提交到远程origin的master分支
本地仓库可能存在多条分支，怎么把另外的分支上传到远程仓库？
方法一 git push -u origin x:x --将本地x的分支提交到远程origin的x分支 左边代表源头，右边代表目的地
方法二 git checkout x --切换分支 git push -u origin x
```

## 远程覆盖本地

```shell
git fetch --all
git reset --hard origin/master 
```

## git 版本管理

### git 撤销

* git reset --hard 
  git撤销push到远端的commit修改.

先在本地回退到相应的版本:

```
git reset --hard <版本号>
--hard 会抛弃当前工作区的修改
--soft 参数会回退到之前的版本，但是保留当前修改
```

此时使用git push origin <branch> ,会提示本地分支落后远程分支，需要强推

* git reset --soft
  soft （修改版本库，保留暂存区，保留工作区）

### git删除文件

* 从版本库中删除
  
  ```
  git rm filename
  ```

* 对文件取消跟踪

```
git rm --cached readme1.txt    删除readme1.txt的跟踪，并保留在本地。
git rm -r                              删除文件夹跟踪
git rm --f readme1.txt    删除readme1.txt的跟踪，并且删除本地文件。
```

### git撤销文件修改

```
git checkout -- file可以丢弃工作区的修改
git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令
```

### git rebase用法

## git cherry-pick

获取指定提交到分支

```shell
git cherry-pick <commitHash>
```

`git cherry-pick`命令的常用配置项如下。

**（1）`-e`，`--edit`**

打开外部编辑器，编辑提交信息。

**（2）`-n`，`--no-commit`**

只更新工作区和暂存区，不产生新的提交。

**（3）`-x`**

在提交信息的末尾追加一行`(cherry picked from commit ...)`，方便以后查到这个提交是如何产生的。

**（4）`-s`，`--signoff`**

在提交信息的末尾追加一行操作者的签名，表示是谁进行了这个操作。

**（5）`-m parent-number`，`--mainline parent-number`**

如果原始提交是一个合并节点，来自于两个分支的合并，那么 Cherry pick 默认将失败，因为它不知道应该采用哪个分支的代码变动。

`-m`配置项告诉 Git，应该采用哪个分支的变动。它的参数`parent-number`是一个从`1`开始的整数，代表原始提交的父分支编号。

> ```bash
> $ git cherry-pick -m 1 <commitHash>
> ```

上面命令表示，Cherry pick 采用提交`commitHash`来自编号1的父分支的变动。

一般来说，1号父分支是接受变动的分支（the branch being merged into），2号父分支是作为变动来源的分支（the branch being merged from）。

## 分支管理

### 查看分支

- 查看本地所有分支
  
  ```
  git branch
  ```

- 查看所有分支包括远程

```
git branch -a
```

### 创建并切换分支

```git
 git checkout -b dev
切换到一个新分支 'dev'
等价于
$ git branch dev
$ git checkout dev
```

### 替换git checkout

It’s a command that Git has added in `Git 2.23`

```git
if you want to switch branches than we can use

git switch develop
# same as 'git checkout develop'
git switch -c new-branch
# same as 'git checkout -b new-branch'
and also you can use if you restore a file

git restore README.md
# same as 'git checkout -- README.md'
git restore --staged README.md
# same as 'git reset HEAD README.md'
```

### 拉取远程指定分支到本地

```
git checkout -b localbranchname origin/upstreanbranchname
```

### 拉取其它分支提取文件

```
git checkout [branch] -- [file name]
```

### 删除分支

* 删除本地分支

```
git branch  -d  branchname
```

* 删除远程分支

```
git push origin --delete branchname
```

* 查看远程分支和本地分支的对应关系
  
  ```
  git remote show origin  
  ```

* 删除远程已经删除过的分支
  
  ```
  git remote prune origin
  ```

### 远程分支强制覆盖本地分支

```
git fetch --all    //只是下载代码到本地，不进行合并操作

git reset --hard origin/分支名如master    //把HEAD指向最新下载的版本
```

### 分支重命名

* 分支还没有推送到远程
  
  ```
  git branch -m oldName newName
  ```

## 暂存

在开发过程中需要创建一个临时分支修复bug， 修复后，合并分支，然后将临时分支删除

```
hd@hd:~/Jitu/CloudStaring/cloudstaringSrc/linux_camera $ git status
位于分支 TimeInterval
您的分支与上游分支 'origin/TimeInterval' 一致。
尚未暂存以备提交的变更：
  （使用 "git add/rm <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

    修改：     ../gestureReco/gestureReco.pro
    修改：     AlgrithmApllcation/AlgrithmApplication.cpp
    修改：     AlgrithmApllcation/BackGroudThread.cpp
    删除：     AlgrithmApllcation/DealImageQenue.cpp
    修改：     AlgrithmApllcation/EventJudge.cpp
    修改：     AlgrithmApllcation/ModelDetectThd.cpp
```

 git 提供一个stash 命令把当前工作现场“储藏”起来，等以后恢复现场后继续工作

```
hd@hd:~/Jitu/CloudStaring/cloudstaringSrc/linux_camera $ git stash
Saved working directory and index state WIP on TimeInterval: 95c5214 Merge branch 'TimeInterval' of http://42.193.36.70:23005/Hello/cloudstaring into TimeInterval
HEAD 现在位于 95c5214 Merge branch 'TimeInterval' of http://42.193.36.70:23005/Hello/cloudstaring into TimeInterval
```

现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。
工作区是干净的，刚才的工作现场存到哪去了？

* git stash list命令看看：
  显示保存的工作进度列表，编号越小代表保存进度的时间越近

* git stash apply stash@{num}
  使用了 git stash clear  所有代码都被清空了，禁用

* 查看暂存区修改信息 
  
  ```
  git diff --cached
  ```

* 撤销暂存区修改

git reset <file>

## 忽略特殊文件

有些时候想要忽略git status 显示出的未跟踪文件，这个问题需要在git的根目录创建一个特殊的.gitignore文件，然后把忽略的文件名填进去，Git就会自动忽略这些文件。
GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略前:

```
未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

    .vscode/
    gestureReco/.qmake.stash
    linux_camera/.~lock.README.docx#
    linux_camera/VideoManager/AbsDevice/CMakeCache.txt
    linux_camera/VideoManager/AbsDevice/CMakeFiles/
    linux_camera/VideoManager/AbsDevice/cmake_install.cmake
    linux_camera/VideoManager/CMakeFiles/VideoManager.dir/relink.txt
    linux_camera/alg/VeModelDetector/makefile
    linux_camera/alg/caffe-master/build/
    linux_camera/json/makefile
```

忽略后:

```
未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

    alg/VeModelDetector/makefile
    json/makefile
```

- 忽略文件权限

  ```shell
  我们可以使用如下方法让git忽略文件权限，执行以下命令
  
  git config core.filemode false
  ```

  

#### 已经被跟踪文件如何忽略

不仅将文件添加到 Git 仓库，还推送到远程仓库了。并且其他合作的小伙伴已经从远程仓库拉取了更新。这个时候才发现某些文件需要忽略。

这种情况下，如果我们再将想忽略的文件添加到 `.gitignore` 文件中，已经完全不起任何作用了。

那怎么办呢？我们可以使用另外一个命令:

```
git update-index --assume-unchanged <file>
```

## Git fork 后保持与原仓库同步

### merge 前的设定

- step 1 : 进入本地仓库
- step 2 : 执行命令 git remote -v 查看你的远程仓库路径

```git
git remote -v
origin    git@42.193.36.70:Hello/cloudstaring.git (fetch)
origin    git@42.193.36.70:Hello/cloudstaring.git (push)
```

 如果上面只有俩行，说明未设置upstream

- step 3 : 执行命令

```git
git remote add upstream http://42.193.36.70:23005/jitutech/cloudstaring.git

git remote -v
origin    git@42.193.36.70:Hello/cloudstaring.git (fetch)
origin    git@42.193.36.70:Hello/cloudstaring.git (push)
upstream    http://42.193.36.70:23005/jitutech/cloudstaring.git (fetch)
upstream    http://42.193.36.70:23005/jitutech/cloudstaring.git (push)
```

- step 4 : 执行命令 git status 检查本地是否有未提交的更改

```git
git add -A 或者 git add filename
git commit -m "your note"
git push origin master
git status
```

### merge 的关键命令

```shell
 - step 5 : 执行命令 git fetch upstream 抓取原仓库的更新
 - step 6 : 执行命令 git checkout master 切换到master分支
 - step 7 : 执行命令 git merge upstream/master 合并远程的master分支
 - step 8 : 执行命令 git push 把本地仓库向github仓库（你fork到自己名下的仓库）推送修改
```

[ref]: https://github.com/selfteaching/the-craft-of-selfteaching/issues/67

## SubModule

### 什么是SubModule

某个工作中的项目需要包含并使用另一个项目。 也许是第三方库，或者你独立开发的，用于多个父项目的库。 现在问题来了：你想要把它们当做两个独立的项目，同时又想在一个项目中使用另一个。
Git 通过子模块来解决这个问题。 子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。 它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。

## git log

查看分支线

```shell
git log --all --decorate --oneline --graph
git log --graph --decorate --oneline
```

## 关于commit

### 查看commit指定文件的修改信息

```shell
git show commitId
git show commitId fileName
```

### 撤销操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令来重新提交：

```console
$ git commit --amend
```

这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令）， 那么快照会保持不变，而你所修改的只是提交信息。

文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。

例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

```console
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

最终你只会有一个提交——第二次提交将代替第一次提交的结果。

### 使用git rebase -i 来修改某一次的提交信息

```svg
git rebase -i master^^ // 假设我们当前在master分支
```

> 上面两个^ 是什么鬼？上述操作在分支名后总共可以放两个字符，一个是 ^ 一个是~，具体规则如下

- ^ 的用法：在 commit 的后面加一个或多个 ^ 号，可以把 commit 往回偏移，偏移的数量是 ^ 的数量。例如：master^^表示 当前master 指向的 commit 之前倒数第2个 commit
- ~ 的用法：在 commit 的后面加上 ~ 号和一个数，可以把 commit 往回偏移，偏移的数量是 ~ 号后面的数。例如：master~2 表示的和master^^是一样操作。

    eg: 想要修改的信息是倒数第二条

1. step1


```git
git rebase -i branch master~2
```

 接下来可以操作的命令都在上图中显示了，我们要做的是编辑，并且要编辑的是第一行（它的排列顺序是一个正序排序，也就是说旧的commit信息在上面，新的commit在下面），我们将pick改为edit,推出保存

2. step2 做想要修改的操作

```git
git add file
git commit --amend -m "msg"
```

3. step3 改完信息后，我们还需要git rebase --continue (eg:修改提交的作者信息)

```shell
git commit --amend --author="Author Name <email@address.com>" 来修改commit
```

### commit 提交修改文件某一行

You can use:

```bash
git add --patch <filename>
```

or for short:

```bash
git add -p <filename>
```

Git will break down your file into what it thinks are sensible "hunks" (portions of the file). It will then prompt you with this question:

```
Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]?
```

Here is a description of each option:

- y stage this hunk for the next commit

- n do not stage this hunk for the next commit

- q quit; do not stage this hunk or any of the remaining hunks

- a stage this hunk and all later hunks in the file

- d do not stage this hunk or any of the later hunks in the file

- g select a hunk to go to

- / search for a hunk matching the given regex

- j leave this hunk undecided, see next undecided hunk

- J leave this hunk undecided, see next hunk

- k leave this hunk undecided, see previous undecided hunk

- K leave this hunk undecided, see previous hunk

- s split the current hunk into smaller hunks

- e

   

  manually edit the current hunk

  - You can then edit the hunk manually by replacing `+`/`-` by `#` (thanks [veksen](https://stackoverflow.com/users/1732521/veksen))

- ? print hunk help

If the file is not in the repository yet, you can first do `git add -N <filename>`. Afterwards you can go on with `git add -p <filename>`.

Afterwards, you can use:

- `git diff --staged` to check that you staged the correct changes
- `git reset -p` to unstage mistakenly added hunks
- `git commit -v` to view your commit while you edit the commit message.

Note this is far different than the `git format-patch` command, whose purpose is to parse commit data into a `.patch` files.



## git 显示差异

### git 显示暂存区更改

 It should just be:

```
git diff --cached
```

`--cached` means show the changes in the cache/index (i.e. staged changes) against the current `HEAD`. `--staged` is a synonym for `--cached`.

`--staged` and `--cached` does not point to `HEAD`, just difference with respect to `HEAD`. If you cherry pick what to commit using `git add --patch` (or `git add -p`), `--staged` will return what is staged.

`git diff --name-only --cached`    指定特定文件

## git add显示进度

脚本

```shell
git add --verbose . > ../progress.txt & percent=0; while [[ $percent -le 99 && $percent -ge 0 ]]; do num1=$(cat ../progress.txt | wc -l); num2=$(find . -type f -not -path "./.git/*" | wc -l); percent=$((num1*100 / (num2 - 3) )); echo $percent"%"; sleep 1; done; echo "DONE"; sleep 1; rm ../progress.txt
```



## git tag

### Switched to a new branch 'v3.0-branch'

```git
git checkout tags/v3.0 -b v3.0-branch
```

## 



Tags:
  git
