#### 关于使用Iteratorb遍历集合时进行修改引发的java.util.ConcurrentModificationException 异常

##### 产生原因:

- 在用Iterator迭代时,对集合进行的增删和清除操作的次数都记录在`modCount`变量里面
- 在创建`Iterator`的时候会将`modCount`赋值给`expectedModCount`，在遍历ArrayList过程中，没有其他地方可以设置`expectedModCount`了，因此遍历过程中`expectedModCount`会一直保持初始值
- 而每次迭代都会调用`checkForComodification`方法，该方法会判断`modCount`是否等于`expectedModCount`.
- 所以迭代时对集合的修改会导致`expectedModCount`和`modCount`不相等,从而报错.

##### 解决办法:

网上普遍分为**单线程**和**多线程**两种情形

我个人是在对集合进行add操作时发现的,我先说说**remove操作**:可以调用Iterator自带的remove方法,此方法操作最后会将`modCount`赋值给`expectedModCount`

至于**add操作**,应该可以把迭代时要进行的操作在新的集合进行记录,在遍历结束后统一进行操作

##### 总结: