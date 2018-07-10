---
title: 听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬
date: 2018-06-14 16:13:01
categories: "开发"
tags:
	- 网络爬虫
	- NoSQL
	- 编程语言
	- HTML
	- Python

---

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip]

代理池设计

 *  获取器：就是我们的爬虫接口，抓取免费ip，这里我们为了后面的可扩展性，需要支持自由添加爬虫进获取器；
 *  数据库：我们选择Mongodb存放有效的代理，上面文章写了关于Mongodb可扩展的封装，我们这里直接搬来使用；
 *  调度器：主要是用于检测爬虫是否有效，并添加有效代理入库，定制计划任务检测库中代理，控制爬虫的启动；
 *  Api：为了更方便的调用新的代理，我们使用flask做外部接口。

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 1]

获取器

我们打开百度输入“免费ip”就可以看到很多提供免费ip的网站，这里我们选择幻代理，66代理，快代理，西刺代理。

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 2]

直接上代码：

``````````
# coding=utf-8# __author__ = 'yk'import requestsfrom lxml import etreefrom requests.exceptions import ConnectionErrordef parse_url(url): headers = { 'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:54.0) Gecko/20100101 Firefox/54.0' } try: resp = requests.get(url, headers=headers) if resp.status_code == 200: return resp.text return None except ConnectionError: print('Error.') return Nonedef proxy_xici(): url = 'http://www.xicidaili.com/' resp = parse_url(url) html = etree.HTML(resp) ips = html.xpath('//*[@id="ip_list"]/tr/td[2]/text()') ports = html.xpath('//*[@id="ip_list"]/tr/td[3]/text()') for ip, port in zip(ips, ports): proxy = ip + ':' + port yield proxy
``````````

我们使用xpath解析出代理。

可扩展

我们需要抓取的网站有四个，未来肯定会更多，为了能够更方便的扩展，我们需要写一个元类。元类主要是控制代理获取类的实现，在获取类中加入两个属性。用于存放类里的每个网站爬虫方法，以及所有爬虫的数量。方便我们在调度器中调用：

``````````
# Spider/get_proxy.pyclass ProxyMetaclass(type): """ 元类，在ProxyGetter类中加入 __CrawlFunc__和__CrawlFuncCount__两个属性 分别表示爬虫函数和爬虫函数的数量 """ def __new__(cls, name, bases, attrs): count = 0 attrs['__CrawlFunc__'] = [] for k in attrs.keys(): if k.startswith('proxy_'): attrs['__CrawlFunc__'].append(k) count += 1 attrs['__CrawlFuncCount__'] = count return type.__new__(cls, name, bases, attrs)class ProxyGetter(object, metaclass=ProxyMetaclass): def proxy_ip66(self): pass def proxy_xici(self): pass def proxy_kuai(self): pass def proxy_ihuan(self): pass
``````````

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 3]

``````````
class ProxyGetter(object, metaclass=ProxyMetaclass): ... def get_raw_proxies(self, callback): proxies = [] for proxy in eval("self.{}()".format(callback)): print('Getting', proxy, 'from', callback) proxies.append(proxy) return proxies
``````````

python内置的eval函数作用不知道的可以自行查找，这里做一个简单解释：

``````````
>>> m = 5>>> n = 3>>> eval('m') + eval('n')8
``````````

数据库

数据库我们直接使用上篇文章封装的类，不过未免存入相同ip，我们需要判断去重。

``````````
# Db/db.py...def put(self, proxy): """ 放置代理到数据库 """ num = self.proxy_num() + 1 if self.db[self.table].find_one({'proxy': proxy}): self.delete(proxy) self.db[self.table].insert({'proxy': proxy, 'num': num}) else: self.db[self.table].insert({'proxy': proxy, 'num': num})
``````````

具体实现可以参考上篇文章。

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 4]

``````````
# Schedule/tester.pyimport asyncioimport aiohttpfrom Db.db import MongodbClient# 将配置信息放入配置文件config.pyfrom config import TEST_URLclass ProxyTester(object): test_url = TEST_URL def __init__(self): self._raw_proxies = None def set_raw_proxies(self, proxies): # 供外部添加需要测试的代理 self._raw_proxies = proxies self._conn = MongodbClient() async def test_single_proxy(self, proxy): """ 测试一个代理，如果有效，将他放入usable-proxies """ try: async with aiohttp.ClientSession() as session: try: if isinstance(proxy, bytes): proxy = proxy.decode('utf-8') real_proxy = 'http://' + proxy print('Testing', proxy) async with session.get(self.test_url, proxy=real_proxy, timeout=10) as response: if response.status == 200: # 请求成功，放入数据库 self._conn.put(proxy) print('Valid proxy', proxy) except Exception as e: print(e) except Exception as e: print(e) def test(self): """ 异步测试所有代理 """ print('Tester is working...') try: loop = asyncio.get_event_loop() tasks = [self.test_single_proxy(proxy) for proxy in self._raw_proxies] loop.run_until_complete(asyncio.wait(tasks)) except ValueError: print('Async Error')
``````````

关于aiohttp的用法，大家可以参考这篇文章。也可以直接参考中文文档

aiohttp大致的get请求方式如：

``````````
async with aiohttp.ClientSession() as session: async with session.get(url, headers=headers, proxy=proxy, timeout=1) as r: content, text = await r.read(), await r.text(encoding=None, errors='ignore')
``````````

test()方法是启动测试的方法，使用asyncio库。asyncio的编程模型就是一个消息循环。最后我们获取一个EventLoop的引用：loop = asyncio.get\_event\_loop()，然后迭代出所有需要测试的代理，放入测试协程方法，将协程放入EventLoop中去执行。

