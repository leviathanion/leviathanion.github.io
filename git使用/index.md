# Git使用


# git使用[^1]

{{< music auto="https://music.163.com/#/song?id=1418131335" >}}

## 基本命令

* **创建仓库**`git init`

* **把文件添加到仓库**`git add`
* **全部添加** `git add .`

* `git commit`

> **把文件提交到仓库**
>
> > **可以多次`git add`后git commit**
> >
> > **之后必须加参数 `-m "注释"`来提交**
>
> **类似于游戏的存档**

* **查看目前仓库情况**`git status`

* `git diff`**在修改文件但是没有add之前查看文件修改的结果，即将本地文件与*暂存区*的文件进行比较**

* `git log`

> * **查看文件历次commit提交的情况**
>
> * ![](/gitlog.PNG)
>
>   **commit后面的数字是提交的编号**
>
>   **`HEAD`表示当前版本,`HEAD^`表示上个版本，`HEAD^^`表示上上个版本**
>
>   **`HEAD~100`表示往上的100个版本**

* `git reset`

> **用法：`git reset --hard HEAD^`表示回退到前一个版本**
>
> ![](/gitreset.PNG)
>
> **发现最早的版本记录没了**
>
> **此时可以通过`git reset --hard 之前的版本号`来恢复操作**

* `git reflog`

> **查看git的所有命令**
>
> **配合`git reset --hard 版本号`可以撤回回退的操作**

### 小结

* 创建git仓库使用`git init`命令
* 提交文件，对多个文件`git add`之后`git commit -m ""`
* 查看仓库情况，使用`git status`可以查看**文件被修改**，**add了修改文件的情况**，**仓库没有发生变化**三种状态
* 如果`git status`显示**文件被修改，但还没被`add`**，可以用`git diff`查看修改情况
* 查看版本提交历史情况使用`git log`
* `HEAD`表示当前版本,`HEAD^`表示上个版本，`HEAD^^`表示上上个版本`HEAD~100`表示往上的100个版本
* 版本穿梭使用`git reset --hard commit_id/HEAD^^/HEAD~100`
* 重返未来，使用`git reflog`查看提交历史命令

## 暂存区和`master`分支

* git**暂存区**和**master分支**的概念
* git是**管理修改**的，`git add`之后将文件添加到**暂存区**,如果没有将修改提交到暂存区，对文件的更改也**不会上传到master分支上**
* 如果对文件修改错误，但还没add`git checkout --readme.txt`让文件恢复到最后一次`git add`或`git commit`的状态
* 如果错误的文件已经`git add`，使用`git reset HEAD readme.txt`可将**暂存区的文件**恢复至**版本库文件的版本**

### 小结

![](/小结.PNG)

## 删除文件

* 如果想要删除版本库里的文件，先**`删除本地文件`**,然后`git rm`删除文件，然后`git commit`

## 链接远程库

![](/连接远程库1.PNG)

![](/连接远程库2.PNG)

* `git remote -v`查看远程仓库

## 分支创建和基本使用

* **创建分支**`git branch dev`
* **切换到分支**`git checkout dev`
* **查看分支情况**`git branch`

* **将别的分支合并到当前分支**`git merge 分支名`
* **删除分支**`git branch -d 分支名`

对一个分支的本地文件操作不会影响到另一分支的本地文件情况

[^1]: https://www.liaoxuefeng.com/wiki/896043488029600

