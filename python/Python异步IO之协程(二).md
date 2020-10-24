# 总结

1. 个人感觉还是as_completed和gather比较实用
2. 只有当给`wait()`传入`timeout`参数时才有可能产生`pending`列表。
3. `wait()`返回的**结果集**是**按照**事件循环中的任务**完成顺序**排列的，所以其往往**和原始任务顺序不同**。
4. 使用`as_completed(tasks)`和`wait(tasks)`**相同之处**是返回结果的顺序是**协程的完成顺序**，这与gather()恰好相反。而**不同之处**是`as_completed(tasks)`可以**实时返回**当前完成的结果，而`wait(tasks)`需要等待所有协程结束后返回的`done`去获得结果。



# 原文

https://blog.csdn.net/SL_World/article/details/86691747

在多个协程中的**线性控制流**很容易**通过**内置的**关键词`await`来管理**。使用`asyncio`模块中的方法可以实现更多复杂的结构，它可以**并发地**完成多个协程。

## 一、asyncio.wait()

你可以将一个操作分成多个部分并分开执行，而`wait(tasks)`可以被用于**中断**任务集合(tasks)中的某个**被事件循环轮询**到的**任务**，直到该协程的其他后台操作完成才**被唤醒**。

```js
import time
import asyncio
async def taskIO_1():
    print('开始运行IO任务1...')
    await asyncio.sleep(3)  # 假设该任务耗时3s
    print('IO任务1已完成，耗时3s')
    return taskIO_1.__name__
async def taskIO_2():
    print('开始运行IO任务2...')
    await asyncio.sleep(2)  # 假设该任务耗时2s
    print('IO任务2已完成，耗时2s')
    return taskIO_2.__name__
async def main(): # 调用方
    tasks = [taskIO_1(), taskIO_2()]  # 把所有任务添加到task中
    done,pending = await asyncio.wait(tasks) # 子生成器
    for r in done: # done和pending都是一个任务，所以返回结果需要逐个调用result()
        print('协程无序返回值：'+r.result())

if __name__ == '__main__':
    start = time.time()
    loop = asyncio.get_event_loop() # 创建一个事件循环对象loop
    try:
        loop.run_until_complete(main()) # 完成事件循环，直到最后一个任务结束
    finally:
        loop.close() # 结束事件循环
    print('所有IO任务总耗时%.5f秒' % float(time.time()-start))
1234567891011121314151617181920212223242526
```

执行结果如下：

```js
开始运行IO任务1...
开始运行IO任务2...
IO任务2已完成，耗时2s
IO任务1已完成，耗时3s
协程无序返回值：taskIO_2
协程无序返回值：taskIO_1
所有IO任务总耗时3.00209秒
1234567
```

