# Git使用


# git使用[^1]

{{< music auto="https://music.163.com/#/song?id=1418131335" >}}

## 基本命令

* **创建仓库**`git init`

* **把文件添加到仓库**`git add`
* **全部添加** `git add .`

* **把文件提交到仓库`git commit`**
>
> 可以多次`git add`后`git commit`
> 
> 命令之后必须加参数 `-m "注释"`来提交

* **查看目前仓库情况**`git status`

* **`git diff`将本地文件与*暂存区*的文件进行比较**

* `git log`

> * **查看文件历次commit提交的情况**
>
> * ![](/gitlog.PNG)
>
>   commit后面的**数字**是**提交的编号**
>
>   `HEAD`表示**当前版本**,`HEAD^`表示**上个版本**，`HEAD^^`表示**上上个版本**
>
>   `HEAD~100`表示**往上的100个版本**


* `git reset`

> 用法：`git reset --hard HEAD^`表示**回退**到**前一个版本**
>
> ![](/gitreset.PNG)
>
> 发现最早的版本记录没了
>
> 此时可以通过`git reset --hard 之前的版本号`来恢复操作

* `git reflog`

> * 可以查看**所有分支**的**所有操作**记录（包括已经被删除的 commit 记录和 reset 的操作）
>
> > 例如执行 `git reset --hard HEAD~1`，退回到上一个版本，用`git log`则是看不出来被删除的`commitid`，用`git reflog`则可以看到被删除的`commitid`，恢复到被删除的那个版本。


### 小结

* 创建git仓库使用`git init`命令
* 提交文件，对多个文件`git add`之后`git commit -m ""`
* 查看仓库情况，使用`git status`可以查看**文件被修改**，**add了修改文件的情况**，**仓库没有发生变化**三种状态
* 如果`git status`显示**文件被修改，但还没被`add`**，可以用`git diff`查看修改情况
* 查看版本提交历史情况使用`git log`
* `HEAD`表示当前版本,`HEAD^`表示上个版本，`HEAD^^`表示上上个版本`HEAD~100`表示往上的100个版本
* 版本回退使用`git reset --hard commit_id/HEAD^^/HEAD~100`
* 重返未来，使用`git reflog`查看提交历史命令

## 暂存区和`master`分支

* git**暂存区**和**master分支**的概念
* git是**管理修改**的，`git add`之后将文件添加到**暂存区**,如果没有将修改提交到暂存区，对文件的更改也**不会上传到master分支上**
* 如果对文件修改错误，但还没add`git checkout --readme.txt`让文件恢复到最后一次`git add`或`git commit`的状态
* 如果错误的文件已经`git add`，使用`git reset HEAD readme.txt`可将**暂存区的文件**恢复至**版本库文件的版本**
> 1. 当改乱了某文件内容，想直接丢弃工作区修改时，使用`git checkout -- file`
> 
> 2. 如果已经add了，文件在暂存区中，使用`git reset HEAD <file>` 回到第一步，然后再按第一步操作
> 
> 3. 如果已经commit了，使用`git log` 然后使用`git reset`命令

## 删除文件

* 如果想要删除版本库里的文件，先**删除本地文件**,然后`git rm`删除文件，然后`git commit`
* `git rm <file>`将文件**从暂存区和工作区中删除**
* `git rm --cached <file>`**从暂存区移除**，但仍然保留在当前工作目录中，仅是**从跟踪清单中删除**

## 链接远程库

* `git remote add origin ***`**关联远程库，`***`代表远程库的地址，可以为git或者https,git地址速度块**
* 首次`s`
* `git remote -v`**查看远程仓库**
* `git push --force origin master`或者`git push -f origin master`**强制覆盖远程分支**
* `git checkout -b a origin/a`从远程a分支拉取分支信息到本地a分支

## 分支创建和基本使用

* **创建分支**`git branch dev`
* **切换到分支**`git checkout dev`
* **查看分支情况**`git branch`

* **将别的分支合并到当前分支**`git merge 分支名`
  * 如果存在冲突，需要解决冲突
  * 修改完冲突文件后使用`git add .`命令
  * 再使用`git commit`命令，可以不带`-m `参数添加注释
* **删除分支**`git branch -d 分支名`
* 在操作中添加`-r`参数，代表对**远程仓库**进行分支操作
对一个分支的本地文件操作不会影响到另一分支的本地文件情况

## Git进阶

### 分支管理

* `git rebase branch`**变基操作**

  * **将branch分支中的commit放到当前的commit之前**，合并为同一分支
* `git commit --amend`可以**编辑当前的commit信息**
* `git rebase -i HEAD~3`

  * **编辑前三个commit**
  * 通过**squash**可用于多个commit信息的**合并**
  * 通过**edit**可用于历史commit信息的**编辑**
  > 编辑完文件后仍需要**多次运行`git commit --amend`**来编辑多次历史提交信息
  >
  > 每次运行上述命令之后，**需要`git rebase --continue`代表保存并进行下一条**
  >
  > **`--abort`参数可放弃当前操作**，例如`git rebase --abort`

* `git push --set-upstream origin newbranch`**将本地分支newbranch与远程分支newbranch关联**

* `origin/main`和是远程分支在本地分支更新的**克隆**
  * `git pull`操作是`git fetch`和`git merge origin/***`两个命令的集合
  * `git fetch`命令使用远程分支**更新本地`origin/***`分支**
  * `git merge origin/***`将`origin/***`分支**合并到当前`***`分支**

[^1]: https://www.liaoxuefeng.com/wiki/896043488029600

