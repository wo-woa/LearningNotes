# 总结

1. 这篇文章的创建方式又和之前写的有一定的不同，结合之前的3种，把这四种方式写在一起：

   ```python
   import asyncio
   
   async def taskIO_1():
       print('开始运行IO任务1...')
       await asyncio.sleep(2)  # 假设该任务耗时2s
       print('IO任务1已完成，耗时2s')
       return taskIO_1.__name__
   async def taskIO_2():
       print('开始运行IO任务2...')
       await asyncio.sleep(3)  # 假设该任务耗时3s
       print('IO任务2已完成，耗时3s')
       return taskIO_2.__name__
   async def main(): # 调用方
       tasks = [taskIO_1(), taskIO_2()]  # 把所有任务添加到task中
       done,pending = await asyncio.wait(tasks) # 子生成器
       for r in done: # done和pending都是一个任务，所以返回结果需要逐个调用result()
           print('协程无序返回值：'+r.result())
   
   def main_create_task(): # 调用方
       # 获取事件循环 Eventloop
       loop = asyncio.get_event_loop()
       #创建任务
       task= [asyncio.create_task(taskIO_1()),asyncio.create_task(taskIO_2())]
       # 执行协程
       loop.run_until_complete(asyncio.wait(task))
   
   async def main_asyncio_create_task(): # 调用方
       #创建任务
       tasks= [asyncio.create_task(taskIO_1()),asyncio.create_task(taskIO_2())]
       # 执行协程
       await asyncio.gather(*tasks)
   
   def main_ensure_future(): # 调用方
       task= [asyncio.ensure_future(taskIO_1()),asyncio.ensure_future(taskIO_2())]
       # 获取事件循环 Eventloop
       loop = asyncio.get_event_loop()
       # 执行协程
       loop.run_until_complete(asyncio.wait(task))
   
   if __name__ == '__main__':
       start = time.time()
       asyncio.run(main_asyncio_create_task())
       # main_create_task()
       # main_ensure_future()
       # loop = asyncio.get_event_loop() # 创建一个事件循环对象loop
       # try:
       #     loop.run_until_complete(main()) # 完成事件循环，直到最后一个任务结束
       # finally:
       #     loop.close() # 结束事件循环
       print('所有IO任务总耗时%.5f秒' % float(time.time()-start))
   ```

2. 使用uvloop加速，在`用aiohttp和uvloop实现一个高性能爬虫`种是在开始添加asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())即可。而结合https://www.cnpython.com/pypi/uvloop，发现还有一种更加简便的方式调用，就是uvloop.install()，其他都不用管了

3. 参考https://www.cnblogs.com/lovesoo/p/7741576.html，使用concurrent 代替mutiprocessing和threading，Python3.2中引入的concurrent其实就是这两个的封装，但使用起来更加人性化，修改之前的生成封面的代码

   ```python
   with futures.ThreadPoolExecutor(max_workers=4) as executor:
       for file in self.folder_list:
       	executor.map(self.copy_compress,(file,))
   # for file in self.folder_list:
   #     # self.copy_compress(i)
   #     # p.apply(self.copy_compress, args=(file,))
   #     # p.apply_async(self.copy_compress, args=(self,file,))
   #     p = threading.Thread(target=self.copy_compress, args=(file,))
   #     p.start()
   # p.join()
   ```

   



# 原文

https://www.toutiao.com/i6762712823479927299/

IO 密集型应用IO 密集型应用CPU等待IO时间远大于CPU 自身运行时间，太浪费；常见的 IO 密集型业务包括：浏览器交互、磁盘请求、网络爬虫、数据库请求等

