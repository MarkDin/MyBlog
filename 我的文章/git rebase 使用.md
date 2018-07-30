# git rebase 使用

原创 2014年03月31日 13:13:41

git rebase 不会取回代码 要用git fetch先取回， git rebase 是合并代码。

（1）首先用git fetch返回服务器上的代码

（2）首先用git rebase origin/master 合并

（3）如果发生冲突了会提示， 然后可以使用git diff查看冲突， 在手工改掉冲突， 在用git add ‘文件名’ 添加修改后文件，最后用git rebase --continue继续没完成的合并

（4）最后就可以用git push 更新到服务器上去。

转自： http://blog.chinaunix.net/uid-26952464-id-3352144.html

[Git Community Book 中文版](http://gitbook.liuhui998.com/index.html)书上，摘录如下：

 

一、基本

git rebase用于把一个分支的修改合并到当前分支。

假设你现在基于远程分支"origin"，创建一个叫"mywork"的分支。

$ git checkout -b mywork origin

假设远程分支"origin"已经有了2个提交，如图

[![img](http://blog.chinaunix.net/attachment/201209/19/26952464_1348017759qs4k.png)](http://blog.chinaunix.net/attachment/201209/19/26952464_1348017759qs4k.png)

现在我们在这个分支做一些修改，然后生成两个提交(commit).

$ vi file.txt

$ git commit

$ vi otherfile.txt

$ git commit

...

但是与此同时，有些人也在"origin"分支上做了一些修改并且做了提交了. 这就意味着"origin"和"mywork"这两个分支各自"前进"了，它们之间"分叉"了。

[![img](http://blog.chinaunix.net/attachment/201209/19/26952464_1348017823wiQB.png)](http://blog.chinaunix.net/attachment/201209/19/26952464_1348017823wiQB.png)

在这里，你可以用"pull"命令把"origin"分支上的修改拉下来并且和你的修改合并； 结果看起来就像一个新的"合并的提交"(merge commit):

[![img](http://blog.chinaunix.net/attachment/201209/19/26952464_13480178579bG6.png)](http://blog.chinaunix.net/attachment/201209/19/26952464_13480178579bG6.png)

但是，如果你想让"mywork"分支历史看起来像没有经过任何合并一样，你也许可以用 git rebase:

$ git checkout mywork

$ git rebase origin

这些命令会把你的"mywork"分支里的每个提交(commit)取消掉，并且把它们临时 保存为补丁(patch)(这些补丁放到".git/rebase"目录中),然后把"mywork"分支更新 为最新的"origin"分支，最后把保存的这些补丁应用到"mywork"分支上。

[![img](http://blog.chinaunix.net/attachment/201209/19/26952464_13480178888X41.png)](http://blog.chinaunix.net/attachment/201209/19/26952464_13480178888X41.png)

 

当'mywork'分支更新之后，它会指向这些新创建的提交(commit),而那些老的提交会被丢弃。 如果运行垃圾收集命令(pruning garbage collection), 这些被丢弃的提交就会删除. （请查看 git gc)

[![img](http://blog.chinaunix.net/attachment/201209/19/26952464_1348017931Waw1.png)](http://blog.chinaunix.net/attachment/201209/19/26952464_1348017931Waw1.png)

 

二、解决冲突

在rebase的过程中，也许会出现冲突(conflict). 在这种情况，Git会停止rebase并会让你去解决 冲突；在解决完冲突后，用"git-add"命令去更新这些内容的索引(index), 然后，你无需执行 git-commit,只要执行:

$ git rebase --continue

这样git会继续应用(apply)余下的补丁。

在任何时候，你可以用--abort参数来终止rebase的行动，并且"mywork" 分支会回到rebase开始前的状态。

$ git rebase --abort

三、git rebase和git merge的区别

现在我们可以看一下用合并(merge)和用rebase所产生的历史的区别：

[![img](http://blog.chinaunix.net/attachment/201209/19/26952464_1348018047J6ss.png)](http://blog.chinaunix.net/attachment/201209/19/26952464_1348018047J6ss.png)

当我们使用Git log来参看commit时，其commit的顺序也有所不同。

假设C3提交于9:00AM,C5提交于10:00AM,C4提交于11:00AM，C6提交于12:00AM,

对于使用git merge来合并所看到的commit的顺序（从新到旧）是：C7 ,C6,C4,C5,C3,C2,C1

对于使用git rebase来合并所看到的commit的顺序（从新到旧）是：C7 ,C6‘,C5',C4,C3,C2,C1

 因为C6'提交只是C6提交的克隆，C5'提交只是C5提交的克隆，

从用户的角度看使用git rebase来合并后所看到的commit的顺序（从新到旧）是：C7 ,C6,C5,C4,C3,C2,C1

 

 

参考资料：[http://www.cnblogs.com/kym/archive/2010/08/12/1797937.html](http://www.cnblogs.com/kym/archive/2010/08/12/1797937.html)

[git rebase小计(转)](http://www.cnblogs.com/kym/archive/2010/08/12/1797937.html)git rebase，顾名思义，就是重新定义（re）起点（base）的作用，即重新定义分支的版本库状态。要搞清楚这个东西，要先看看版本库状态切换的两种情况：我们知道，在某个分支上，我们可以通过git reset，实现将当前分支切换到本分支以前的任何一个版本状态，即所谓的“回溯”。即实现了本分支的“后悔药”。也即版本控制系统的初衷。还有另一种情况，当我们的项目有多个分支的时候。我们除了在本地开发的时候可能会“回溯”外，也常常会将和自己并行开发的别人的分支修改添加到自 己本地来。这种情况下很常见。作为项目管理员，肯定会不断的合并各个子项目的补丁，并将最新版本推送到公共版本库，而作为开发人员之一，提交自己的补丁之 后，往往需要将自己的工作更新到最新的版本库，也就是说把别的分支的工作包含进来。举个例子来说吧！假设我们的项目初期只有一个master分支，然后分支上作过两次提交。这个时候系统只有一个master分支，他的分支历史如下：master0（初始化后的版本）
||
v
master1（第一次提交后的版本）
||
v
master2（第二次提交后的版本）这个时候，我们可以通过git reset将master分支（工作目录、工作缓存或者是版本库）切换到master1或者master0版本，这就是前面所说的第一种情况。
假设我们这里把master分支通过git reset回溯到了master1状态。那么这个时候系统仍然只有一个master分支，分支的历史如下：master0（初始化后的版本）
||
v
master1（第一次提交后的版本）然后，我们在这里以master1为起点，创建了另一个分支test。那么对于test分支来说，他的第一个版本test0就和master1是同一个版本，此时项目的分支历史如下：master0（初始化后的版本）
||
v
master1（第一次提交后的版本）===test0（test分支，初始化自master分支master1状态）这个时候，我们分别对master分支、test分支作两次提交，此时版本库应该成了这个样子：master0（初始化后的版本）
||
v
master1===test0==>test1===>test2
||
v
master2===>master3这个时候，通过第一种git reset的方式，可以将master分支的当前状态（master3）回溯到master分支的master0、master1、master2状态。 也可已将test分支当前状态（test2）回溯到test分支的test0、test1状态，以及test分支的父分支master的master0、 master1状态。那么。如果我要让test分支从test0到test2之间所有的改变都添加到master分支来，使得master分支包含test分支的所有修改。这个时候就要用到git rebase了。首先，我们切换到master分支，然后运行下面的命令，即可实现我们的要求：
**                     git** rebase **test**这个时候，git做了些什么呢？先将test分支的代码checkout出来，作为工作目录然后将master分支从test分支创建起的所有改变的补丁，依次打上。如果打补丁的过程没问题，rebase就搞定了如果打补丁的时候出现了问题，就会提示你处理冲突。处理好了，可以运行git rebase –continue继续直到完成如果你不想处理，你还是有两个选择，一个是放弃rebase过程（运行git rebase –abort），另一个是直接用test分支的取代当前分支的（git rebase –skip）。