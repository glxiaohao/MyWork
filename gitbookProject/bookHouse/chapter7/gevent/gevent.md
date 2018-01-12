# 10.4 gevent

## 介绍

这教程的结构假设一个中等层次的Python程序员且没有什么并发性的知识．目的是给你知道怎么使用gevent,帮助你解决并发性的问题，和开始编写异步的应用程序．

## 贡献者
按贡献的时间顺序: 
* [Stephen Diehl](http://www.stephendiehl.com/)
* [Jérémy Bethmont](https://github.com/jerem)
* [sww](https://github.com/sww)
* [Bruno Bigras](https://github.com/brunoqc)
* [David Ripton](https://github.com/dripton)
* [Travis Cline](https://github.com/traviscline)
* [Boris Feld](https://github.com/Lothiraldan)
* [youngsterxyf](https://github.com/youngsterxyf)
* [Eddie Hebert](https://github.com/Lothiraldan)
* [Alexis Metaireau](http://notmyidea.org/)

这是根据MIT许可证发布的协作文档。 有东西要添加？ 看到一个错字？ Fork一个问题分支到github,任何和所有的贡献是受欢迎的。

## 核心
在gevent中主要使用Greenlet,给Python提供一个轻量级的协同程序,作为一个C的扩展模块.Greenlets主程序运行的所有系统进程是合理安排的. 这不同于任何multiprocessing或者multithreading提供的库和POSIX线程，这是真正的并行多处理器或多线程库提供真正的并行结构

## 同步&异步执行
并发的核心思想是一个更大的任务可以分解成多个子任务，其运行不依赖于其他任务的集合，因此可以异步运行 ，而不是一个在时间 同步。两个执行程序间的转换是一个关联转换。
在gevent中一个关联转换可以通过 ```yielding``` 来实现.在这个例子,两个程序的转换是通过调用 ```gevent.sleep(0)```.

```python
import gevent

def foo():
    print('Running in foo')
    gevent.sleep(0)
    print('Explicit context switch to foo again')

def bar():
    print('Explicit context to bar')
    gevent.sleep(0)
    print('Implicit context switch back to bar')

gevent.joinall([
    gevent.spawn(foo),
    gevent.spawn(bar),
])
```
```
Running in foo
Explicit context to bar
Explicit context switch to foo again
Implicit context switch back to bar
```

在调解器里面清楚地看到程序在两个转换之间是怎么运行的.
当我们将它用于可以合作调度的网络和IO绑定函数时，gevent的真正威力就来了。 Gevent已经处理了所有的细节，以确保您的网络库将尽可能隐式地产生他们的greenlet上下文。 我不能强调这是一个强大的成语。 但也许一个例子将说明。

```python
import time
import gevent
from gevent import select

start = time.time()
tic = lambda: 'at %1.1f seconds' % (time.time() - start)

def gr1():
    # Busy waits for a second, but we don't want to stick around...
    print('Started Polling: ', tic())
    select.select([], [], [], 2)
    print('Ended Polling: ', tic())

def gr2():
    # Busy waits for a second, but we don't want to stick around...
    print('Started Polling: ', tic())
    select.select([], [], [], 2)
    print('Ended Polling: ', tic())

def gr3():
    print("Hey lets do some stuff while the greenlets poll, at", tic())
    gevent.sleep(1)

gevent.joinall([
    gevent.spawn(gr1),
    gevent.spawn(gr2),
    gevent.spawn(gr3),
])
```

```
Started Polling:  at 0.0 seconds
Started Polling:  at 0.0 seconds
Hey lets do some stuff while the greenlets poll, at at 0.0 seconds
Ended Polling:  at 2.0 seconds
Ended Polling:  at 2.0 seconds
```

一个比较综合的例子,定义一个task函数,它是不确定的(并不能保证相同的输入输出).在这种情况运行task函数的作用只是暂停其执行几秒钟的随机数.

```python
import gevent
import random

def task(pid):
    """
    Some non-deterministic task
    """
    gevent.sleep(random.randint(0,2)*0.001)
    print('Task', pid, 'done')

def synchronous():
    for i in range(1,10):
        task(i)

def asynchronous():
    threads = [gevent.spawn(task, i) for i in xrange(10)]
    gevent.joinall(threads)

print('Synchronous:')
synchronous()

print('Asynchronous:')
asynchronous()
```

```
Synchronous:
Task 1 done
Task 2 done
Task 3 done
Task 4 done
Task 5 done
Task 6 done
Task 7 done
Task 8 done
Task 9 done
Asynchronous:
Task 1 done
Task 6 done
Task 5 done
Task 0 done
Task 9 done
Task 8 done
Task 7 done
Task 4 done
Task 3 done
Task 2 done
```
在同步的情况所有任务都会顺序的运行,当每个任务执行的时候导致主程序 blocking.

程序重要的部分是包装起来的函数gevent.spawn , 它是Greenlet的线程. 初始化的greenlets储存在一个数组threads ,然后提交给 gevent.joinall函数,然后阻塞当前的程序去运行所有greenlets.只有当所有greenlets停止的时候程序才会继续运行.

要注意的是异步的情况程序是无序的,异步的执行时间是远少于同步的.事实上同步去完成每个任务停止2秒的话,结果是要20秒才能完成整个队列.在异步的情况最大的运行时间大概就是2秒,因为每个任务的执行都不会阻塞其他的任务.

一个更常见的情况,是从服务器上异步获取数据,请求之间 fetch() 的运行时间会给服务器带来不同的负载.

```python
import gevent.monkey
gevent.monkey.patch_socket()

import gevent
import urllib2
import simplejson as json

def fetch(pid):
    response = urllib2.urlopen('http://json-time.appspot.com/time.json')
    result = response.read()
    json_result = json.loads(result)
    datetime = json_result['datetime']

    print 'Process ', pid, datetime
    return json_result['datetime']

def synchronous():
    for i in range(1,10):
        fetch(i)

def asynchronous():
    threads = []
    for i in range(1,10):
        threads.append(gevent.spawn(fetch, i))
    gevent.joinall(threads)

print 'Synchronous:'
synchronous()

print 'Asynchronous:'
asynchronous()
```