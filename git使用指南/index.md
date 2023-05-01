# Git使用指南


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
### 远程仓库基础知识
* 常见的origin其实是对某个远程仓库的别名
* 为了方便push时指定对应的远程仓库，所以使用别名，也可以改为别的别名
* 别名`git fetch`时会在本地创建`<远程仓库别名>/<分支名>`的分支，和本地分支不同
* 多个远程分支管理参考`.git/config`文件

## 仓库整体管理
### 创建仓库
* `git init`
### 查看工作区和暂存区情况
* `git status`查看仓库**当前工作区和暂存区情况**，包括文件修改，文件add，仓库无变化三个状态
### 显示文件修改情况
* `git blame file`以行的形式显示某个文件的修改情况
### 查看commit情况
* `git log`查看**仓库区版本**即历次commit的情况
    * `--graph`会以图的形式展示
    * `--oneline`单行显示提交信息
    * `--all`显示所有分支，可以替换为`branchA branchB`只显示这两个分支的关系
> 查看不了reset和删除的信息
>
> commit后面的**数字**是**提交的编号commitid**
>
> `HEAD`表示**当前版本**,`HEAD^`表示**上个版本**，`HEAD^^`表示**上上个版本**
>
> `HEAD~100`表示**往上的100个版本**
* `git reflog`可以查看**所有分支**的**所有操作**记录（包括已经被删除的 commit 记录和 reset 的操作）
> git log查看不了reset或rebase合并多次提交之前的版本，可以通过git reflog来复原

### 版本回退
* `git reset --hard HEAD^|~n|commitid`回退到对应版本
> `git reset`有三种工作模式
>
> * `--soft`移动`HEAD`指针，不改变工作区和暂存区的内容
>
> * `--mixed`默认参数，移动`HEAD`指针，改变暂存区内容，不改变工作区内容
>
> * `--hard`工作区，暂存区和`HEAD`指针均改变

## 工作区管理
### 恢复工作区
* `git checkout -- file`根据**暂存区和仓库区**恢复**工作区**
> 若暂存区有文件，则将工作区还原成和暂存区一样
> 若暂存区无文件，则将工作区还原成和仓库区一样
### 删除工作区文件
* `git rm file`从**工作区中删除**某文件
> 若已经`add`，需要加`-f`参数，同步删除暂存区

## 暂存区管理
### 添加到暂存区
* `git add file|.`将某个文件或所有文件添加到暂存区中
### 恢复暂存区(参考版本回退)
* `git reset HEAD file`将**暂存区文件**恢复至**仓库区文件**的版本，但**不改变工作区**
### 删除暂存区文件
* `git rm --cached file`将某个文件从暂存区中删除，即**从跟踪清单中删除**，不改变工作区


## 仓库区管理
### 添加到仓库区
* `git commit -m ""`从暂存区提交到仓库区版本库
### 编辑commit信息
* `git commit --amend`可以**编辑当前的commit信息**
* `git rebase -i HEAD~num`，将commit状态修改为edit，可以**编辑多个commit信息**

## 栈管理
* `git stash`或`git stash save '注释'`保存工作区与暂存区的状态，把当前修改保存到git栈中
    * 栈结构，遵循先进后出
    * 每次使用都会新加一个`stash@{num}`
* `git stash list`查看当前git栈中的所有内容
* `git stash show stash@{num}`显示stash与当前目录的差异，不指定默认显示栈顶
* `git stash pop`回复git栈中最上面的stash，并删除该stash
* `git stash apply stash@{num}`将栈中的内容恢复到当前分支，不删除stash
* `git stash drop stash@{num}`移除指定stash
* `git stash clear`移除所有stash

## 远程库管理
> `<远程仓库别名>/<分支名>`是远程分支在本地分支的**克隆**
### 添加远程库
* `git remote add <远程仓库别名> <远程库地址>`**关联远程库，远程库的地址，可以为git或者https,git地址速度更快**
### 查看远程库
* `git remote -v`**查看远程仓库**
### 关联远程库
* `git push --set-upstream <远程仓库别名> <分支名>`**将本地分支与对应远程仓库的对应分支关联**
> 首次提交至远程库按照以下步骤
> * `git remote add <远程仓库别名> <远程地址>`
> * `git push -u <远程仓库别名> <本地分支>`
### 重命名远程库
* `git remove rename oldname newname` **重命名一个远程仓库**
### 提交至远程库
* `git push <远程仓库别名> <本地分支名>:<远程分支名>`或者`git push <远程仓库地址> <本地分支名>:<远程分支名>`
    * **将本地分支推送至远程分支中**
    * 省略远程仓库别名，查看当前分支对应的远程仓库和远程分支配置，如果没有，则默认origin
    * 省略远程分支，删除冒号，使用配置中的东西来更新，否则查找相同名称的分支
    * 省略本地分支，不删除冒号，等同于推送一个空的本地分支到远程分支，用来删除远程分支,等同于`git push origin --delete master`
    * 本地分支，远程分支均省略，则查看当前分支对应的远程仓库和远程分支配置
