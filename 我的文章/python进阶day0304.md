## python中的一个常见问题

类名.`_init__.__default__`

## 元类编程

### property动态属性

**补充: 为什么需要`if __name__ == '__main__':`这条语句**

解释: 因为没有语句的话, 在其他模块中引入本模块名时, 所有代码都会被执行一遍

### `__getattr__`和`__getattrbute__`魔法函数

###### 当查找不到属性时, 就会进入`__getattr__`

###### 而`__getattrbute__`函数如果重写, 则访问任何属性都会无条件的先进入`__getattr__`, 即使属性存在

###### `__getattrbute__`是所有属性的入口

### 属性描述符和属性查找过程



***两种种类的描述符：***

　　只要至少实现__get__、__set__、__delete__方法中的一个就可以认为是描述符；

　　***数据描述符（data descriptor）和非数据描述符（non-data descriptors）***

　　***数据描述符：定义了 set 和 get方法的对象***

　　***非数据描述符：只定义了 get 方法的对象。***

### `__init__`和`__new__`

`new`是用来控制对象生成过程, 在对象生成之前调用

`init`是用来完善对象的

如果`new`方法不返回对象, 那么init方法不会被调用

### 自定义元类

#### type动态创建类

语法: type(类名, (父类名,), {属性的键值对:name:'dk', 'say':say})

#### 什么是元类

 元类是创建类的类

#### python中类的实例化过程