![Python 异步 IO(http://p1.pstatp.com/large/pgc-image/44c5b1d092a241e2a59d471512ee609f)、多进程、多线程性能对比](http://p1.pstatp.com/large/pgc-image/44c5b1d092a241e2a59d471512ee609f)



Python 世界对于 IO 密集型场景的并发提升有 3 种方法：多进程、多线程、异步 IO(asyncio);理论上讲asyncio是性能最高的，原因如下：

1.进程、线程会有CPU上下文切换

2.进程、线程需要内核态和用户态的交互，性能开销大；而协程对内核透明的,只在用户态运行

3.进程、线程并不可以无限创建，最佳实践一般是 CPU*2；而协程并发能力强，并发上限理论上取决于操作系统IO多路复用(Linux下是 epoll)可注册的文件描述符的极限

那asyncio的实际表现是否如理论上那么强，到底强多少呢？我构建了如下测试场景：

```python
访问500台 DB，并sleep 100ms模拟业务查询 
方法 1；顺序串行一台台执行
方法 2：多进程
方法 3：多线程
方法 4：asyncio
方法 5：asyncio+uvloop
```

最后的 asyncio+uvloop 和官方asyncio 最大不同是用 Cython+libuv 重新实现了asyncio 的事件循环(event loop)部分,官方测试性能是 node.js的 2 倍，持平 golang。

以下测试代码需要 Pyhton3.7+： 顺序串行一台台执行

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import records

user=xx
pass=xx
port=xx
hosts= [....] # 500台 db列表

def query(host):
 conn = records.Database(
 f'mysql+pymysql://{user}:{pass}@{host}:{port}/mysql?charset=utf8mb4')
 rows = conn.query('select sleep(0.1);')
 print(rows[0])

def main():
 for h in hosts:
 query(h)

# main entrance
if __name__ == '__main__':
 main()
```

多进程

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
from concurrent import futures
import records

user=xx
pass=xx
port=xx
hosts= [....] # 500台 db列表

def query(host):
 conn = records.Database(
 f'mysql+pymysql://{user}:{pass}@{host}:{port}/mysql?charset=utf8mb4')
 rows = conn.query('select sleep(0.1);')
 print(rows[0])

def main():
 with futures.ProcessPoolExecutor() as executor:
 for future in executor.map(query,hosts):
 pass

# main entrance
if __name__ == '__main__':
 main()
```

多线程

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
from concurrent import futures
import records

user=xx
pass=xx
port=xx
hosts= [....] # 500台 db列表

def query(host):
 conn = records.Database(
 f'mysql+pymysql://{user}:{pass}@{host}:{port}/mysql?charset=utf8mb4')
 rows = conn.query('select sleep(0.1);')
 print(rows[0])

def main():
 with futures.ThreadPoolExecutor() as executor:
 for future in executor.map(query,hosts):
 pass

# main entrance
if __name__ == '__main__':
 main()
```

asyncio

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import asyncio
from databases import Database 

user=xx
pass=xx
port=xx
hosts= [....] # 500台 db列表

async def query(host):
 DATABASE_URL = f'mysql+pymysql://{user}:{pass}@{host}:{port}/mysql?charset=utf8mb4'
 async with Database(DATABASE_URL) as database:
 query = 'select sleep(0.1);'
 rows = await database.fetch_all(query=query)
 print(rows[0])

async def main():
 tasks = [asyncio.create_task(query(host)) for host in hosts]
 await asyncio.gather(*tasks)

# main entrance
if __name__ == '__main__':
 asyncio.run(main())
```

asyncio+uvloop

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import asyncio
import uvloop
from databases import Database 

user=xx
pass=xx
port=xx
hosts= [....] # 500台 db列表

async def query(host):
 DATABASE_URL = f'mysql+pymysql://{user}:{pass}@{host}:{port}/mysql?charset=utf8mb4'
 async with Database(DATABASE_URL) as database:
 query = 'select sleep(0.1);'
 rows = await database.fetch_all(query=query)
 print(rows[0])

async def main():
 tasks = [asyncio.create_task(query(host)) for host in hosts]
 await asyncio.gather(*tasks)

# main entrance
if __name__ == '__main__':
 uvloop.install()
 asyncio.run(main())
```

**运行时间对比**

**方式运行时间**串行1m7.745s多进程2.932s多线程4.813sasyncio1.068sasyncio+uvloop0.750s

可以看出: 无论多进程、多进程还是asyncio都能大幅提升IO 密集型场景下的并发，但asyncio+uvloop性能最高，运行时间只有原始串行运行时间的 1/90，相差快 2 个数量级了！

内存占用对比 串行

![Python 异步 IO(http://p1.pstatp.com/large/pgc-image/be61852cd52a46999bc6b850ff6c5191)、多进程、多线程性能对比](http://p1.pstatp.com/large/pgc-image/be61852cd52a46999bc6b850ff6c5191)



多进程

![Python 异步 IO(http://p9.pstatp.com/large/pgc-image/aba795995e2b4048af2c43625bcd656a)、多进程、多线程性能对比](http://p9.pstatp.com/large/pgc-image/aba795995e2b4048af2c43625bcd656a)



多线程

![Python 异步 IO(http://p1.pstatp.com/large/pgc-image/0ccf0f0d926f49a3b98793496d0f13f4)、多进程、多线程性能对比](http://p1.pstatp.com/large/pgc-image/0ccf0f0d926f49a3b98793496d0f13f4)



asyncio

![Python 异步 IO(http://p1.pstatp.com/large/pgc-image/0ce254e0d00d4fab8d2605e41aac0365)、多进程、多线程性能对比](http://p1.pstatp.com/large/pgc-image/0ce254e0d00d4fab8d2605e41aac0365)



asyncio+uvloop

![Python 异步 IO(http://p1.pstatp.com/large/pgc-image/f6a04347bf674e0c8d0a4fe5d593b56e)、多进程、多线程性能对比](http://p1.pstatp.com/large/pgc-image/f6a04347bf674e0c8d0a4fe5d593b56e)



可以看出 asyncio+uvloop 内存占用表现仍然最优,只有 60M;而多进程占用多达 1.4G,果然创建进程是个十分重的操作~

总结asyncio 无论运行时间还是内存占用都远优于多进程、多线程，配合 uvloop 性能还能进一步提升，在 IO 密集型业务中可以优先使用 asyncio。