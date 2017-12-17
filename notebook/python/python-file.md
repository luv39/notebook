# python文件的读取

```python
f = open('hello.txt', 'r')
#(r 读)(w 写)(a 追加)
f.read()#把所有内容读入内存
f.close()#关闭文件
f.read(size)#读取size个字节的内容
f.readline()#每次读取一行
f.readlines()#读取所有的内容并按行返回list
for line in f.readlines():
    print line.strip()#把末尾的'\n'删掉
```

# python文件的写入

```python
f = open("hello.txt", 'w')
f.write("hello world")#写入字符串
f.writelines("hello","world")#写入字符串数组
```

# python文件指针

```python
import os

f = open("hello.text", 'r')
f.tell()#目前文件指针所在的位置
f.seek(0, os.SEEK_SET)#前面一个参数是偏移量,后面一个是相对位置,os.SEEK_SET为文件开头,值为0
f.seek(0, os.SEEK_CUR)#os.SEEK_CUR为文件指针当前位置,值为1
f.seek(0, os.SEEK_END)#os.SEEK_END为文件指针结尾位置,值为2
```

# python标准文件
* 文件标准输入:sys.stdin
* 文件标准输出:sys.stdout
* 文件标准错误:sys.stderr

# python文件命令行参数,sys.argv
```python
import sys

if __name__ == '__main__':
    print len(sys.argv)
    for arg in sys.argv:
        print arg
```

# python文件编码格式

# 使用os模块打开文件
```python
import os

os.open(filename, flag)
```
> flag:打开文件方式
* os.O_CREAT创建文件
* os.O_RDONLY只读方式打开
* os.O_WRONLY只写方式打开
* os.O_RDWR读写方式打开

```python
fd = os.open("hello.txt", os.O_CREAT | os.O_RDWR)
os.write(fd, "imooc")#写入
os.lseek(fd, 0, os.SEEK_SET)#文件指针定位
str1 = os.read(fd, size)#读
os.close(fd)#文件关闭
```