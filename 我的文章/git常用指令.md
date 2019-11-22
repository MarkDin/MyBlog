# git常用指令



![git](http://www.ruanyifeng.com/blogimg/asset/2014/bg2014061202.jpg)

### `git config --global credential.helper store`

用于操作时不断提示输入用户名和密码

## `git log`

**查看操作日志**

#### `--pretty=oneline`

**将 git log的信息显示在一行**



## `git reflog`

**要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。**



## `git init`

**把这个当前目录变成Git可以管理的仓库**



## `git status`

**列出当前目录所有还没有被git管理的文件和被git管理且被修改但还未提交(git commit)的文件**

下面是命令执行后返回内容中的个类型

==index即为暂存区==

==Git Directory即为工作区==

> ”Changes to be committed“中所列的内容是在Index中的内容，commit之后进入Git Directory。
>
> “Changed but not updated”中所列的内容是在Working Directory中的内容，add之后将进入Index。
>
> “Untracked files”中所列的内容是尚未被Git跟踪的内容，add之后进入Index
>
> 通过`git status -uno`可以只列出所有已经被git管理的且被修改但没提交的文件。



## `git remote `

**为了便于管理，Git要求每个远程主机都必须指定一个主机名。`git remote`命令就用于管理主机名。**

**不带选项的时候，`git remote`命令列出所有远程主机。**

### `git remote add`命令用于添加远程主机。

> ```
> $ git remote add <主机名> <网址>
> ```

#### `git remote add origin +仓库地址`

**关联远程仓库**

若是出现`fatal: remote origin already exists`说明已经关联，使用`git remote rm origin`删除远程仓库

### `git remote rm`命令用于删除远程主机。

> ```
> $ git remote rm <主机名>
> ```



## `git pull`

`git pull`命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。

它的完整格式 `git pull <远程主机名> <远程分支名>:<本地分支名>`

### ` git pull origin next:master`

上面的命令表示取回`origin`主机的`next`分支，与本地的`master`分支合并

**如果远程分支是与当前分支合并，则冒号后面的部分可以省略。**

> ```
> $ git pull origin next
> ```

##`git pull origin master --allow-unrelated-histories`

用于本地仓库和远程仓库相互独立时

**作用**:将远程仓库和本地仓库的历史合并,这样就可以解决`fatal: refusing to merge unrelated histories` 的错误了

## 强制覆盖本地代码（与git远程仓库保持一致）

#### `git fetch --all`

#### `git reset --hard origin/master`

#### `git pull`

## git push

**`git push`命令用于将本地分支的更新，推送到远程主机**

**完整格式为**`git push <远程主机名> <本地分支名>:<远程分支名>`

### `git push origin`

上面命令表示，将当前分支推送到`origin`主机的对应分支。

如果当前分支只有一个追踪分支，那么主机名都可以省略。

### `git push origin master`=>`git push origin master:master`

将本地的`master`分支推送到`origin`主机的`master`分支

### `git push -u origin master`

将本地的`master`分支推送到`origin`主机，同时指定`origin`为默认主机，后面就可以不加任何参数使用`git push`了。

`-u`选项指定了一个默认主机

### `git push --all origin`

上面命令表示，将所有本地分支都推送到`origin`主机。

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。

> ```
> $ git push --force origin 
> ```

最后导致远程主机上更新的版本被覆盖

## 分支操作

### `git checkout -b 分支名字`:从当前使用的分支创建一个新的分支

### `git branch -d 分支名字`:删除分支

#### `git checkout 分支名字`: 切换分支

#### `git branch -a`:查看远程分支

#### `git branch`:查看本地分支

#### `git push --set-upstream 远程主机 本地要上传的分支名字 `:将本地分支推送到远程主机

#### `git branch -v`:查看各个分支最后一个提交对象的信息

## 储藏（Stashing）

经常有这样的事情发生，当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。解决这个问题的办法就是`git stash`命令。

“‘储藏”“可以获取你工作目录的中间状态——也就是你修改过的被追踪的文件和暂存的变更——并将它保存到一个未完结变更的堆栈中，随时可以重新应用。

现在你想切换分支，但是你还不想提交你正在进行中的工作；所以你储藏这些变更。为了往堆栈推送一个新的储藏，只要运行 `git stash`

这时，你可以方便地切换到其他分支工作；你的变更都保存在栈上。要查看现有的储藏，你可以使用 `git stash list`

你可以重新应用你刚刚实施的储藏，所采用的命令就是之前在原始的 stash 命令的帮助输出里提示的：`git stash apply`。

如果你想应用更早的储藏，你可以通过名字指定它，像这样：`git stash apply stash@{2}`。如果你不指明，Git 默认使用最近的储藏并尝试应用它：



# git常用术语

#### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区：

![working-dir](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849082162373cc083b22a2049c4a47408722a61a770000/0)

#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

#### `git add、 git commit`原理

当你在工作区增删了文件，或者对文件进行了修改，git都会进行记录和追踪，进行`git add`操作就是将这些变动添加到==暂存区==，`暂存区`可以看作是当前工作区的**镜像**，此时工作区的内容并没有发生变化。

`git commit`就是将这些改变提交，从而工作区的内容更新为`暂存区`的内容
