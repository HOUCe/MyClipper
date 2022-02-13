# [Python中的分组函数（groupby、itertools）_t15600624671的博客-CSDN博客_python归类分组代码](https://blog.csdn.net/t15600624671/article/details/78614251)

转载自：[https://www.cnblogs.com/dreamer-fish/p/5522687.html](https://www.cnblogs.com/dreamer-fish/p/5522687.html)

```
from operator import itemgetter 
from itertools import groupby 
d1={'name':'zhangsan','age':20,'country':'China'}
d2={'name':'wangwu','age':19,'country':'USA'}
d3={'name':'lisi','age':22,'country':'JP'}
d4={'name':'zhaoliu','age':22,'country':'USA'}
d5={'name':'pengqi','age':22,'country':'USA'}
d6={'name':'lijiu','age':22,'country':'China'}
lst=[d1,d2,d3,d4,d5,d6]



lst.sort(key=itemgetter('country')) 
lstg = groupby(lst,itemgetter('country')) 



for key,group in lstg:
    for g in group: 
        print key,g
返回：
China {'country': 'China', 'age': 20, 'name': 'zhangsan'}
China {'country': 'China', 'age': 22, 'name': 'lijiu'}
JP {'country': 'JP', 'age': 22, 'name': 'lisi'}
USA {'country': 'USA', 'age': 19, 'name': 'wangwu'}
USA {'country': 'USA', 'age': 22, 'name': 'zhaoliu'}
USA {'country': 'USA', 'age': 22, 'name': 'pengqi'}


print [key for key,group in lstg] 


print [(key,list(group)) for key,group in lstg]
```

返回的list中包含着三个元组：  
\[(‘China’, \[{‘country’: ‘China’, ‘age’: 20, ‘name’: ‘zhangsan’}, {‘country’: ‘China’, ‘age’: 22, ‘name’: ‘lijiu’}\]), (‘JP’, \[{‘country’: ‘JP’, ‘age’: 22, ‘name’: ‘lisi’}\]), (‘USA’, \[{‘country’: ‘USA’, ‘age’: 19, ‘name’: ‘wangwu’}, {‘country’: ‘USA’, ‘age’: 22, ‘name’: ‘zhaoliu’}, {‘country’: ‘USA’, ‘age’: 22, ‘name’: ‘pengqi’}\])\]

print dict(\[(key,list(group)) for key,group in lstg\])

```
返回每个分组的个数：
{'JP': 1, 'China': 2, 'USA': 3}

lstg = groupby(sorted(lst,key=itemgetter('country')),key=itemgetter('country')) 
lstgall=[(key,list(group)) for key,group in lstg ]
print dict(filter(lambda x:len(x[1])>2,lstgall)) 
```

过滤出分组后的元素个数大于2个的分组，返回：  
{‘USA’: \[{‘country’: ‘USA’, ‘age’: 19, ‘name’: ‘wangwu’}, {‘country’: ‘USA’, ‘age’: 22, ‘name’: ‘zhaoliu’}, {‘country’: ‘USA’, ‘age’: 22, ‘name’: ‘pengqi’}\]}

### 自定义分组

```
from itertools import groupby
lst=[2,8,11,25,43,6,9,29,51,66]

def gb(num):
    if num <= 10:
        return 'less'
    elif num >=30:
        return 'great'
    else:
        return 'middle'

print [(k,list(g))for k,g in groupby(sorted(lst),key=gb)]
```

返回：  
\[(‘less’, \[2, 6, 8, 9\]), (‘middle’, \[11, 25, 29\]), (‘great’, \[43, 51, 66\])\]

```
lst = [1,1,0,0,0,0,1,1,0,1]
print max(len(list(g)) for k, g in groupby(s) if k==1)
```
