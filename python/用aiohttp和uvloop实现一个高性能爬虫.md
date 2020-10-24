## 总结

1. 和之前的`Python异步IO之协程`相似，但是在创建task时有所不同，一个是使用await asyncio.wait(tasks)，而另一个是使用asyncio.ensure_future(task)，个人还是比较喜欢第一种方式

2. https://docs.python.org/zh-cn/3/library/asyncio-task.html  根据官方文档，现在应该是使用**create_task**而不是低层级的ensure_future。注意创建方式为：老的方式asyncio.ensure_future(),新的方式loop.create_task()

3. 结合之前的`Python异步IO之协程`,把这三种方式写在一起：

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
       task= [loop.create_task(taskIO_1()),loop.create_task(taskIO_2())]
       # 执行协程
       loop.run_until_complete(asyncio.wait(task))
   
   def main_ensure_future(): # 调用方
       task= [asyncio.ensure_future(taskIO_1()),asyncio.ensure_future(taskIO_2())]
       # 获取事件循环 Eventloop
       loop = asyncio.get_event_loop()
       # 执行协程
       loop.run_until_complete(asyncio.wait(task))
   
   if __name__ == '__main__':
       start = time.time()
       main_create_task()
       # main_ensure_future()
       # loop = asyncio.get_event_loop() # 创建一个事件循环对象loop
       # try:
       #     loop.run_until_complete(main()) # 完成事件循环，直到最后一个任务结束
       # finally:
       #     loop.close() # 结束事件循环
       print('所有IO任务总耗时%.5f秒' % float(time.time()-start))
   ```

4. 使用Semaphore限制最大并发数，写发有点特殊async with sem:await fetch_image_by_id(current_id)

5. 使用uvloop加速，只需要在开始添加asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())即可,但是这个库好像在Windows上安装不了

## 原文

https://blog.csdn.net/xc_zhou/article/details/85167927

```python
# -*- coding:utf-8 -*-
import requests,asyncio,aiohttp,os,time


def run_time(fn):
    '''装饰器，用于查看图片下载运行时间'''
    def wrapper(*args, **kwargs):
        start = time.time()
        fn(*args, **kwargs)
        print('运行时间{}'.format(time.time() - start))

    return wrapper


class Crawl_Image:
    def __init__(self):
        self.num = 1
        if '图片' not in os.listdir('.'):
            os.mkdir('图片')
        self.path = os.path.join(os.path.abspath('.'),'图片')
        os.chdir(self.path)

    def str_dict(self):
        '''把字符串转化成字典，通常的请求头一个一个写成字典麻烦'''
        headers = {}
        heads = '''
        authority: unsplash.com
        method: GET
        path: /napi/photos?page=13&per_page=12&order_by=latest
        scheme: https
        accept: */*
        accept-encoding: gzip, deflate, br
        accept-language: zh-CN,zh;q=0.9
        cache-control: no-cache
        cookie: _ga=GA1.2.1867787748.1541257560; uuid=fbe18d60-df79-11e8-af00-9f82294d3e27; xpos=%7B%7D; lsnp=Wn1U1YnKyn4; ugid=45e258e4a27fdb75969611bfa7b6227a5146352; _gid=GA1.2.104984706.1543905667; lux_uid=154390566744470013; _sp_ses.0295=*; _gat=1; _sp_id.0295=4996ac29-9203-4213-a46d-24cf83d147ce.1541257566.9.1543907256.1541399729.fcdb26b7-0b36-4bab-8056-898cc972167e
        dpr: 1
        pragma: no-cache
        referer: https://unsplash.com/
        user-agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36
        viewport-width: 1350
        '''
        heads = heads.split('\n')
        for head in heads:
            head = head.strip()
            if head:
                head_key,head_value = head.split(':',1)
                headers[head_key] = head_value.strip()
        return headers

    def request_url(self,url):
        '''请求网页，获取图片的 url 。因为图片的请求不多，所以用同步就好'''
        r = requests.get(url,headers=self.str_dict())
        if r.status_code == 200:
            return r.json()

    async def get_image(self, url):
        '''异步请求库aiohttp 加快图片 url 的网页请求'''
        async with aiohttp.ClientSession() as session:
            response = await session.get(url)
            content = await response.read()
            return content

    async def download_image(self,image):
            html = await self.get_image(image[0])
            with open(image[1] + '.jpg','wb') as f:
                f.write(html)
            print('下载第{}张图片成功'.format(self.num))
            self.num += 1

    @run_time
    def run(self):
        for page in range(10):
            url = 'https://unsplash.com/napi/photos?page={}&per_page=12&order_by=latest'.format(page)
            links = self.request_url(url)
            task = [asyncio.ensure_future(self.download_image((link['links']['download'],link['id']))) for link in links]
            # 获取事件循环 Eventloop
            loop = asyncio.get_event_loop()
            # 执行协程
            loop.run_until_complete(asyncio.wait(task))