> `-u`参数可以在`.git/config`文件中指定当前分支对应的远程仓库和远程分支，之后就可以省略远程分支名
> `-f|--force`强制执行
### 从远程库拉取
* `git pull <远程仓库别名> <远程分支名>:<本地分支名>`所有参数均可省略，如果省略分支名，则查找当前分支对应的远程分支自动拉取
  * `git pull`操作是`git fetch`和`git merge <远程仓库别名>/<分支名>`两个命令的集合
  * `git fetch`命令使用远程分支**更新本地`<远程仓库别名>/<分支名>`分支**
  * `git merge <远程仓库别名>/<分支名>`将`<远程仓库别名>/<分支名>`分支**合并到当前本地分支**
> `git fetch`配合`git rebase`可以进行另一种形式的`git pull`
* `git checkout -b a <远程仓库别名>/<分支名>`从对应的远程仓库的对应分支拉取分支信息到本地分支

## 分支管理
> 新版git提供了`git switch`命令来代替`git checkout`
### 本地
> 对一个分支的本地文件操作不会影响到另一分支的本地文件情况
* **创建分支**`git branch dev`
* **切换到分支**`git checkout dev`或者`git switch dev`
* **查看分支情况**`git branch`
* **创建并切换到分支** `git checkout -b dev`或`git switch -c dev`
* **将别的分支合并到当前分支**`git merge 分支名`或者`git rebase 分支名`
  * 如果存在冲突，需要解决冲突
  * 修改完冲突文件后使用`git add .`命令
  * 再使用`git commit`命令，可以不带`-m `参数添加注释
* **删除分支**`git branch -d 分支名`
* **重命名分支**`git branch -m oldname newname`

### 远程
* 在操作中添加`-r`参数，代表对**本地的远程仓库**进行分支操作
* 删除远程分支
    1. 删除本地分支`git branch -d 分支名`
    2. 删除远程分支`git push <远程分支别名> -d <分支名>`
    2. 删除远程分支`git push <远程分支别名>  :<分支名>`
    3. 删除远程分支在本地的副本`git branch -r -d <远程分支别名>/<分支名>`
* 重命名远程分支
    1. 重命名本地分支`git branch -m oldname newname`
    2. 关联本地新名分支和远程新名分支`git push --set-upstream <远程仓库别名> <新分支名>`
    3. 删除远程原分支`git push <远程分支别名> -d <分支名>`
    3. 删除远程原分支`git push <远程分支别名>  :<分支名>`

## 变基操作
* `git rebase branch`**变基操作**
> **将branch分支中的commit放到当前的commit之前**，合并为同一分支
>
> 如果有冲突，编辑完文件后仍需要多次运行`git commit --amend`来编辑多次历史提交信息
>
> 每次运行上述命令之后，**需要`git rebase --continue`代表保存并进行下一条**
>
> **`--abort`参数可放弃当前操作**，例如`git rebase --abort`
* `git rebase -i HEAD~3`
  * **编辑前三个commit**
  * 通过**squash**可用于多个commit信息的**合并**
  * 通过**edit**可用于历史commit信息的**编辑**


## 标签管理
### 查看标签
* 查看标签：`git tag`
* 查看标签详细信息：`git show <tag name>`
### 打标签
* 为最新的commit打上标签：`git tag <tag name>`
* 为对应的commit打上标签：`git tag <tag name> <commitid>`
* 创建带标签的tag：`git tag -a <tag name> -m "<注释文字>" <commitid>`
### 删除标签
* 删除标签：`git tag -d <tag name>`
### 远程操作
* 推送Tag到远程仓库：`git push <远程仓库别名> <tag name>`
* 推送所有本地tag：`git push (<远程仓库别名>) --tags`(<远程仓库别名>可省略)
* 删除远程标签：
    1. 删除本地标签：`git tag -d <tag name>`
    2. 删除远程分支：`git push <远程仓库别名> :refs|tags|<tag name>`
    2. 删除远程分支`git push <远程分支别名> -d <tag name>`

