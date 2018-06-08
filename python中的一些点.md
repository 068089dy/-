# python个人笔记

参考：
[知乎专栏-优雅的python](https://zhuanlan.zhihu.com/p/25892022)
[廖雪峰的教程](https://liaoxuefeng.com/)

### 1.变量交换
```
>>> a, b = b, a
```
### 2.赋值技巧
```
>>> colors = "red", "green", "blue"
>>> red, green, blue = colors
```
### 3.字符串连接
```
>>> colors = ["red", "green", "blue"]
>>> '- '.join(colors)       #join()在内存中只会产生一个字符串对象
'red- green- blue'
```
### 4.if/else三目运算
```
>>> a = 1
>>> text = '1' if a == 1 else '2'
>>> text
'1'
>>> text = '1' if a != 1 else '2'
>>> text
'2'
```
### 5.for/else与try/else
```
# 若for循环完全执行完，没有break跳出，则执行else后面的内容
for ...
else ..
```
### 6.容器（list,tuple,dict,set）
list的添加
```
>>> l = ['1','2','3']
>>> l.append('a')
>>> l
['1', '2', '3', 'a']
```
tuple的初始化：当tuple初始化只有一个元素时，这个元素后面必须加逗号。
```
>>> l = (1)
>>> l
1
>>> l = (1,)
>>> l
(1,)
>>> l = ((1,2))
>>> l
(1, 2)
>>> l = ((1,2),)
>>> l
((1, 2),)
```

设置dict默认值
```
# 法一：
>>> data = [("1", 1), ("2", 2), ("3", 3)]
>>> groups = {}
>>> for key, vlaue in data:
...     groups.setdefault(key, []).append(vlaue)
...
>>> groups
{'3': [3], '1': [1], '2': [2]}
```

```
# 法二：
>>> from collections import defaultdict
>>> groups = defaultdict(list)
>>> for (key, value) in data:
...     groups[key].append(value)
...
>>> groups
defaultdict(<class 'list'>, {'3': [3], '1': [1], '2': [2]})
```

dict.update()
```
>>> a = {"1":1, "2":2}
>>> b = {"3":3, "4":4}
>>> a.update(b)
>>> a
{'2': 2, '1': 1, '4': 4, '3': 3}
>>> a.update(b)
```

### 6.list与deque
list是一个数组
```
```

### 7.Ture和False
各种类型的数据怎样表示Ture和False，以表列出：

|类型   |false                      |ture                      |
|-------|---------------------------|--------------------------|
|bool   |False                      |Ture                      |
|str    |''(空字符串)               |非空字符串                 |
|int    |0                          |非0                       |
|容器   |[],(),{},set() (空容器)    |至少有一个元素的容器对象    |
|None   |None                       |非None对象                |

### 8.None

### 9.遍历

```
# 多项list的遍历
l = (
    ("1","11"),
    ("2","22"),
)

for l1, l2 in l:
    print(l1, l2)

1 11
2 22
```

```
# 带有索引位置的便利
>>> nums = ["1", '2', '3']
>>> for i, num in enumerate(nums):
...     print(i, "-", num)
...
0 - 1
1 - 2
2 - 3
```

```
# zip同时遍历两个迭代器
>>> nums = ["1", '2', '3']
>>> colors = ["red", "blue", "green", "yellow"]
>>> for num, color in zip(nums, colors):
...     print(num, color)
...
1 red
2 blue
3 green
```

```
# zip遍历返回一个元组tuple
>>> nums = ["1", '2', '3']
>>> colors = ["red", "blue", "green", "yellow"]
>>> for i in zip(nums, colors):
...     print(i)
...
('1', 'red')
('2', 'blue')
('3', 'green')
```

```
# 反向迭代

```

### 链式比较
```
>>> a = 2
>>> if 1 < a < 3:
...     print("yes")
...
yes  
```

### 装饰器
简单的例子
```
def log(func):
    def wrapper(*args, **kw):
        print(func.__name__)
        return func(*args, **kw)
    return wrapper

@log        #相当于now = log(now)
def now():
    pass

now()

```

decorator传入参数(3层嵌套的decorator)
```
def log(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print(text, func.__name__)
            return func(*args, **kw)
        return wrapper
    return decorator

@log('execute')
def now():
    pass

now()       #execute now

n = log('execute')(now)

#execute wrapper
#execute now
n()

└─▪ python3 demo.py
execute now
execute wrapper:decorator(func)返回的那个wrapper()函数名字就是'wrapper'，所以，需要把原始函数的__name__等属性复制wrapper()函数中
execute now
```

一个完整的decorator

```
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):           # *args就是传入的func的参数列表，他是一个tuple，是只读的。
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

@log("ss")
def now():
    pass

n = log("ss")(now)
n()

└─▪ python3 demo.py
ss now():
ss now():
```

### 上下文
通过自定义类中的__enter__和__exit__方法实现上下文管理器
```
class A(object):
    def __init__(self):
        self.num = 1

    def __enter__(self):
        return self.num

    def __exit__(self, type, value, traceback):
        # type,value,traceback分别是错误类型，值，追踪栈
        print("exit")
        return True     #返回true表示不抛出错误

with A() as a:
    # as后面的a就是A中__enter__方法的返回值
    print(a)
    # 最后执行__exit__方法
```

### __name__与“__main__”
我们一般这样写程序的入口：
```
if __name__ == "__main__":
    ...
```
其中，__name__就是指当前文件，eg：
在统一目录下建立demo2.py和demo.py
```

# demo2.py
def func_demo2():
    print(__name__)

# demo.py
from demo2 import func_demo2

func_demo2()
print(__name__)

# 打印：
demo2
__main__
```

### *args和**kwargs
```
#args是一个tuple，kwargs是一个dict
def func(*args,**kwargs):
    print(args)
    print(kwargs)

t = (1,2,3)
d = {
    "1":1,
    "2":2
}

func(1,2,3)
print("----------")
func(*t)
print("----------")
func(**d)
```

### 生成器
生成器的每一个元素都是根据前面的元素推断而来，节约内存。
```
# demo.py
def fib():
    i = 0
    while i < 9:
        yield i
        i +=1

for i in fib():
    print(i)

└─▪ python3 demo.py
0
1
2
3
4
5
6
7
8
```

### 删除变量
```
>>> a = 1
>>> a
1
>>> del a
>>> a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'a' is not defined
```
也可以删除容器中的某一元素
```
>>> l
['1', '2', '3', 'a']
>>> del l[0]
>>> l
['2', '3', 'a']
```
### ascii码
```
>>> ord('D')
68
>>> ord('Y')
89
>>> ord('0')
48
>>> ord(')')
41
```
### 判断str是否可以转化为数字(isdigit())
```
>>> '4'.isdigit()
True
>>> 'a'.isdigit()
False
```
