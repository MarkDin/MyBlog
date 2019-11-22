# python进阶day01

## python中一切皆对象

### python中函数和类都是对象, 属于python的一等公民

- 赋值给一个变量
- 可以添加到一个集合对象中
- 可以作为参数传递给函数
- 可以作为函数的返回值

默认情况下，Python中的成员函数和成员变量都是公开的(public),在python中没有类public,private等关键词来修饰成员函数和成员变量。其实，Python并没有真正的私有化支持，但可用下划线得到伪私有。   尽量避免定义以下划线开头的变量！

　　（1）_xxx      "单下划线 " 开始的成员变量叫做保护变量，意思是只有类实例和子类实例能访问到这些变量，需通过类提供的接口进行访问；不能用'from module import *'导入

　　（2）__xxx    类中的私有变量/方法名 （Python的函数也是对象，所以成员方法称为成员变量也行得通。）," 双下划线 " 开始的是私有成员，意思是只有类对象自己能访问，连子类对象也不能访问到这个数据。

　　（3）__xxx__ 系统定义名字，前后均有一个“双下划线” 代表python里特殊方法专用的标识，如 __init__（）代表类的构造函数。

==`__开头的变量, 通过变量重整变成了_类名__变量名`==

### type, class, obeject之间的关系

- 所有的类默认继承的基类是object类
- type也继承了object
- object类是type类的实例
- type类是自己的实例

### python中的常见内置类型

对象的三个特征

- 身份: 内存中的地址
- 类型
- 值

#### None类型

全局只有一个, python解释器在初始化时会生成一个None的对象

```python
a = None
b = None
id(a) == id(b)
# Ture
```

### 数值类型

- int
- float
- complex
- bool

### 集合

### 映射(dict)

....

##魔法函数

以`__`开头,和`__`结尾的函数

####魔法函数与python的一些内置函数有密切关系

比如: 

- 当你使用`for`迭代对象时, 解释器就会调用`__get_item__()`函数
- 当用`print`打印对象时, 会调用`__str__()`函数
- 当用内置函数`abs()`对对象求绝对值时, 会调用`__abs__()`函数

`__get_item__()`:定义后可以对对象进行迭代和切片



## 深入理解对象和类

### 鸭子类型

python中只有多个类实现了一个共同的方法(方法名一样), 这样就可以实现多态

### 抽象模块(abc模块)

python中并不是通过继承某个类或者实现某个接口来实现特定特性, 而是通过实现相关魔法函数来完成

语法:

class 名字():

#### 几个关于抽象类模块

`abc`模块

`collection`模块

## 使用isinstance而不是type

isinstance可以判断出类的继承情况

type只能判断直接的

### 类变量和实例变量

- 类变量是在类中定义的, 而不是在`__init__`函数中传进来的

- `__init__`函数中的是实例变量
- 实例变量需要创建对象后才会生成和使用
- 类变量可以直接使用, 而不需要生成实例

### 类属性, 实例属性, 以及查找顺序

类名.`__mro__`保存了类的继承关系

### 静态方法, 类方法, 实例方法以及参数

staticmethod

classmethod

### python中的自省机制

dir()函数可以列出类的所有属性名称

`__dict__` python 中预置的`__dict__`属性，是保存类实例或对象实例的属性变量键值对字典

**注意,类的dict并不包含其父类的属性.**

```python
s = Student("吉首大学")
# 通过__dict__查询属性
print(s.__dict__)print(s.name)
# 可以直接给__dict__赋值
s.__dict__['extra_attr'] = 'dk'print(s.extra_attr)
```

####自省是通过一定的机制来查询到对象的内部结构

```python
class Person:    
    name = "user"
class Student(Person):    
    def __init__(self, school_name):        
        self.school_name = school_name
if __name__ == '__main__':    
    s = Student("吉首大学")    
    # 通过__dict__查询属性    
    print(s.__dict__)    
    print(s.name)        
    # 可以直接给__dict__赋值    
    s.__dict__['extra_attr'] = 'dk'    
    print(s.extra_attr)
```

### super函数

####我们继承父类并重写了父类的`__inti__`函数, 为什么还要调用`super()`?

#### super函数的执行顺序是什么?

super()函数并不是按照类的继承顺序进行调用的, 而是按照`MRO`算法中的顺序来执行

## python中的序列类

### 序列类型的分类

- 容器序列 : list, tuple, deque
- 扁平序列 : str, bytes, bytearray, array.array
- 可变序列 : list, deque, bytearray,  array
- 不可变序列: str, tuple, bytes

### 抽象序列的abc继承关系

在python中的collection模块中已经定义好了很多的序列的抽象基类, list, tuple等内置的序列是继承了其中的一部分的, 并且实现了其中定义好的一些魔法函数

### list中的`+`, `+=`, `extend`, `append`

```python
# 单独的+会形成一个新的列表 加号两边的数据类型必须一致
a = [1, 2]
b = [3, 4]
a = b + [5, 6]
# 原地加 会调用extend方法, 对要加的对象进行for循环遍历, 所以数据类型可以不一致
a += (7, 8)
# append会将对象看成一个整体添加到list中
a.append((2,3))
```

