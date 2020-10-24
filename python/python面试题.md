# 基础知识

https://www.toutiao.com/i6818132438506537479/

## 如何执行字符串插值？

在不导入模板类的情况下，有3种方法可以插入字符串。

```python
name = 'Chris'
## 1. f strings
print(f'Hello {name}')
## 2. % operator
print('Hey %s %s' % (name, name))
## 3. format
print(
 "My name is {}".format((name))
)
```



## 什么是装饰器（decorator）？

这是每次面试我都会被问到的另一个问题。

decorator允许通过将现有函数传递给decorator来向现有函数添加功能，decorator将执行现有函数和其他代码。

我们将编写一个decorator，它在调用另一个函数时记录日志。

编写decorator函数。它接受一个函数func作为参数。它还定义了一个名为log function的函数，它调用func（）并执行一些print（f'{func}called.'）的代码，然后返回它定义的函数。

```python
def logging(func):
  def log_function_called():
    print(f'{func} called.')
    func()
  return log_function_called
```

查看函数运行时间

```python
def run_time(fn):
    def wrapper(*args, **kwargs):
        start = time.time()
        fn(*args, **kwargs)
        print('运行时间{}'.format(time.time() - start))
    return wrapper
```



## python是通过引用调用还是通过值调用？

字符串、数字和元组等不可变对象是按值调用的。请注意，当在函数内部修改时，name的值在函数外部没有更改。name的值已分配给内存中该函数作用域的新块。

列表之类的可变对象是通过引用调用的。注意在函数外部定义的列表是如何在函数内部修改的。函数中的参数指向内存中存储li值的原始块。



## 什么是Pickling？

Pickling是Python中序列化和取消序列化对象的常用方法。

在下面的示例中，我们序列化和取消序列化字典列表。

```python
import pickle
obj = [
    {'id':1, 'name':'Stuffy'},
    {'id':2, 'name': 'Fluffy'}
]
with open('file.p', 'wb') as f:
    pickle.dump(obj, f)
with open('file.p', 'rb') as f:
    loaded_obj = pickle.load(f)
print(loaded_obj)
#=> [{'id': 1, 'name': 'Stuffy'}, {'id': 2, 'name': 'Fluffy'}]
```



## append 和 extend 有什么区别？

append 向列表中添加值，extend向列表中添加另一个列表中的值。

```python
a = [1,2,3]
b = [1,2,3]
a.append(6)
print(a)
#=> [1, 2, 3, 6]
b.extend([4,5])
print(b)
#=> [1, 2, 3, 4, 5]
```



## 检查字符串是否只包含数字或字母

使用isnumeric（）。

```python
'123a'.isnumeric()
#=> False
'123'.isnumeric()
#=> True
```

使用isalpha（）。

```python
'123a'.isalpha()
#=> False
'a'.isalpha()
#=> True
```

或者两个都可以，使用isalnum（）。

```python
'123abc...'.isalnum()
#=> False
'123abc'.isalnum()
#=> True
```



## remove, del 和 pop有什么区别？

remove（）删除第一个匹配值。

```python
li = ['a','b','c','d']
li.remove('b')
li
#=> ['a', 'c', 'd']
```

**del** 按索引删除元素。

```python
li = ['a','b','c','d']
del li[0]
li
#=> ['b', 'c', 'd']
```

**pop()** 按索引移除元素并返回该元素。

```python
li = ['a','b','c','d']
li.pop(2)
#=> 'c'
li
#=> ['a', 'b', 'd']
```



## *args 和**kwargs的用法

*args可以当作可容纳多个变量组成的list  

```pytho
def fun_var_args(farg, *args):  
	pass
fun_var_args(1, "two", 3)
```

**kwargs可以当作容纳多个key和value的dictionary  

```pyth
def fun_var_kwargs(farg, **kwargs):  
	pass
fun_var_kwargs(farg=1, myarg2="two", myarg3=3)
```





# 具体题目

## 单例模式的实现方式

方式一 使用装饰器

```python
def singleton(cls):
    instances = {}
    def wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return wrapper

@singleton
class foo(object):
    pass
foo1 = foo()
foo2 = foo()
print(foo1 is foo2)
```

方法二 使用基类 New 是真正创建实例对象的方法，所以重写基类的new 方法，以此保证创建对象的时候只生成一个实例

```python
class Singleton2(object):
    def __new__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls._instance = super(Singleton2, cls).__new__(cls, *args, **kwargs)
        return cls._instance
class foo(Singleton2):
    pass
```

方法三 元类，元类是用于创建类对象的类，类对象创建实例对象时一定要调用call方法

```python
class singleton3(type):
    def __call__(cls, *args, **kwargs):
        if not hasattr(cls, '_instance'):
            cls._instance = super(singleton3, cls).__call__(*args, **kwargs)
        return cls._instance
class foo(object):
    __metaclass__ = singleton3
```



