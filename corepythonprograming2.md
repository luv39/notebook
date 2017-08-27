python基础
-----

* 模块结构和布局

```python
#/usr/bin/env python          起始行
"this is a test module"      #模块文档

import sys                   #模块导入
import os 

debug = True                 #全局变量定义

class FooClass(object) :     #类定义
    "Foo class"
    pass

def test():                  #函数定义
    "test fuction"
    foo = FooClass()
    if debug:
        print 'run test()'

if __name__ == '__main__':   #主程序
    test()
```

