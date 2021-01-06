

# 廖雪峰Python教程

## 类和实例

**私有变量**：`__name`， 无法直接从外部访问实例变量.`__name`，但是实际上Python解释器对外
把`__name`变量改成了`_Student__name`，所以，仍然可以通过`_Student__name`来访问__name变量：

**特殊变量**：`__xxx__`,也就是以双下划线开头，并且以双下划线结尾的，是特殊变量

### 创建类的长度属性

```python
class MyDog(object):
def __len__(self):
return 100
```

len('ABC')   等价于  'ABC'.__len__()

### 创建类的静态变量

```python
class Student(object):
	name = 'Student'
print(Student.name)
s = Student()
print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
s.name = 'Michael'
print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
```

但是创建的实例可以覆盖掉这个属性

### 限制绑定属性

```python
class Student(object):
	__slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
s = Student() # 创建新的实例
s.name = 'Michael' # 绑定属性'name'
s.age = 25 # 绑定属性'age'
s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
	File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```

### 类的特殊属性

```python
def __str__(self):
	return 'Student object (name: %s)' % self.name
```

`__str__`()，`__repr__`()两者的区别是`__str__`()返回用户看到的字符串，
而`__repr__`()返回程序开发者看到的字符串，也就是说，`__repr__`()是为调试服务的。
`__repr__` = `__str__`
`__iter__`循环打印，该方法返回一个迭代对
象，然后，Python的for循环就会不断调用该迭代对象的`__next__`()方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环。



## 枚举类Enum

```python
from enum import Enum
Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```

使用方式为：Month.Jan，想要遍历使用：for name, member in Month.__members__.items():

```python
from enum import Enum, unique
@unique
class Weekday(Enum):
	Sun = 0 # Sun的value被设定为0
	Mon = 1
	Tue = 2
	Wed = 3
	Thu = 4
	Fri = 5
	Sat = 6
```

使用方法为Weekday.Tue.value,Weekday['Tue'].value或者Weekday[2].value



## 使用type动态创建一个类

```python
def fn(self, name='world'): # 先定义函数
	print('Hello, %s.' % name)
Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
```

要创建一个class对象，type()函数依次传入3个参数：

1. class的名称；
2. 继承的父类集合，注意Python支持多重继承，如果只有一个父类，别忘了tuple的单元素写法；
3. class的方法名称与函数绑定，这里我们把函数fn绑定到方法名hello上。



## 判断函数类型

\>>> type(fn)==types.FunctionType

\>>> type(abs)==types.BuiltinFunctionType

\>>> type(lambda x: x)==types.LambdaType

\>>> type((x for x in range(10)))==types.GeneratorType

### 判断类型是否多个某种类型中一个

isinstance([1, 2, 3], (list, tuple))



## 序列化保存

```python
import pickle
d = dict(name='Bob', age=20, score=88)
f = open('dump.txt', 'wb')
pickle.dump(d, f)
f.close()
# 读取
f = open('dump.txt', 'rb')
d = pickle.load(f)
f.close()
```



## 类转换为json

创建转换方法

```python
def student2dict(std):
    return {
    'name': std.name,
    'age': std.age,
    'score': std.score
    }
```

或者通用转换方法

print(json.dumps(s, default=lambda obj: obj.__dict__))



## 进程的使用

### 进程 Process

```python
from multiprocessing import Process
p = Process(target=run_proc, args=('test',))
p.start()
p.join()
```

### 大量子进程  Pool

```python
from multiprocessing import Pool
p = Pool(4)
for i in range(5):
	p.apply_async(long_time_task, args=(i,))
p.close()
p.join()
```

### 进程间通信Process, Queue

```python
q = Queue()
pw = Process(target=write, args=(q,))
pr = Process(target=read, args=(q,))
# 启动子进程pw，写入:
pw.start()
# 启动子进程pr，读取:
pr.start()
# 等待pw结束:
pw.join()
# pr进程里是死循环，无法等待其结束，只能强行终止:
pr.terminate()
```



## 进程的使用

进程 Process

```python
from multiprocessing import Process
p = Process(target=run_proc, args=('test',))
p.start()
p.join()
```

大量子进程  Pool

```python
from multiprocessing import Pool
p = Pool(4)
for i in range(5):
p.apply_async(long_time_task, args=(i,))
p.close()
p.join()
```

进程间通信（一个写入，一个读出）Process, Queue

```python
q = Queue()
pw = Process(target=write, args=(q,))
pr = Process(target=read, args=(q,))
# 启动子进程pw，写入:
pw.start()
# 启动子进程pr，读取:
pr.start()
# 等待pw结束:
pw.join()
# pr进程里是死循环，无法等待其结束，只能强行终止:
pr.terminate()
```



## 线程的使用

基础的使用 threading

```python
import time, threading
t1 = threading.Thread(target=run_thread, args=(5,))
t1.start()
t1.join()
```

加锁

```python
lock = threading.Lock()
lock.acquire()
try:
    # 放心地改吧:
    change_it(n)
finally:
    # 改完了一定要释放锁:
    lock.release()
```

