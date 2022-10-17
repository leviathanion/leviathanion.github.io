# git使用指南


# git使用指南

## 基本概念
* git将本地文件的管理分为三个区域**工作区**，**暂存区**，**仓库区**，同时可以通过**远程代码仓库**等服务器来管理git如下图所示

![git分区](/git使用指南/gitoverview.png "git总览")

### 工作区
* 对于文件的添加删除和修改，都发生在工作区
### 暂存区
* 暂存区指将工作区中的操作完成小阶段的存储，是版本库的一部分
* 一经提交就会消失
### 仓库区
* 代表了小阶段的完成，由各个版本
* 各版本可以查看和回退

## 仓库整体管理
* `git init`创建仓库
* `git status`查看仓库**当前工作区和暂存区情况**，包括文件修改，文件add，仓库无变化三个状态
* `git blame file`以行的形式显示某个文件的修改情况
* `git log`查看**仓库区版本**即历次commit的情况
> 查看不了reset和删除的信息
>
> commit后面的**数字**是**提交的编号commitid**
>
> `HEAD`表示**当前版本**,`HEAD^`表示**上个版本**，`HEAD^^`表示**上上个版本**
>
> `HEAD~100`表示**往上的100个版本**
* `git reflog`可以查看**所有分支**的**所有操作**记录（包括已经被删除的 commit 记录和 reset 的操作）
> git log查看不了reset或rebase合并多次提交之前的版本，可以通过git reflog来复原
* `git reset --hard HEAD^/~n/commitid`回退到对应版本
> `git reset`有三种工作模式
>
> * `--soft`移动`HEAD`指针，不改变工作区和暂存区的内容
>
> * `--mixed`默认参数，移动`HEAD`指针，改变暂存区内容，不改变工作区内容
>
> * `--hard`工作区，暂存区和`HEAD`指针均改变

## 工作区管理
* `git checkout -- file`根据**暂存区和仓库区**恢复**工作区**
> 若暂存区有文件，则将工作区还原成和暂存区一样
> 若暂存区无文件，则将工作区还原成和仓库区一样
* `git rm file`从**工作区中删除**某文件
> 若已经`add`，需要加`-f`参数，同步删除暂存区

## 暂存区管理
* `git add file/.`将某个文件或所有文件添加到暂存区中
* `git reset HEAD file`将**暂存区文件**恢复至**仓库区文件**的版本，但**不改变工作区**
* `git rm --cached file`将某个文件从暂存区中删除，即**从跟踪清单中删除**，不改变工作区


## 仓库区管理
* `git commit -m ""`从暂存区提交到仓库区版本库

## 分支管理
### 基本管理
* **创建分支**`git branch dev`
* **切换到分支**`git checkout dev`
* **查看分支情况**`git branch`
* **创建并切换到分支** `git checkout -b dev`
* **将别的分支合并到当前分支**`git merge 分支名`
  * 如果存在冲突，需要解决冲突
  * 修改完冲突文件后使用`git add .`命令
  * 再使用`git commit`命令，可以不带`-m `参数添加注释
* **删除分支**`git branch -d 分支名`
* 在操作中添加`-r`参数，代表对**远程仓库**进行分支操作
> 对一个分支的本地文件操作不会影响到另一分支的本地文件情况
### 高级操作
* `git rebase branch`**变基操作**
> **将branch分支中的commit放到当前的commit之前**，合并为同一分支
>
> 如果有冲突，编辑完文件后仍需要多次运行`git commit --amend`来编辑多次历史提交信息
>
> 每次运行上述命令之后，**需要`git rebase --continue`代表保存并进行下一条**
>
> **`--abort`参数可放弃当前操作**，例如`git rebase --abort`
* `git commit --amend`可以**编辑当前的commit信息**
* `git rebase -i HEAD~3`
  * **编辑前三个commit**
  * 通过**squash**可用于多个commit信息的**合并**
  * 通过**edit**可用于历史commit信息的**编辑**

## 远程库管理
* `git remote add origin remote`**关联远程库，`remote`代表远程库的地址，可以为git或者https,git地址速度更快**
* `git remote -v`**查看远程仓库**
* `git push --force origin master`或者`git push -f origin master`**强制覆盖远程分支**
* `git checkout -b a origin/a`从远程a分支拉取分支信息到本地a分支
* `git push --set-upstream origin newbranch`**将本地分支newbranch与远程分支newbranch关联**
* `origin/main`是远程分支在本地分支的**克隆**
  * `git pull`操作是`git fetch`和`git merge origin/'branch'`两个命令的集合
  * `git fetch`命令使用远程分支**更新本地`origin/'branch'`分支**
  * `git merge origin/'branch'`将`origin/'branch'`分支**合并到当前`'branch'`分支**

## 差异比较
* `git diff file`比较**工作区和暂存区**的差异
* `git diff --cached file`比较**暂存区和仓库区**的区别
* `git diff HEAD file`比较**工作区和仓库区**的区别

## 编码问题
* 在默认设置下，中文文件名在工作区状态输出，查看历史更改概要，以及在补丁文件中，文件名的中文不能正确地显示，而是显示为八进制的字符编码，示例如下：
`"assets/\346\234\272\345\231\250\345\255\246\344\271\240\347\232\204\345\271\266\350\241\214\347\255\226\347\225\245/"
"content/posts/\346\234\272\345\231\250\345\255\246\344\271\240/"`
* 通过将Git配置变量 core.quotepath 设置为false，就可以解决中文文件名称在这些Git命令输出中的显示问题
`git config --global core.quotepath false`
* 将此改到github action文件中，同样可以解决hugo模板lastmod显示不正常的问题