【解释】：wait()[官方文档](https://docs.python.org/zh-cn/3/library/asyncio-task.html)用法如下：

```js
done, pending = await asyncio.wait(aws)
1
```

此处并发运行传入的`aws`(awaitable objects)，同时通过`await`返回一个包含(done, pending)的元组，**done**表示**已完成**的任务列表，**pending**表示**未完成**的任务列表。
**注：**
①只有当给`wait()`传入`timeout`参数时才有可能产生`pending`列表。
②通过`wait()`返回的**结果集**是**按照**事件循环中的任务**完成顺序**排列的，所以其往往**和原始任务顺序不同**。

## 二、asyncio.gather()

如果你只关心协程并发运行后的结果集合，可以使用`gather()`，它不仅通过`await`返回仅一个结果集，而且这个结果集的**结果顺序**是传入任务的**原始顺序**。

```js
import time
import asyncio
async def taskIO_1():
    print('开始运行IO任务1...')
    await asyncio.sleep(3)  # 假设该任务耗时3s
    print('IO任务1已完成，耗时3s')
    return taskIO_1.__name__
async def taskIO_2():
    print('开始运行IO任务2...')
    await asyncio.sleep(2)  # 假设该任务耗时2s
    print('IO任务2已完成，耗时2s')
    return taskIO_2.__name__
async def main(): # 调用方
    resualts = await asyncio.gather(taskIO_1(), taskIO_2()) # 子生成器
    print(resualts)

if __name__ == '__main__':
    start = time.time()
    loop = asyncio.get_event_loop() # 创建一个事件循环对象loop
    try:
        loop.run_until_complete(main()) # 完成事件循环，直到最后一个任务结束
    finally:
        loop.close() # 结束事件循环
    print('所有IO任务总耗时%.5f秒' % float(time.time()-start))
123456789101112131415161718192021222324
```

执行结果如下：

```js
开始运行IO任务2...
开始运行IO任务1...
IO任务2已完成，耗时2s
IO任务1已完成，耗时3s
['taskIO_1', 'taskIO_2']
所有IO任务总耗时3.00184秒
123456
```

【解释】：`gather()`通过`await`直接**返回**一个结果集**列表**，我们可以清晰的从执行结果看出来，虽然任务2是先完成的，但最后返回的**结果集的顺序是按照初始传入的任务顺序排的**。

## 三、asyncio.as_completed()

`as_completed(tasks)`是一个生成器，它管理着一个**协程列表**(此处是传入的tasks)的运行。当任务集合中的某个任务率先执行完毕时，会率先通过`await`关键字返回该任务结果。可见其返回结果的顺序和`wait()`一样，均是按照**完成任务顺序**排列的。

```js
import time
import asyncio
async def taskIO_1():
    print('开始运行IO任务1...')
    await asyncio.sleep(3)  # 假设该任务耗时3s
    print('IO任务1已完成，耗时3s')
    return taskIO_1.__name__
async def taskIO_2():
    print('开始运行IO任务2...')
    await asyncio.sleep(2)  # 假设该任务耗时2s
    print('IO任务2已完成，耗时2s')
    return taskIO_2.__name__
async def main(): # 调用方
    tasks = [taskIO_1(), taskIO_2()]  # 把所有任务添加到task中
    for completed_task in asyncio.as_completed(tasks):
        resualt = await completed_task # 子生成器
        print('协程无序返回值：'+resualt)

if __name__ == '__main__':
    start = time.time()
    loop = asyncio.get_event_loop() # 创建一个事件循环对象loop
    try:
        loop.run_until_complete(main()) # 完成事件循环，直到最后一个任务结束
    finally:
        loop.close() # 结束事件循环
    print('所有IO任务总耗时%.5f秒' % float(time.time()-start))
1234567891011121314151617181920212223242526
```

执行结果如下：

```js
开始运行IO任务2...
开始运行IO任务1...
IO任务2已完成，耗时2s
协程无序返回值：taskIO_2
IO任务1已完成，耗时3s
协程无序返回值：taskIO_1
所有IO任务总耗时3.00300秒
1234567
```

【解释】：从上面的程序可以看出，使用`as_completed(tasks)`和`wait(tasks)`**相同之处**是返回结果的顺序是**协程的完成顺序**，这与gather()恰好相反。而**不同之处**是`as_completed(tasks)`可以**实时返回**当前完成的结果，而`wait(tasks)`需要等待所有协程结束后返回的`done`去获得结果。

## 四、总结

以下`aws`指：`awaitable objects`。即**可等待对象集合**，如一个协程是一个可等待对象，一个装有多个协程的**列表**是一个`aws`。

| asyncio        | 主要传参 | 返回值顺序   | `await`返回值类型                   | 函数返回值类型 |
| -------------- | -------- | ------------ | ----------------------------------- | -------------- |
| wait()         | aws      | 协程完成顺序 | (done,pending) 装有两个任务列表元组 | coroutine      |
| as_completed() | aws      | 协程完成顺序 | 原始返回值                          | 迭代器         |
| gather()       | *aws     | 传参任务顺序 | 返回值列表                          | awaitable      |

【参考文献】：

[[1\] Composing Coroutines with Control Structures](https://pymotw.com/3/asyncio/control.html)
[[2\] Python 3.7.2文档.协程与任务](https://docs.python.org/zh-cn/3/library/asyncio-task.html)
[[3\] 控制组合式 Coroutines](https://mozillazg.com/2017/08/python-asyncio-note-control-coroutines)