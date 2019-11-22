## 实现可切片对象

要实现像list一样的可切片对象, 就必须要在自定义的类中实现一些必要的魔法函数

- `__get_item__(self, item)`



## bissect维护已排序序列

bissect包是用来处理已排序序列的

```python
# 演示bissect操作
import bisect
insert_list = []
bisect.insort(insert_list, 3)
bisect.insort(insert_list, 8)
bisect.insort(insert_list, 2)
bisect.insort(insert_list, 5)
bisect.insort(insert_list, 1)
# 打印insert_listprint(insert_list)
# 打印插入元素的位置
print(bisect.bisect(insert_list, 3))
```

## 什么时候我们不该使用列表

array就是c语言中的数组

```python
# array
import array
# array和list的一个重要区别就是array只能存放指定的数据类型
my_array = array.array('i')
my_array.append(1)
print(my_array)
```

## 列表推导式, 生成器表达式, 字典推导式

##### 列表表达式

```
odd_list = [i for i in range(25) if i&1 == 0]
```

**列表生成式性能高于直接列表操作**

##### 生成器表达式

```python
# 生成器表达式
odd_list = (i for i in range(25) if i&1 == 0)
print(odd_list)
# <generator object <genexpr> at 0x02B16F90>
for odd in odd_list:    
	print(odd)
# 0, 2, 4, 6....
```

## set和dict详解

`dict`是属于mapping类型的

### dict的两个重要方法

- `setdefault(key, default=)`: 取字典中的key对应的value, 当key不存在时, 会将default的值赋值给key, 并返回值
- `update()`:  更新一个或者多个键值对, 可接受字典,  键值对, 元组序列, 包含元组的列表

```python
d.update(a=1)
d.update({'b':3})
d.update([('bb','dfd'),('yy', 'tt')])
print(d)
```

### dict的子类

**不要直接去继承`dict`, 会出现很多错误**

可以继承`collections`模块中的`UserDict`

`defaultDict`

### set和frozenset

set集合无序, 不重复

##### 两个初始化方法

- `my_set = {'a', 'b'}`
- my_set = set([1,2,3,4])

###### `frozeset`创建好就不可变了(不能增删和修改), 可以作为`dict`的`key`

不可变对象都是可以哈希的, 比如`str, tuple, frozenset`

自己的类可以实现`__hash__()`函数, 来变成可哈希对象

- `dict`中元素的存储顺序和添加的顺序有关
- `dict`中添加数据可能会改变已有数据的顺序
- `dict`耗费存储空间, 使用`set`去重更佳 

## 对象引用, 可变性和垃圾回收

### python变量到底是什么

python和java中变量的本质不同, java在声明变量时, 就指定了类型和大小; python的变量实质是一个指针, 指针大小都是一样, 可以把python中的变量看成是**便利贴**, python是先生成对象, 再去将变量指向对象

### is 和 == 的区别

python中小整数和小段字符串, python会生成一个全局对象, 因此都是同一个对象

### del语句和垃圾回收

python中垃圾回收算法使用的是**引用计数**

