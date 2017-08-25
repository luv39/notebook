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

git对象
-----

