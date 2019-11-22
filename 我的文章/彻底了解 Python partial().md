# **彻底了解 Python partial()**

文章出处:http://www.imooc.com/article/255115

今天介绍一下 Python 中的偏函数(functools.partial)

> 阅读本文可能需要 5-8 分钟

首先，我们需要简单了解下偏函数的作用：和装饰器一样，它可以扩展函数的功能，但又不完成等价于装饰器。通常应用的场景是当我们要频繁调用某个函数时，其中某些参数是已知的固定值，通常我们可以调用这个函数多次，但这样看上去似乎代码有些冗余，而偏函数的出现就是为了很少的解决这一个问题。举一个很简单的例子，比如我就想知道 100 加任意数的和是多少，通常我们的实现方式是这样的：

```python
# 第一种做法：
def add(*args):
    return sum(args)

print(add(1, 2, 3) + 100)
print(add(5, 5, 5) + 100)

# 第二种做法
def add(*args):
    # 对传入的数值相加后，再加上100返回
    return sum(args) + 100

print(add(1, 2, 3))  # 106
print(add(5, 5, 5))  # 115 
```

看上面的代码，貌似也挺简单，也不是很费劲。但两种做法都会存在有问题：第一种，100这个固定值会返回出现，代码总感觉有重复；第二种，就是当我们想要修改 100 这个固定值的时候，我们需要改动 add 这个方法。下面我们来看下用 parital 怎么实现：

```python
from functools import partial

def add(*args):
    return sum(args)

add_100 = partial(add, 100)
print(add_100(1, 2, 3))  # 106

add_101 = partial(add, 101)
print(add_100(1, 2, 3))  # 107
```

是不是很简单~

大概了解了偏函数的例子后，我们再来看一下偏函数的定义：

```python
类func = functools.partial(func, *args, **keywords)
```

我们可以看到，partial 一定接受三个参数，从之前的例子，我们也能大概知道这三个参数的作用，简单介绍下：

```python
func: 需要被扩展的函数，返回的函数其实是一个类 func 的函数
*args: 需要被固定的位置参数
**kwargs: 需要被固定的关键字参数
# 如果在原来的函数 func 中关键字不存在，将会扩展，如果存在，则会覆盖
```

用一个简单的包含位置参数和关键字参数的示例代码来说明用法：

```python
# 同样是刚刚求和的代码，不同的是加入的关键字参数
def add(*args, **kwargs):
    # 打印位置参数
    for n in args:
        print(n)
    print("-"*20)
    # 打印关键字参数
    for k, v in kwargs.items():
       print('%s:%s' % (k, v))
    # 暂不做返回，只看下参数效果，理解 partial 用法

# 普通调用
add(1, 2, 3, v1=10, v2=20)
"""
1
2
3
--------------------
v1:10
v2:20
"""

# partial
add_partial = partial(add, 10, k1=10, k2=20)
add_partial(1, 2, 3, k3=20)
"""
1
2
3
--------------------
k1:10
k2:20
k3:20
"""

add_partial(1, 2, 3, k1=20)
"""
10
1
2
3
--------------------
k1:20
k2:20
"""
```

最后，我们来看一下官方文档中的解释，相信有了前面的介绍，再回头来看官方文档，应该会比较好理解了，同时也能加深我们的印象：

> functools.partial(func, *args, **keywords)
> Return a new partial object which when called will behave like func called with the positional arguments args and keyword arguments keywords. If more arguments are supplied to the call, they are appended to args. If additional keyword arguments are supplied, they extend and override keywords. Roughly equivalent to:

简单翻译: 它返回一个偏函数对象，这个对象和 func 一样，可以被调用，同时在调用的时候可以指定位置参数 (*args) 和 关键字参数(**kwargs)。如果有更多的位置参数提供调用，它们会被附加到 args 中。如果有额外的关键字参数提供，它们将会扩展并覆盖原有的关键字参数。它的实现大致等同于如下代码：

```python
# 定义一个函数，它接受三个参数
def partial(func, *args, **keywords):
    def newfunc(*fargs, **fkeywords):
        newkeywords = keywords.copy()
        newkeywords.update(fkeywords)
        return func(*args, *fargs, **newkeywords)
    newfunc.func = func
    newfunc.args = args
    newfunc.keywords = keywords
    return newfunc
```

> The partial() is used for partial function application which “freezes” some portion of a function’s arguments and/or keywords resulting in a new object with a simplified signature. For example, partial() can be used to create a callable that behaves like the int() function where the base argument defaults to two:

简单翻译：partial() 是被用作 “冻结” 某些函数的参数或者关键字参数，同时会生成一个带有新标签的对象(即返回一个新的函数)。比如，partial() 可以用于合建一个类似 int() 的函数，同时指定 base 参数为2，代码如下：

```python
# 这个代码很简单，就不啰嗦了
>>> from functools import partial
>>> basetwo = partial(int, base=2)
>>> basetwo.__doc__ = 'Convert base 2 string to an int.'
>>> basetwo('10010')
```

现在再看文档是不是很清晰了。