线程中如果有用到多个函数，想要使用全局变量，可以使用**ThreadLocal**（threading.local()），我暂时想不到有什么地方可以用上



## 正则表达式

匹配的两种方式

- 使用正则的模块直接匹配，re.search(pa,s)

- 使用预编译再匹配

  ```python
  re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')
  re_telephone.match('010-12345').groups()
  ```

  

匹配多个使用finditer，而不是findall，findall返回的是字符的列，如果有小括号只返回括号中匹配的内容，而使用finditer则是返回正则类的列表，同样使用group获取结果

匹配中文:**[\u4e00-\u9fa5]**



## datetime

当前时间：datetime.datetime.now()

格式化时间：datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')

字符转时间：datetime.datetime.**strptime**(string,'%Y-%m-%d %H:%M:%S')

时间的加减

```python
from datetime import datetime, timedelta
now = datetime.now()
now + timedelta(hours=10)
```



## 集和collections

namedtuple创建不变的集和类似枚举类

```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p.x)
```

枚举类的实现方式

```python
# 导入枚举类
from enum import Enum
# 继承枚举类
class color(Enum):
    YELLOW  = 1
    BEOWN   = 1 
```

没有看出有多大的区别



deque创建可以方便得在前面插入和删除的list

```python
from collections import deque
q = deque(['a', 'b', 'c'])
q.append('x')
q.appendleft('y')
```

Counter 统计字符出现的个数

```python
from collections import Counter
c = Counter()
for ch in 'programming':
	c[ch] = c[ch] + 1
```



## base64

因为二进制文件包含很多无法显示和打印的字符，如果要让记事本这样的文本处理软件能处理二进制数据，就需要一个二进制到字符串的转换方法。

Base64编码会把3字节的二进制数据编码为4字节的文本数据，长度增加33%，好处是编码后的文本数据可以在邮件正文、网页等直接显示。

如果要编码的二进制数据不是3的倍数，最后会剩下1个或2个字节怎么办？Base64用\x00字节在末尾补足后，再在编码的末尾加上1个或2个=号，表示补了多少字节，解码的时候，会自动去掉。

```python
import base64
>>> base64.b64encode(b'binary\x00string')
b'YmluYXJ5AHN0cmluZw=='
>>> base64.b64decode(b'YmluYXJ5AHN0cmluZw==')
b'binary\x00string'
```

由于标准的Base64编码后可能出现字符+和/，在URL中就不能直接作为参数，所以又有一种"url safe"的base64编码，其实就是把字符+和/分别变成-和_：

```python
>>> base64.b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd++//'
>>> base64.urlsafe_b64encode(b'i\xb7\x1d\xfb\xef\xff')
b'abcd--__'
>>> base64.urlsafe_b64decode('abcd--__')
b'i\xb7\x1d\xfb\xef\xff'
```



## hashlib

常用的加密算法，也叫做摘要算法

md5

```python
import hashlib
md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
```

SHA1

```python
sha1 = hashlib.sha1()
sha1.update('how to use sha1 in '.encode('utf-8'))
sha1.update('python hashlib?'.encode('utf-8'))
print(sha1.hexdigest())
```

对简单口令加强保护,通过对原始口令加一个复杂字符串来实现，俗称“加盐”：

```python
def calc_md5(password):
	return get_md5(password + 'the-Salt')
```

hmac   标准的加盐算法

```python
>>> import hmac
>>> message = b'Hello, world!'
>>> key = b'secret'
>>> h = hmac.new(key, message, digestmod='MD5')
>>> # 如果消息很长，可以多次调用h.update(msg)
>>> h.hexdigest()
'fa4ee7d173f2d97ee79022d1a7355bcf'
```



## itertools

count无限递增

```python
>>> import itertools
>>> natuals = itertools.count(1)
>>> for n in natuals:
... 	print(n)
```

cycle循环

```python
cs = itertools.cycle('ABC') # 注意字符串也是序列的一种
>>> for c in cs:
... 	print(c)
```



## contextlib

@contextmanager这个decorator接受一个generator，用yield语句把with ... as var把变量输出出去，然后，with语句就可以正常地工作了：

```python
def tag(name):
    print("<%s>" % name)
    yield
    print("</%s>" % name)
with tag("h1"):
    print("hello")
    print("world")
```

可以用在计算方法使用了多少的时间上

1. with语句首先执行yield之前的语句，因此打印出<h1>；

2. yield调用会执行with语句内部的所有语句，因此打印出hello和world；
3. 最后执行yield之后的语句，打印出</h1>。



## chardet

判断二进制的编码格式

```python
>>> data = '离离原上草，一岁一枯荣'.encode('gbk')
>>> chardet.detect(data)
{'encoding': 'GB2312', 'confidence': 0.7407407407407407, 'language': 'Chinese'}
```

confidence字段，表示检测的概率



## psutil

获取系统信息，如cpu使用情况，内存等，运维使用较多

获取当前所有进程名字和进程号

```python
for i in psutil.pids():
    p = psutil.Process(i)
    print(str(i)+"  "+ p.name())
```

杀死进程在linux和window上不一样

```python
p.terminate()  linux
os.kill(2980,-1)  windows
```

