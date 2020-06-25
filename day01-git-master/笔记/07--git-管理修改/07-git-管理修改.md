# GIT

## GIT管理修改

现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对demo.txt做一个修改，比如加一行内容：

```basic
$ cat demo.txt
git --version //查看git版本
git config --global user.name //查看全局用户名
git add //告诉git把文件添加到仓库
git commit -m '说明内容'
git status
```

然后，添加：

```bash
$ git add demo.txt
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   demo.txt
```

然后，再修改demo.txt：

```bash
$ cat demo.txt
git --version //查看git版本
git config --global user.name //查看全局用户名
git add //告诉git把文件添加到仓库
git commit -m '说明内容'
git status //获取git管理的文件的状态
```

提交：

```bash
$ git commit -m '测试管理修改'
[master 7fba6e4] 测试管理修改
 1 file changed, 2 insertions(+), 1 deletion(-)
```

提交后，再看看状态：

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   demo.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

第二次的修改没有被提交？

我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用`git diff HEAD -- demo.txt`命令可以查看工作区和版本库里面最新版本的区别：

```bash
$ git diff HEAD -- demo.txt
diff --git a/demo.txt b/demo.txt
index aa7bb2d..b806459 100644
--- a/demo.txt
+++ b/demo.txt
@@ -2,4 +2,4 @@ git --version //查看git版本
 git config --global user.name //查看git全局用户名
 git add //告诉git把文件添加到仓库
 git commit -m '说明内容'
-git status
\ No newline at end of file
+git status //获取git管理的文件的状态
\ No newline at end of file
```

可见，第二次修改确实没有被提交。

那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

好，现在，把第二次修改提交了