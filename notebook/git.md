# git教程

## git初始化

> 初始化git
* `git init`

> 设置自己的用户名和email
* `git config --global user.name "luv39"`
* `git config --global user.email "1519667170@qq.com"`

> 设置ssh密钥
* `ssh-keygen -t rsa`

> git命令输出中开启颜色显示
* `git config --global color.ui true`

> 新建文件加入到版本库
* `git add git.md`

> 提交
* `git commit -m "creat git.md"`

## git暂存区

> 查看提交日志(附加的--stat参数可以看到每次提交的文件变更统计)
* `git log --stat`

> 查看文件状态(-s参数显示精简格式的状态输出)
* `git status -s`

> 工作区和HEAD比较
* `git diff HEAD`
* `git diff master`

> 工作区和stage比较
* `git diff`

> stage和HEAD比较
* `git diff --cached`
* `git diff --cached HEAD`

## git版本回退

* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
* 穿梭前，用`git log`可以查看提交历史，以便确定回退到哪个版本。
* 要返回未来，用`git reflog`查看命令历史，以便确定回到未来的哪个版本。

## 撤销修改

* 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file` 。（`git checkout`其实是用版本库里的版本替换工作区的版本）
* 当你不想提交暂存区的修改时，用命令`git reset HEAD file`,就可以回到最后一次提交的版本。

## 删除文件

> 如果有一个文件被删除了，你有两个选择
* 你确实要删除这个文件，用`git rm`命令删除文件，并且`git commit`。
* 你不想删除这个文件，用`git checkout -- file`。

## 添加远程库

* 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`。
* 关联后，使用命令`git push -u origin master`第一次推送master分支所有内容。
* 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。

## 从远程库克隆

* 首先复制远程仓库的地址，然后用命令`git clone`克隆远程仓库。

## 创建与合并分支

* 查看分支：`git branch`
* 创建分支：`git branch <name>`
* 切换分支：`git checkout <name>`
* 创建加切换分支：`git checkout -b <name>`
* 合并某分支到当前分支：`git merge <name>`
* 删除分支：`git branch -d <name>`

## 标签管理

* 新建一个标签`git tag <name> commit_id`默认标签到最新的commit上。
* 指定标签信息`git tag -a <tagname> -m "balabala"`。
* 命令`git tag`可以查看所有标签。
* 命令`git show <tagname>`可以查看标签的详细信息。

* 推送一个本地标签`git push origin <tagname>`
* 推送全部未推送的标签`git push origin --tags`。
* 删除一个本地标签`git tag -d <tagname>`。
* 删除一个远程标签`git push origin :refs/tags/<tagname>`。

## 搭建git服务器

> 创建git用户
* `useradd git`
* `passwd git`

> 初始化git仓库
* `mkdir /git`
* `cd /git`
* `git init --bare gittest.git`
* `chown -R git:git gittest.git`
* 接下来就可以用`git@ip:/git/gittest.git`这个链接进行克隆,提交等动作了
