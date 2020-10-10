### python脚本参数

```python
# 脚本获取参数
import sys, getopt
opts, args = getopt.getopt(sys.argv[1:], "hd:s:")
if len(opts)==0:
    # operations
    sys.exit()
else:
    for op, val in opts:
    	if op == "-d":
            # operations
        elif op == "-s":
            # operations
    	else :
            sys.exit()

# 另外一种方式
import argparse

FLAGS = None

def main(_):# 注意下划线
    first_arg = FLAGS.first_arg
    second_arg = FLAGS.secong_arg
    
if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('--first_arg', type=str, default='', help='Help information')
    parser.add_argument('--second_arg', type=int, default=3, help='Help information')
    FLAGS, unparsed = parser.parse_known_args()
    main()
```

---
### python多线程

```python
# 多线程
import threading
def threadsStart():
    global mutex,count
    mutex = threading.Lock()
    threads = []
    for i in a_list:
        threads.append(threading.Thread(target = method_name, args=(method_param1, method_param2,)))
    for t in threads:
        t.start()
    for t in threads:
        t.join()
# mutex.acquire()
# total = total + 1
# mutex.release()
if __name__=='__main__':
    threadsStart()
```
### 多进程

```python
import multiprocessing as mp

def job(i)
    # do something with i
    return res

pool = mp.Pool(processes=7) # 进程池，处理器数量
res = pool.map(job, range(10)) # map, 接收job方法和迭代器

# 相当于单进程的：
for i in range(10):
    # do something with i

```

---
### python中的时间

```python
# 时间相关操作
import time
# 获取当前日期并标准化
str(time.strftime('%Y-%m-%d',time.localtime(time.time())))
# 获取当前精确时间，精确到秒
time.ctime()
# pandas的时间移动
from pandas.tseries.offsets import *
datetime + DateOffset(years=y_delta, months=m_delta, days=d_delta, 
                        hours=h_delta, minutes=n_delta, seconds=s_delta)
```

---
### python脚本输出打印重定位

```python
# 怎么关掉打印？
# 将打印内容重定向到/dev/null中，相当于直接丢弃，任何地方都找不到打印的内容
python your_script.py >> /dev/null &
# 如果又要查看打印的内容，怎么让它输出(还在运行)？
python your_script.py >> log.txt &   #将输出重定向到当前目录下的log.txt文件中
cat log.txt  # 查看截止目前所有的日志内容
tail log.txt # 查看截止目前，最后10行日志
tail -f log.txt # 从最后10行开始滚动输出，准实时刷新
tail -n 100 -f log.txt  # 从最后100行开始滚动输出
```

---
### python的pickle数据存储

```python
# -*- coding: utf-8 -*-
import pickle
# 也可以这样：
# import cPickle as pickle
 
obj = {"a": 1, "b": 2, "c": 3}
 
# 将 obj 持久化保存到文件 tmp.txt 中
pickle.dump(obj, open("tmp.txt", "w"))

# 从 tmp.txt 中读取并恢复 obj 对象
obj2 = pickle.load(open("tmp.txt", "r"))
```

---
### python与json文件

```python
import json

alist = [1,2,3,4]
bdict = {'a':1,'b':2,'c':3}

# 使用dump可以保存至本地json文件
filename = 'a.json'
with open(filename, 'w') as f:
    json.dump(alist, f)

# 读取本地的json文件
with open(filename, 'r') as f:
    alist = json.load(f)

# 也可以用dumps得到json格式的数据，即将字典转换为字符串
bjson = json.dumps(bdict, ensure_ascii=False)
# ensure_ascii=False也能显示中文

# 相应的，也可以讲json字符串转换为字典
bdict = json.loads(bjson)

# 需要注意的是，如果出现类似如下错误
TypeError: *** is not JSON serializable
# 那么可能是数据的格式不对，可以使用int()/float()等强制转换一下
# 如：
def format(alist):
    for a in alist:
        if isinstance(a, np.floating):
            a = float(a)
        elif isinstance(a, np.int):
            a = int(a)
```

----

### autoreload

```python
%load_ext autoreload
%autoreload 2
```

---

### python logging

```python
import logging
LOG_FORMAT = "%(asctime)s - %(levelname)s - %(message)s"
logging.basicConfig(filename='expandLabels.log', level=logging.INFO, format=LOG_FORMAT)

logging.info("message")
```

#### 输出到文件

```shell
>> python sc.py 2>&1 | tee -a ./logfile.log
# -u 则即时写入文件，否则会等代buffer满才会写入文件
>> python -u sc.py 2>&1 | tee -a ./logfile.log
```

---

### python schedule

```python
import schedule
import time

def Schedule():
    do something

def main():
    schedule.every().minutes.do(Schedule)
    while True:
        schedule.run_pending()
        time.sleep(60)

```

### python 文件

```python
import os
path = "" #文件夹目录
files= os.listdir(path) #得到文件夹下的所有文件名称
os.path.isdir(file): #判断是否是文件夹
```

### python用字符串访问变量

```python
var = "This is a string"
>>> varName = 'var'
>>> s= locals()[varName] # 定义局部变量，定义全局变量则使用globals
>>> s
'This is a string'
>>> s2=vars()[varName]
>>> s2
'This is a string'
>>> s3=eval(varName)
>>> s3
'This is a string'

# 用字符串调用函数：getattr()方法
# getattr() 方法接受两个参数
# 第一个参数为待调用方法所属类或变量
# 第二个参数为待调用方法的字符串名称
date = pd.to_datetime('20180909')
# 获取date的day属性，相当于：date.day
getattr(date, 'day')
# 通过chinese_calendar调用is_holiday方法判断date是否是节假日
# 相当于：chinese_calendar.is_holiday(date)
getattr(chinese_calendar, 'is_holiday')(date)
```