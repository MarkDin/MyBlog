# python中类的属性

```python
class Animal(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
```

### 动态添加属性

```python
a = Animal()
a.extra_property = 'dfdffd'
```

###使用`__slot__`限制属性

```python
class Animal(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
    __slots__ = ['name', 'age']
```

这样除了`__slot__`指定的属性,不可添加其他属性

### 运行期间使用MethodType来添加属性

- 给单个实例添加

  ```
  b = Animal()
  b.set_color = MethodType(set_color, b, Animal)
  ```

  第二个参数指定被添加的对像

- 给整个类添加

  ```
  Animal.set_other = MethodType(set_color, None, Animal)
  ```

### property装饰器

**作用**:把实例方法变成其同名属性，以支持.号访问，它亦可标记设置限制，加以规范