if __name__ == '__main__':
    crawl_image = Crawl_Image()
    crawl_image.run()
```

asyncio于Python3.4引入标准库，增加了对异步I/O的支持，asyncio基于事件循环，可以轻松实现异步I/O操作。接下来，我们用基于asyncio的库实现一个高性能爬虫。

### 准备工作

[Earth View from Google Earth](https://link.jianshu.com/?t=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fearth-view-from-google-ea%2Fbhloflhklmhfpedakmangadcdofhnnoh)是一款Chrome插件，会在打开新标签页时自动加载一张来自Google Earth的背景图片。


![image](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzEyNTA0NTA4LWE5NTAxNDI0Y2QyYWFjM2M?x-oss-process=image/format,png)

Earth View from Google Earth

使用Chrome开发者工具观察插件的网络请求，我们发现插件会请求一个地址如[https://www.gstatic.com/prettyearth/assets/data/v2/1234.json](https://link.jianshu.com/?t=https%3A%2F%2Fwww.gstatic.com%2Fprettyearth%2Fassets%2Fdata%2Fv2%2F1234.json)的JSON文件，文件中包含了经过Base64的图片内容，观察发现，图片的ID范围大致在1000-8000之间，我们的爬虫就要来爬取这些精美的背景图片。

### 实现主要逻辑

由于爬取目标是JSON文件，爬虫的主要逻辑就变成了**爬取JSON–>提取图片–>保存图片**。

requests是一个常用的http请求库，但是由于requests的请求都是同步的，我们使用[aiohttp](https://aiohttp.readthedocs.io/en/stable/)这个异步http请求库来代替。

```
import aiohttp,json,base64,os

async def fetch_image_by_id(item_id):
    url = f'https://www.gstatic.com/prettyearth/assets/data/v2/{item_id}.json'
    # 由于URL是https的，所以选择不验证SSL
    async with aiohttp.ClientSession(connector=aiohttp.TCPConnector(verify_ssl=False)) as session:
        async with session.get(url) as response: # 获取后需要将JSON字符串转为对象
            try:
                json_obj = json.loads(await response.text())
            except json.decoder.JSONDecodeError as e:
                print(f'Download failed - {item_id}.jpg')
                return
            # 获取JSON中的图片内容字段，经过Base64解码成二进制内容
            image_str = json_obj['dataUri'].replace('data:image/jpeg;base64,', '')
            image_data = base64.b64decode(image_str)
            save_folder = dir_path = os.path.dirname(os.path.realpath(__file__)) + '/google_earth/'
            with open(f'{save_folder}{item_id}.jpg', 'wb') as f:
                f.write(image_data)
            print(f'Download complete - {item_id}.jpg')
```

aiohttp基于asyncio，所以在调用时需要使用async/await语法糖，可以看到，由于aiohttp中提供了一个ClientSession上下文，代码中使用了async with的语法糖。

### 加入并行逻辑

上面的代码是抓取单张图片的逻辑，批量抓取图片，需要再嵌套一层方法：

```
import asyncio

async def fetch_all_images():
    # 使用Semaphore限制最大并发数
    sem = asyncio.Semaphore(10)
    ids = [id for id in range(1000, 8000)] 
    for current_id in ids: 
        async with sem:await fetch_image_by_id(current_id)
```

接下来，将这个方法加入到asyncio的事件循环中。

```
event_loop = asyncio.get_event_loop()
future = asyncio.ensure_future(fetch_all_images())
results = event_loop.run_until_complete(future)
```

### 使用uvloop加速

uvloop基于libuv，libuv是一个使用C语言实现的高性能异步I/O库，uvloop用来代替asyncio默认事件循环，可以进一步加快异步I/O操作的速度。
uvloop的使用非常简单，只要在获取事件循环前，调用如下方法，将asyncio的事件循环策略设置为uvloop的事件循环策略。

```
asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())
```

使用上面的代码，我们可以快速将大约1500张的图片爬取下来。
![image.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xMjUwNDUwOC02YzAzNmQ4NTE2N2JhYmIyLnBuZw?x-oss-process=image/format,png)

爬取下来的Google Earth图片

### 性能对比

为了验证aiohttp和uvloop的性能，笔者使用requests+concurrent库实现了一个多进程版的爬虫，分别爬取20个id，消耗的时间如图。

![image](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzEyNTA0NTA4LWIzYzc2ZGFhNmYzMjE3MDA?x-oss-process=image/format,png)

可以看到，耗时相差了大概7倍，aiohttp+uvloop的组合在爬虫这种I/O密集型的场景下，可以说具有压倒性优势。