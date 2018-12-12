# python命令行参数和getopt模块

[TOC]

有时候我们需要写一些脚本处理一些任务，这时候往往需要提供一些命令行参数，根据不同参数进行不同的处理，在Python里的命令行参数是存储在`sys.argv`里，`argv`是一个列表，第一个元素也为程序名称。

而Python的标准库里实际上有专门处理命令行参数的`getopt`模块，里面的提供了2个函数和一个类，我们主要使用`getopt`函数，先看下函数原型：

```python
def getopt(args, shortopts, longopts = [])：
```

先看一个例子，这样会便于理解。

```python
try:
	opts, args = getopt.getopt(sys.argv[1:], "ho:", ["help", "output="])
except getopt.GetoptError:
    # print help information and exit:
for name, value in opts:
	print name, value
for item in args:
	print item
```

> 1. 处理所使用的函数叫`getopt()` ，因为是直接使用import 导入的`getopt` 模块，所以要加上限定`getopt` 才可以。 
>
> 2. 使用`sys.argv[1:]` 过滤掉第一个参数（它是执行脚本的名字，不应算作参数的一部分）。 
>
> 3. 使用短格式分析串"ho:" 。当一个选项只是表示开关状态时，即后面不带附加参数时，在分析串中写入选项字符。当选项后面是带一个附加参数时，在分析串中写入选项字符同时后面加一个":" 号 。所以"ho:" 就表示"h" 是一个开关选项；"o:" 则表示后面应该带一个参数。 
>
> 4. 使用长格式分析串列表：["help", "output="] 。长格式串也可以有开关状态，即后面不跟"=" 号。如果跟一个等号则表示后面还应有一个参数 。这个长格式表示"help" 是一个开关选项；"output=" 则表示后面应该带一个参数。 
>
> 5. 调用`getopt` 函数。函数返回两个列表：`opts` 和`args`。`opts` 为分析出的格式信息。`args` 为不属于格式信息的剩余的命令行参数。opts 是一个两元组的列表。每个元素为：( 选项串, 附加参数) 。如果没有附加参数则为空串'' 。
>
> 6. 整个过程使用异常来包含，这样当分析出错时，就可以打印出使用信息来通知用户如何使用这个程序。 

如上面解释的一个命令行例子为： 

***'-h -o file --help --output=out file1 file2'***

在分析完成后，`opts` 应该是： 

***[('-h', ''), ('-o', 'file'), ('--help', ''), ('--output', 'out')]***

而`args` 则为： 

***['file1', 'file2']***

下面就是根据不同参数处理：

```python
for o, a in opts:
    if o in ("-h", "--help"):
        usage()
        sys.exit()
    if o in ("-o", "--output"):
        output = a
```



