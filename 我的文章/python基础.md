# python基础

## 4月2日

````python
print('\\qwe')
````

结果`\qwe`

````python
print(r'\\qwe')
````

结果`\\qwe`

Python允许用`r''`表示`''`内部的字符串默认不转义

若是输出中有双引号

````
print
````



python中的布尔值可以用`and`、`or`和`not`运算。对应C语言的`与 或 非`

Python中，通常用全部大写的变量名表示常量：PI = 3.14 15926

## #`\` 是除法运算， 结果为浮点数， `\\`运算结果为整数



## ###ord()和chr()

ord()获取字符的整数表示

chr()将整数转换成对应的字符

```
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```



## 4月3日

### list

1. `list.insert(index, value)`在index位置插入value
2. list.pop()删除最后一个元素
3. list.pop(index)删除index位置的元素
4. list.append()在末尾添加元素
5.  enumerate()使用
   - 如果对一个列表，既要遍历索引又要遍历元素时，首先可以这样写
   - for index, item in enumerate(list1):

###tuple

1 .tuple.index(value, start, end)找出从start到end位置之间value出现的位置

2.tuple.count(value)找出在tuple中value出现的次数

### dict

````python
d = {'丁珂':12, 'bob':34, 'Mike':45} # 构造一个字典
````

**一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉**

**由于key在dict中不存在的时候会报错，因此我们在查询前使用`key in dict`来检测，如果返回True则存在**



通过dict提供的`get()`方法，如果key不存在，可以返回`None`，或者自己指定的value

````python
d.get('Thomes', -1) #若是Thomes不存在则会返回-1，-1是自己指定的返回值，若自己不指定，则返回None
````



> 和list比较，dict有以下几个特点：
>
> 1. 查找和插入的速度极快，不会随着key的增加而变慢；
> 2. 需要占用大量的内存，内存浪费多。
>
> 而list相反：
>
> 1. 查找和插入的时间随着元素的增加而增加；
> 2. 占用空间小，浪费内存很少

### set

set的初始化必须给定一个iterator可迭代对像

比如

````python
s = set([1,2,3,4]) # 初始为list
s = set((1,2,3,4)) # 初始为tuple
````

1. s.discard(key) 如果key存在,删除key，否则返回None
2. s.pop()随机删除元素
3. s.remove(key)删除指定key，若不存在则报错
4. s.add(key)添加元素，相同则覆盖
##4月4日

### 常用函数

max(value1, value2, value3,....)返回多个值中最大者

pow()

reversed(*seq*)

sorted(*iterable*, ***, *key=None*, *reverse=False)*

isinstance(testType, (int, float, str......))

### 函数基本规则

定义函数时，需要确定函数名和参数个数；

如果有必要，可以先对参数的数据类型做检查；

函数体内部可以用`return`随时返回函数结果；

函数执行完毕也没有`return`语句时，自动`return None`。

函数可以同时返回多个值，但其实就是一个tuple

### 默认参数注意事项

定义默认参数要牢记一点：默认参数必须指向不变对象！默认参数也还是一个变量，指向某一块内存，每一次的调用都会使其发生变化。

不可变对像比如`str`,`None`

### 可变参数

用于参数个数不确定的情况

`*nums`表示把`nums`这个list的所有元素作为可变参数传进去。这种写法相当有用，而且很常见。

### 尾递归

尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

### 切片

1.切片是一个完整的类，适用于`list` `tuple` `str`

````python
a = [1,2,3,4,5,6,7]
s = slice(1,3) # 表示取iterator的下标1到2的元素
a[s] # 这里的打印结果应该是2，3
````

2.可以直接在iterator对像后面加上`[]`,来进行选取，语法为`[start:end]`,表示选取下标`start`到`end-1`的元素

````python
a = [1,2,3,4,5]
a[1:4]
````

上面代码表示选取小标为1到3的元素

3.可以按照步长来选取，`[start:end:step]`,即在下标start到end-1元素中每隔step个取出一个

4.`s[::]`表示全部取出

5.a[-4:-1]是合法的 a[-1:-4]非法

## 迭代

1.当我们使用`for`循环时，只要作用于一个可迭代对象，`for`循环就可以正常运行，而我们不太关心该对象究竟是list还是其他数据类型。

2.判断对像是否可迭代使用`collections模块的Iterable类型`

````python
 from collections import Iterable
isinstance('abc', Iterable) # str是否可迭代
````

3.对dict进行迭代

````python
for key in d:
    print(d[key])
    
for value in d.values():
    print(value)

for key, value in d.items():
    print(str(key) + str(value))
````

4.Python内置的`enumerate`函数可以把一个list变成索引-元素对，这样就可以在`for`循环中同时迭代索引和元素本身：

```python
for i, value in enumerate(['A', 'B', 'C']):
	print(i, value)
```



## 列表生成式



