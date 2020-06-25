# GIT

## GIT撤销修改

我们在`demo.txt`中添加了一行：

```bash
$ cat demo.txt
git --version //查看git版本
git config --global user.name //查看git全局用户名
git add //告诉git把文件添加到仓库
git commit -m '说明内容'
git status //获取git管理的文件的状态
我要升职加薪干掉老板
```

提交这个文件我们基本上就是失业状态了

纠正这个错误我们以删掉最后一行，手动把文件恢复到上一个版本的状态。但是如果用`git status`查看一下：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   demo.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以发现，Git会告诉我们，`git checkout -- file`可以丢弃工作区的修改：

```
$ git checkout -- demo.txt
```

命令`git checkout -- demo.txt`意思就是，把`demo.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`demo.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`demo.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

现在，看看`demo.txt`的文件内容：

```bash
$ cat demo.txt
git --version //查看git版本
git config --global user.name //查看git全局用户名
git add //告诉git把文件添加到仓库
git commit -m '说明内容'
git status //获取git管理的文件的状态
```

文件内容果然复原了。

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

现在假定我们不但写了一些胡话，还`git add`到暂存区了：

```bash
$ cat demo.txt
git --version //查看git版本
git config --global user.name //查看git全局用户名
git add //告诉git把文件添加到仓库
git commit -m '说明内容'
git status //获取git管理的文件的状态
我要升职加薪干掉老板

$ git add demo.txt
```

庆幸的是，在`commit`之前，我们发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   demo.txt
```

Git同样告诉我们，用命令`git restore --staged `可以把暂存区的修改撤销掉（unstage），重新放回工作区：

再用`git status`查看一下，现在暂存区是干净的，工作区有修改：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   demo.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

或者使用: `git reset HEAD <file>`

```bash
$ git reset HEAD demo.txt
Unstaged changes after reset:
M       demo.txt
```

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

再用`git status`查看一下，现在暂存区是干净的，工作区有修改：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   demo.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

还记得如何丢弃工作区的修改吗？

```bash
$ git checkout -- demo.txt

$ git status
On branch master
nothing to commit, working tree clean
```

现在，假设我们不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得版本回退一节吗？可以回退到上一个版本。不过，这是有条件的，就是我们还没有把自己的本地版本库推送到远程。Git是分布式版本控制系统，之后我们会讲到远程版本库，一旦我们把胡话提交推送到远程版本库，我们就...

