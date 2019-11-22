```python

import re
 
 
c = re.compile(r'\d')
 
s = 'you1are2welcome'
 
# 用指定的内容，替换正则匹配的内容，也可以指定替换次数
ret = c.sub(' ', s, 1)
 
print(ret)
 
 
# 处理函数接收一个参数(每次的匹配结果)
def deal(s):
    return str(int(s.group()) * 2)
 
# 可以认为干预替换过程，传递一个函数即可
ret = re.sub(r'\d', deal, 'you1are2welcome')

```

