git初始化
-----

* 初始化git

```
git init
```

* 设置自己的用户名和email

```
git config --global user.name "luv39"
git config --global user.email "1519667170@qq.com"
```

* git命令输出中开启颜色显示

```
git config --global color.ui true
```

* 新建文件加入到版本库

```
git add git.md
```

* 提交

```
git commit -m "creat git.md"
```

git暂存区
-----

* 查看提交日志(附加的--stat参数可以看到每次提交的文件变更统计)

```
git log --stat
```

* 查看文件状态(-s参数显示精简格式的状态输出)

```
git status -s
```

* git diff魔法

工作区和HEAD比较

```
git diff HEAD
git diff master
```

工作区和stage比较

```
git diff
```

stage和HEAD比较

```
git diff --cached
git diff --cached HEAD
```

git版本回退
-----

* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
* 穿梭前，用`git log`可以查看提交历史，以便确定回退到哪个版本。
* 要返回未来，用`git reflog`查看命令历史，以便确定回到未来的哪个版本。

撤销修改
-----

* 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file` 。
* 当你不想提交暂存区的修改时，用命令`git reset HEAD file`,就可以回到最后一次提交的版本。

删除文件
-----

