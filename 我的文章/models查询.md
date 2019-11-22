#### models查询

###### filter查询

1.__contains

shell命令下查询：Blog.objects.filter(title__contains ="django")------------------>返回一个queryset[]查询(查询集)只能输入一个值。加上一个"i"后不区别大小写【sql等数据库中】

2.__range(一个范围,使用元组)

Blog.objects.filter(id__range =(30,45))

3.__in （其中之一，可以传入一个列表，传多个值。）

Blog.objects.filter(id__in = [3,6,9])

4.__gt  **大于**

5.__gte **大于等于**

6.__it **小于**

7.__ite **小于等于**

8.__startswith

9.__endswith

#### F

**作用:用于查询间逻辑关系**

**导入语句**:from django.db.models import Q

BookInfo.objects.filter(read__gt=**F**('comment')) 表示查询阅读量大于评论量的书

#### Q查询

**左右:用于类的属性之间比较** (~,|,&)
**导入语句**:from django.db.models import F

BookInfo.objects.filter(Q(id__ gt=5 )|Q(id___ lt=10)) 查询ID大于5小于10的书

BookInfo.objects.filter(~Q(id=5)) 查询ID不等于5的书

#### 聚合函数

Count

Sum

Max

Ave

**用法**:BookInfo.objects.all().aggregate(函数(属性))

**示例**:BookInfo.objects.all().aggregate(Sum(read)) 查询所有图书阅读量总和



#### 查询集

###### 特性

1.惰性 不进行真正使用不会触发SQL查询

2.缓存 查询过的结果会发生缓存





#### 模型管理器

每一个模型类可以配备一个模型管理器

```python
# 模型管理器    
class BookInfoManager(models.Manager):
        def create_book(self, title, pub_date):
        b = BookInfo()
        # class_name = self.model
        # b = class_name()
        b.btittle = title
        b.bpub_date = pub_date
        # 保存了
        b.save()
        # 返回
        return b
# 模型类
class BookInfo(models.Model):
    btittle = models.CharField(max_length=100)
    bpub_date = models.DateField()
    manager = BookInfoManager()

```

可以通过BookInfo.manager.create_book()来创建一个实例对象

在管理器中使用self.model就可以创建一个其对应的模型类对象

#### 指定模型类对应的表名

如果模型类所在文件夹名字发生改变,那么表名也会发生改变,为了避免这种情况,需要在模型类中指定表名

```python
class BookInfo(models.Model):
    btittle = models.CharField(max_length=100)
    bpub_date = models.DateField()
    manager = BookInfoManager()
    class Meta:
        db_table = 'bookinfo'
```

