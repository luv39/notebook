# python cook book

[TOC]

## 数据结构和算法

### 将序列分解为单独的变量（序列解包）

任何序列（或可迭代对象）都可以通过一个简单的赋值操作来分解为单独的变量。唯一的要求是变量总数和结构要与序列相吻合。

### 从任意长度的可迭代对象中分解元素

```python
def drop_first_last(grades):
    first, *middle, last = grades # middle将会是一个列表
    return avg(middle)
```

### 保存最后N个元素

我们希望在迭代或是其他形式的处理过程中对最后几项记录做一个有限的历史记录统计。

保存有限的历史记录可算是`collections.deque`的完美应用场景了。
