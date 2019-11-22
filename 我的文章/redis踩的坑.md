# redis踩的坑

#### 问题:

```
redis celery AttributeError: 'str' object has no attribute 'items'
```

#### 解决:

redis版本问题
报错版本redis=3.2.0，降低版本redis=2.10.6后，解决

#### 问题:Celery运行错误：拒绝反序列化pickle类型的不可信内容

```
in loads
    raise self._for_untrusted_content(content_type, 'untrusted')
kombu.exceptions.ContentDisallowed: Refusing to deserialize untrusted content of type pickle (application/x-python-serialize)
```

#### 解决:配置对应的配置项

```python
from kombu import serialization
serialization.registry._decoders.pop("application/x-python-serialize")

app.conf.update(
    CELERY_ACCEPT_CONTENT = ['json'],
    CELERY_TASK_SERIALIZER = 'json',
    CELERY_RESULT_SERIALIZER = 'json',
)
```