## 差异比较
* `git diff file`比较**工作区和暂存区**的差异
* `git diff --cached file`比较**暂存区和仓库区**的区别
* `git diff HEAD file`比较**工作区和仓库区**的区别
* `git diff <branch_a> <branch_b>`比较**两个分支**的区别
> 此命令并不能查看未追踪的文件，因此新建的文件在未add之前，不能显示出新文件的差异。
>
> 可通过`git status`查看哪些文件是未追踪的

## 补丁操作
### 生成补丁
* `git diff`命令后接`> ***.patch`
* `git format-patch`
    * `git format-patch HEAD^` 生成最近的1次commit的patch
    * `git format-patch HEAD^^` 生成最近的2次commit的patch
    * `git format-patch HEAD^^^` 生成最近的3次commit的patch
    * `git format-patch HEAD^^^^` 生成最近的4次commit的patch
    * `git format-patch <r1>..<r2>` 生成两个commit间的修改的patch（生成的patch不包含r1. <r1>和<r2>都是具体的commit号)
    * `git format-patch -1 <r1>` 生成单个commit的patch
    * `git format-patch <r1>` 生成某commit以来的修改patch（不包含该commit）
    * `git format-patch --root <r1>` 生成从根到r1提交的所有patch
### 打补丁
* `git apply`此种方式不会应用`commit message`，需要自己重新`git add`和`git commit`
    * 强制打补丁`git apply --reject xxx.patch`，如果有冲突，会生成.ref文件
* `git am`会直接将patch的所有信息打上去，而且不用重新git add和git commit，author也是patch的author而不是打patch的人。


## 合并仓库
1. 创建新仓库
```shell
mkdir newProject
cd newProject
git init
```
2. 将要合并的仓库作为新仓库的远程仓库
```shell
git remote add project1 <project1_repo_url>
git remote add project2 <project2_repo_url>
git remote add project3 <project3_repo_url>
```
3. 拉取到`<远程仓库别名>/<分支名>`的本地分支下
```shell
git fetch project1
git fetch project2
git fetch project3
```
4. 创建三个本地分支，并合并远程仓库的本地备份分支
```shell
git switch -c project1
git merge|rebase project1/<分支名> 
git switch -c project2
git merge|rebase project2/<分支名> 
git switch -c project3
git merge|rebase project3/<分支名> 
```
5. 将三个分支一起合并到主分支上
```shell
git switch master
git merge/rebase project1
git merge/rebase project2
git merge/rebase project3

```

## 编码问题
* 在默认设置下，中文文件名在工作区状态输出，查看历史更改概要，以及在补丁文件中，文件名的中文不能正确地显示，而是显示为八进制的字符编码，示例如下：
`"assets/\346\234\272\345\231\250\345\255\246\344\271\240\347\232\204\345\271\266\350\241\214\347\255\226\347\225\245/"
"content/posts/\346\234\272\345\231\250\345\255\246\344\271\240/"`
* 通过将Git配置变量 core.quotepath 设置为false，就可以解决中文文件名称在这些Git命令输出中的显示问题
`git config --global core.quotepath false`
* 将此改到github action文件中，同样可以解决hugo模板lastmod显示不正常的问题

## git提交规范
* commit message格式
```
<type>(<scope>): <subject>

<body>

<footer>
```
* Header(必须)
    * 包含三组字符`type`（必选），`scope`(可选)，`subject`(必选)
    * `type`用于说明commit的类别，主要包含七类
    > * feat：新功能（feature）
    > * fix：修补bug
    > * docs：文档（documentation）
    > * style： 格式（不影响代码运行的变动）
    > * refactor：重构（即不是新增功能，也不是修改bug的代码变动）
    > * test：增加测试
    > * chore：构建过程或辅助工具的变动
    > 如果type为feat和fix，则该 commit 将肯定出现在 Change log 之中。其他视情况而定
    * `scope` 说明commit的影响范围，比如数据层，控制层，文件或者目录等
    * `subject`是commit目的的简短描述
    > * 以动词为开头，使用第一人称现在时
    > * 第一个字母小写
    > * 结尾不加句号
* Body(可选)
    * 是对本次commit的详细描述
    * 使用第一人称现在时
    * 说明代码变动的动机，以及与以前行为的对比
* Footer(可选)
    * 不兼容变动下使用，以`BREAKING CHANGE`开头，后面是对变动的描述，包括理由和迁移方法等
    * 关闭`Issue`，`Close #123`
    