这样，一个异步测试的类就实现了。

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 5]

``````````
# Schedule/adder.pyfrom Db.db import MongodbClientfrom Spider.get_proxy import ProxyGetterfrom .tester import ProxyTesterclass PoolAdder(object): """ 启动爬虫，添加代理到数据库中 """ def __init__(self, threshold): self._threshold = threshold self._conn = MongodbClient() self._tester = ProxyTester() self._crawler = ProxyGetter() def is_over_threshold(self): """ 判断数据库中代理数量是否达到设定阈值 """ return True if self._conn.get_nums >= self._threshold else False def add_to_pool(self): """ 补充代理 """ print('PoolAdder is working...') proxy_count = 0 while not self.is_over_threshold(): # 迭代所有的爬虫，元类给ProxyGetter的两个方法 # __CrawlFuncCount__是爬虫数量，__CrawlFunc__是爬虫方法 for callback_label in range(self._crawler.__CrawlFuncCount__): callback = self._crawler.__CrawlFunc__[callback_label] # 调用ProxyGetter()方法进行抓取代理 raw_proxies = self._crawler.get_raw_proxies(callback) # 调用方法测试爬取到的代理 self._tester.set_raw_proxies(raw_proxies) self._tester.test() proxy_count += len(raw_proxies) if self.is_over_threshold(): print('Proxy is enough, waiting to be used...') break if proxy_count == 0: print('The proxy source is exhausted.')
``````````

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 6]

``````````
# Schedulr/schedule.pyimport timefrom multiprocessing import Processfrom ProxyPool.db import MongodbClientfrom .tester import ProxyTesterfrom .adder import PoolAdderfrom config import VALID_CHECK_CYCLE, POOL_LEN_CHECK_CYCLE  POOL_LOWER_THRESHOLD, POOL_UPPER_THRESHOLDclass Schedule(object): @staticmethod def valid_proxy(cycle=VALID_CHECK_CYCLE): """ 从数据库中拿到一半代理进行检查 """ conn = MongodbClient() tester = ProxyTester() while True: print('Refreshing ip...') # 调用数据库，从左边开始拿到一半代理 count = int(0.5 * conn.get_nums) if count == 0: print('Waiting for adding...') time.sleep(cycle) continue raw_proxies = conn.get(count) tester.set_raw_proxies(raw_proxies) tester.test() time.sleep(cycle) @staticmethod def check_pool(lower_threshold=POOL_LOWER_THRESHOLD, upper_threshold=POOL_UPPER_THRESHOLD, cycle=POOL_LEN_CHECK_CYCLE): """ 如果代理数量少于最低阈值，添加代理 """ conn = MongodbClient() adder = PoolAdder(upper_threshold) while True: if conn.get_nums < lower_threshold: adder.add_to_pool() time.sleep(cycle) def run(self): print('Ip Processing running...') valid_process = Process(target=Schedule.valid_proxy) check_process = Process(target=Schedule.check_pool) valid_process.start() check_process.start()
``````````

代码中的几个变量需要放置配置文件：

``````````
# config.py# Pool 的低阈值和高阈值POOL_LOWER_THRESHOLD = 10POOL_UPPER_THRESHOLD = 40# 两个调度进程的周期VALID_CHECK_CYCLE = 600POOL_LEN_CHECK_CYCLE = 20
``````````

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 7]

``````````
# run.pyfrom Api.api import appfrom Schedule.schedule import Scheduledef main(): # 任务类的两个周期进程就是整个调度器 s = Schedule() s.run() app.run()if __name__ == '__main__': main()
``````````

项目地址

github

可以直接下载到本地，切换到项目目录。

安装依赖：pip install -r requirements.txt

启动：python run.py

示例

上面我们已经实现了我们的代理池，下面只是做一个基本的爬虫调用。

首先我们启动代理池：

``````````
>>> python run.py
``````````

如果没有报错则，并且显示的信息正常，说明代理池已经启动了。这是打开浏览器访问http://127.0.0.1:5000/get就可以看到一条代理了。

![听说你玩爬虫爬妹子时老被封ip？教你搭建一个ip代理池！想爬就爬][ip_ip 8]

最爬虫中调用：

``````````
import requestsdef get_proxy(): resp = requests.get('http://127.0.0.1:5000/get') proxy = resp.text ip = 'http://' + proxy return ipdef get_resp(url): ip = get_proxy() proxy = {'http': 'http://{}'.format(ip)} resp = requests.get(url, proxies=proxy) if resp.status_code == 200: print('success')
``````````

**私信小编01即可获取源码！**


[ip_ip]: /pro/os/crawler/VIQR-FIJM-IIZJ.jpg
[ip_ip 1]: /pro/os/crawler/A3I3-YAAF-NFQV.jpg
[ip_ip 2]: /pro/os/crawler/NAFI-R2M3-IFIZ.jpg
[ip_ip 3]: /pro/os/crawler/JYRI-6JAA-MVEU.jpg
[ip_ip 4]: /pro/os/crawler/YB7R-EMBN-YQ7V.jpg
[ip_ip 5]: /pro/os/crawler/ZEV6-BEIR-Q73Q.jpg
[ip_ip 6]: /pro/os/crawler/E26J-BFUJ-V2QA.jpg
[ip_ip 7]: /pro/os/crawler/3ARZ-R2AJ-QY7R.jpg
[ip_ip 8]: /pro/os/crawler/IQZA-QJMA-UFVE.jpg
 *  **原文作者：** 繁华落尽and曲终人散
 *  **原文链接：** https://www.toutiao.com/item/6566850298780844552/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。