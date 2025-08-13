#  Python学习文档 #

***
## 基础篇 ##
---

### 1 Lambda表达式 ###


#### 1.1 表达式 ####
❗️***lambda表达式定义了匿名函数***

```python
lambda 参数列表：表达式

# 定义方法1
def fun1(name):
		return f'hello {name}'

# 定义方法2
fun1 = lambda name: f'hello {name}'
```
❗️*** 两种定义方法等价*** 



#### 1.2 缺省参数 ####

```python
list[]

for i in range(1,4):
		list.append(lambda: i)		# 注意此时并未进行计算，只是进行函数定义
		
for func in list:
		res = func()							# 此时才进行调用，而此时 i = 3
		print(res)								# 因此 输出应为3 3 3
```
**若想让最终答案为 1 2 3 ，则应该如下代码：**
```python
list[]

for i in range(1,4):
		list append(lambda a = i: a)		# 因为是默认值，所以在定义的时候就已经定义到表达式中了
		
for func in list:
		res = func()
		print(res)											# 最终输出为 1 2 3
```
---




### 2 迭代&迭代器 ###
***迭代：遍历整个数据结构***
***迭代器：用于遍历的一种手段，通常与next( )搭配使用***
```python
# 方式一：利用for循环
list = [23,14,53,25]

it = iter(list)

for i in it:					# for循环后，可以加一个列表，也可以加迭代器
		print(i)					# 没有数据时，for循环会处理StopIteration错误，自动截止
		
# 方式二：重复使用next()
list = [23,14,53,25]

it = iter(list)

print(next(it))
print(next(it))
print(next(it))
print(next(it))				# 只能加少于已知数量的next() 否则会报StopIteration错误
```
---



### 3 map( )、filter( )、reduce( )函数 ###
#### 3.1 map( )函数 ####
❗️***用于将一个函数应用到可迭代对象（如列表、元组、字符串）的每个元素，并返回一个迭代器***

**基础语法**	`map(function, iterable, ...)`

>**function：要应用的函数，可以是普通函数也可以是lambda匿名函数**
>**iterable：可迭代对象，如列表、元组、字符串等。**


**用例一：对列表每个元素平方**
```python
numbers = [1, 2, 3, 4]
print(list(map(lambda x:x**2,numbers)))		# 输出为[1,4,9,16]
```

**用例二：提取元组中的项**
```python
students = [("张三",23), ("李四",21), ("王五",29)]
print(list(map(lambda x:x[1],students)))		# 输出为[23,21,29]
```
**用例三：将字符串列表转为整数列表**
```python
str_list = ["1","2","3"]
print(list(map(int,str_list)))		# 输出应为[1,2,3]
```
**用例四：多参数**
```python
list1 = [1,2,3]
list2 = [4,5,6]
print(list(map(lambda x,y:x+y,list1,list2)))		# 输出为[5,7,9]
```

❗️==***map( )函数可以快速对可迭代对象进行简单操作***==

---

#### 3.2 filter( )函数 ####

❗️***用于从一个可迭代对象中筛选满足条件的元素，返回一个迭代器***

**基础语法**	`filter(function, iterable)`

>**function：筛选函数，返回True或False或None**
>**iterable：可迭代对象，如列表、元组、字符串等。**

|场景|示例代码|输出|
|:-:|:-:|:-:|
|过滤数字|`print(list(filter(lambda x:x>10,[2, 3, 18, 21])))`|[18, 21]|
|筛选有效数据|`print(list(filter(bool,[0, 1, "", "a"])))`|[1, 'a']|
|字符串过滤|`print(list(filter(str.isalpha,"a1b2c")))`|['a', 'b', 'c']|
|多条件筛选|`print(list(filter(lambda x:x%2==0 and x>2,[1,2,3,4])))`|[4]|
|与map嵌套|`print(list(map(lambda x:x**0.5,filter(lambda x:x>0,[-1,0,4,16]))))`|[2.0, 4.0]|

---

#### 3.3 reduce( )函数 ####
❗️***用于==累积计算==的高阶函数（来自 functools 模块），它会对可迭代对象中的元素从左到右依次应用函数，最终将序列缩减为单个值。***


**基础语法**	
```python
from functools import reduce

reduce(function, iterable[, initializer])
```

>**function：接收两个参数的函数（第一个是累积值，第二个是当前元素）**
>**iterable：可迭代对象，如列表、元组等。**
>**initializer（可选）：初始累积值，若为空则默认用迭代器的第一个元素。**

❗️**reduce()每次一次从可迭代对象取前两个元素进行操作，然后将结果与下一个对象进行操作，以此类推。**

#### 用例 ####

|场景|示例代码|输出|
|:-:|:-:|:-:|
|  计算阶乘  |            `reduce(lambda x,y: x*y, range(1,6))`             |   `120` (5!)    |
|  找最大值  |        `reduce(lambda x,y: x if x>y else y, [7,2,9])`        |       `9`       |
| 列表扁平化 |           `reduce(lambda x,y: x+y, [[1,2],[3,4]])`           |   `[1,2,3,4]`   |
|  字典合并  |   `reduce(lambda d1,d2: {**d1,**d2}, [{'a':1}, {'b':2}])`    | `{'a':1,'b':2}` |


#### 初始值（initializer）的作用 ####
❗️**当迭代器为空或需要指定初识累计值时使用**
```python
# 计算总和并初始值为10
nums = [1, 2, 3]
total = reduce(lambda x, y: x + y, nums, 10)
print(total)  # 输出: 16 (10+1+2+3)

# 处理空列表时避免错误
empty_list = []
result = reduce(lambda x, y: x + y, empty_list, 0)
print(result)  # 输出: 0
```
---



### 4 列表、字典解析 ###

#### 4.1 列表解析 ####
❗️***列表解析是对一个可迭代对象操作  ==形成新列表==  的过程***
❗️***map()操作是对原可迭代对象进行的操作，列表解析形成新列表，不动原列表***
**基础语法一**   `[输出表达式 for 元素 in 列表]` 


**用例一：求列表值的平方**
```python
# map方法
print(list(map(lambda x:x*x,[1, 2, 3])))

# 列表解析
print([a*a for a in [1, 2, 3]])
```

**基础语法二**  `[输出表达式 for 元素 in 列表 if 条件语句]`


**用例二：大于2的再进行平方**
```python
# 列表解析的条件语句，适用于比较简单的条件
print([a*a for a in [1, 2, 3, 4] if a>2])
```

#### 4.2 字典 ####

**键值对（key：value）**

##### 4.2.1 访问字典的内容 #####


```python
student = {
		'name' : 'Tom',
		'age'  : '22',
		'hobbies' : ['game','travel','swimming'] 
}

# 1.使用key访问  如果关键字不存在会报错
print(student['age'])

# 2.用get方法访问  如果关键字不存在会显示None  推荐
print(student.get('age'))
```


##### 4.2.2 遍历字典中的内容 #####

❗️***取得字典的内容列表***   `dict.items()`
```python
student = {
		'name' : 'Tom',
		'age'  : '22',
		'hobbies' : ['game','travel','swimming'] 
}

# student.item()返回的是由字典转换的列表
for data in student.items():
		print(data) 				#		data打印出来的是元组
    print(data[1])			#		此时打印出来的是字典的值
```
❗️***取得字典的所有key列表***
```python
for key in student.keys():
		print(student.get(key))
```
❗️***直接遍历字典***
```python
for key in student:					# 此时拿到的仍然是字典的key
		print(student.get(key))
```
❗️***取得字典的value列表***
```python
for s in student.values():
		print(s)
```

---

#### 4.3 字典解析 ####


❗️***字典解析是对一个可迭代对象操作  ==形成新字典==  的过程***

**基础语法**  `{key_expr: value_expr for item in iterable if condition}`
>**key_expr：生成键的表达式**
**value_expr：生成值的表达式**
**item：可迭代对象中的元素**
**if condition（可选）：过滤条件**


**用例一：列表转字典**
```python
words = ["apple", "banana", "cherry"]
length_dict = {word: len(word) for word in words}
# 输出: {'apple': 5, 'banana': 6, 'cherry': 6}
```
**用例二：筛选特定项创建字典**
```python
numbers = [1, 2, 3, 4]
square_dict = {n: n**2 for n in numbers if n % 2 == 0}
# 输出: {2: 4, 4: 16}

# 对字典的操作相同 (对大于等于60分的同学分数增加20%，形成新字典)
grades = {'Jack': 80,'Tom': 30,'Mary': 60}
new_grades = {name:int(grade*1.2) for name,grade in grades.items() if grade >= 60}
print(new_grades)
# 输出：{'Jack': 96, 'Mary': 72}
```


---
### 5 模块与包 ###


#### 5.1 什么是模块 ####
>[!note]
>***1.可以认为一个py文件就是一个模块***
>***2.一个模块中可以包含函数、类等多种信息***


#### 5.2 如何引入其他模块 ####
##### 5.2.1 import 模块名 （as 别名） #####

❗️***假设根目录下有两个文件main.py和utils.py***


***要点一：import直接引入同根目录模块时，调用引入文件中的函数需要加上模块名utils.add()***
```python
#	utils.py
def add(x:int,y:int) ->int:
		return x+y
```

```python
#	main.py
import utils
res = utils.add(2,3)
print(res)	
```

***要点二：import引入模块时，模块里的语句会被执行***

```python
# utils.py
print("This is utils.py")
```
```python
# main.py
import utils
print("This is main.py")
# 运行main.py时会输出：
# This is utils.py
# This is main.py
```

***要点三：as后加别名，引用可以不写原模块名***

```python
 # main.py
import utils as ut
print(ut.add(1,2))
```

##### 5.2.2 from  模块名 import 函数/类名 (as 别名) #####

❗️***假设根目录下有两个文件main.py和utils.py***

***要点一：从模块名引入某个函数/类时，使用可直接使用***

```python
# utils.py
def add(x:int,y:int) -> int:
		return x+y
```
```python
# main.py
from utils import add
print(add(1,2))
```

***要点二：可以通过"\*"来引入模块的所有函数/类***

```python
# utils.py
def add(x:int,y:int) -> int:
		return x+y
def sub(x:int,y:int) -> int:
		return x-y
```
```python
# main.py
from utils import *
print(add(1,2))
print(sub(2,1))
```

>[!tip]
>*如果想要引入多个函数，建议通过from utils import add,sub 来引入，可以显示来源*


#### 5.3 \_\_name\_\_变量 ####
>[!note]
>*1.当执行一个程序文件时，'\_\_name\_\_'变量的内容就是'\_\_main\_\_'*
>*2.当一个程序文件被其他文件引入时，'\_\_name\_\_'变量的内容就是模块名*

❗️***通常程序会有一个主程序入口***
```python
def main():
		pass
		
if	__name__ == '__main __'
		main()
```


#### 5.4 包的使用 ####


>[!note]
>*1.包对应着文件夹*
>*2.每个包文件夹下需要一个\_\_init\_\_.py文件*
>*3.可以使用from...import...来引入不同包中的模块*

❗️***from 包名 import 模块名***
❗️***from 包名.子包名 import 模块名***


---
## 进阶篇 ##
---
### 1 装饰器 ###
>[!note]
>***允许在不修改原函数代码的情况下动态扩展其功能。***
>***说人话就是：将函数中相同的逻辑提取出来，用装饰器让其他函数实现这些功能***
#### 1.1 自定义装饰器 ####
❗️***基础语法***
```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("函数执行前")
        func()
        print("函数执行后")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
```
#### 1.2 @wraps装饰器 ####

>[!note]
>***在上述装饰器中，函数的元信息已经被修改，但我还想保留我调用函数的信息，需要用到@wraps***

```python
from functools import wraps

def decorator(func):
    @wraps(func)							# 这个语句就是保留信息的关键语句
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```


#### 1.3 带参数的装饰器 ####


```python
def repeat(n):					# 相当于将repeat函数内部的装饰器返回，返回一个装饰器
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)						# 相当于 repeat(3)返回了一个装饰器，然后@decorator
def greet(name):
    print(f"Hello {name}")

greet("Alice")
```

---
### 2 生成器 ###


***使用 yield 关键字替代 return，函数执行到 yield 时暂停并返回当前值，下次调用时从暂停处继续执行。***
**用例一：求平方数**
```python
def square(count:int):				# 相当于一个可迭代对象
		for i in range(count):
				yield i**2						# 只有当有next()或者for循环、list()等方法时才会继续生成
				
for num in square(4):
		print(num)
```

***因其惰性计算的性质，生成器具有较高性能***


|特性|说明|
|:-:|:-:|
|惰性求值|只在调用 next() 或迭代时生成值，节省内存|
|状态保持|每次 yield 后保留局部变量状态|
|一次性使用|遍历结束后需重新创建生成器|
|兼容迭代协议|可被 for 循环、sum()、list() 等直接使用|

***生成器VS列表***

| 对比项       | 生成器             | 列表                          |
| :------------: |:------------------:|:-----------------------------:|
| **内存占用** | 极低（逐个生成）   | 高（存储所有元素）            |
| **执行时机** | 惰性（按需计算）   | 立即计算                      |
| **复用性**   | 遍历后需重新创建   | 可多次遍历                    |
| **适用场景** | 大数据流、无限序列 | 需随机访问/多次遍历的小数据集 |




---

### 3 上下文管理器 ###


❗️***上下文管理器（Context Manager） 是一种用于精确控制资源分配和释放的机制，通过 with 语句自动管理资源的生命周期（如文件操作、数据库连接、线程锁等）***

***用例引入：文件操作***
```python
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)
# 文件会在 with 块结束后自动关闭，即使发生异常
```

***基础用法***

>[!note]
>**1.\_\_enter\_\_方法在进入 with 块时调用，返回值赋值给 as 后的变量**
>**2.\_\_exit\_\_方法在退出 with 块时调用，处理异常（参数为异常类型、值、traceback）
>3.通过两个方法的调用实现上下文管理**
```python
import time
class Timer:
      def __init__(self):
          self.elapsed = 0

      def __enter__(self):
          self.start = time.perf_counter()
          return self  # 可返回对象供 with 块使用

      def __exit__(self, exc_type, exc_val, exc_tb):
          self.stop = time.perf_counter()
          self.elapsed = self.stop - self.start
          return False  # 若返回 True 会抑制异常

# 使用示例
def func():
		nums = []
		for i in range(10000):
				nums.append(i**2)
with Timer() as timer:
		func()
		
print(timer.elapsed)
```

>[!warning]
>**1.若 with 块内发生异常，会传递给\_\_exit\_\_ 的三个参数
>2.\_\_exit\_\_ 返回 True 表示抑制异常（隐藏错误），False 则向上传播异常**


---
### 4 并发编程 ###

#### 4.1 线程 ####


***❗️基础语法：创建线程***
```python
from threading import Thread

def task():
		...
thread1 = Thread(target = task, args = )
thread2 = Thread(target = task, args = )

thread1.start()
thread2.start()

thread1.join()		# 主线程等待线程1结束
thread2.join()		# 主线程等待线程2结束
```

***❗️通过继承类来创建线程：循环打印***

```python
from threading import Thread
import time

class MyThread(Thread):
		def __init__(self, name:str, count:int)
				super().__init__()
				
				self.setName(name)			# 设置线程的名字
				self.count = count
		
		def run(self) -> None:
				for n in range(self.count):
						print(f"{self.getName()} - {n}\n", end = '')		
						time.sleep(0.01)

t_1 = MyThread("A",10)
t_2 = MyThread("B",10)

t_1.start()
t_2.start()
```

***❗️守护线程***



>[!note]
>***1.守护线程会在主线程结束的时候自动结束***
>***2.主线程则需要等到所有非守护线程结束才能结束***
>***3.守护线程一般用于非关键线程，比如日志***


```python
# 方法一：在类中直接设置self.setDaemon(True)
# 方法二：在创建线程时设置
thread = Thread(target = task, daemon = True)
```

***❗️线程安全队列***



>[!note]
>***✅线程安全队列的"安全"主要体现在它能够确保在多线程环境下对共享数据的访问不会导致竞态条件、数据损坏或不一致等问题。***
>>    ​	**1. 操作的原子性（Atomic Operations）
>>​	线程安全队列的所有操作（如put()、get()等）都是原子性的，这意味着：
>>（1）每个操作在执行过程中不会被其他线程中断
>>（2）其他线程看到的是操作完成前或完成后的状态，不会看到中间状态
>>​	例如，当多个线程同时调用put()时，队列内部会确保每个元素被完整地、按顺序添加到队列中，不会出现元素丢失或顺序混乱的情况。**
>
>>**2. 内置同步机制
>>线程安全队列内部使用了多种同步原语来保证安全：
>>（1）互斥锁（Lock）：保护队列内部数据结构不被并发访问
>>（2）条件变量（Condition Variable）：协调生产者和消费者线程
>>		当队列为空时，消费者线程会自动等待
>>		当队列有空间时，生产者线程会被通知**


```python
from queue import Queue						#	模块queue中导入Queue
q = Queue()												# 创建队列
q.put(item, block = False)				# 队列中放入item，block阻塞，False指队列满了继续放会抛出异常，为True则会等待
q.put(item, timeout = 3)					# timeout指等候时间
q.get(block = False)							#	从队列中取出item，False指若队列为空则抛出异常，True则会等待
q.get(timeout = 10)								#	timeout指等候时间
q.qsize()													#	查看队列大小
q.empty()													#	查看队列是否为空
q.full()													#	查看队列是否为满
```




#### 4.2 线程锁 ####


***❗️当多个线程在同一时刻访问相同数据时可能产生数据丢失、覆盖、不完整等问题，线程锁是用来解决这一问题的重要手段。***


```python
#	用法一：try-finally模式
from threading import Lock
lock = Lock()
lock.acquire()
try:
		# do something
finally:
		lock.release()
		
		
#	用法二：with模式
from threading import Lock

lock = Lock()
with lock:
		#	do something
```
#### 4.3 线程池 ####


>[!note]
>***1.线程的创建和销毁相对比较昂贵***
>***2.频繁的创建销毁线程不利于高性能***
>***3.线程池是python提供的便于线程管理和提高性能的工具***

|<img src="/Users/heartye/Desktop/截屏2025-08-12 18.37.18.png" alt="线程的生命周期" style="zoom:50%;" />|
|:--:|
|*图1 线程的生命周期*|

**新建线程系统需要分配资源，终止线程系统需要回收资源，如果可以重用线程，则可以减去新建/终止的开销**

| <img src="/Users/heartye/Desktop/截屏2025-08-12 18.52.53.png" style="zoom: 33%;" /> |
| :----------------------------------------------------------: |
|                     *图2 线程池分配方式*                     |

**ThreadPoolExcutor的使用语法**
```python
from concurrent.futures import ThreadPoolExcutor, as_completed
```
***用法一：map函数（简单，注意map函数的结果和入参顺序是对应的）***

```python
with ThreadPoolExcutor() as pool:

		results = pool.map(craw, urls)
		
		for result in results:
				print(result)
```

***用法二：future模式（更强大，注意如果用的是as_completed顺序是不定的）***

```python
with ThreadPoolExcutor() as pool:

		futures = [pool.submit(craw, url) for url in urls]
		for future in futures:
				print(future.result())
		for future in as_completed(futures):
				print(future.result())
```

#### 4.4 多进程 ####


>[!note]
>***❗️如果是CPU密集型计算，多线程反而会降低执行速度***
>
>>***1.虽然有全局解释器锁GIL，但是因为有IO的存在，多线程依然可以加速运行。***
>>***2.对于CPU密集型计算，线程的自动切换，反而变成了负担，多线程甚至减慢了运行速度。***
>
>***❗️multiprocessing模块就是python为了解决GIL缺陷引入的模块，原理是用多进程在多CPU上并行执行。***
>

|语法条目|多线程|多进程|
|:-:|:--|:-|
|引入模块|`from threading import Thread`|`from multiprocessing import Process`|
|新建<br>启动<br>等待结束|`t = Thread(target = func, args = (100,))`<br>`t.start()`<br>`t.join()`|`p = Process(target = f, args = ('Bob',))`<br>`p.start()`<br>`p.join()`|
|数据通信|`from queue import Queue`<br>`q = Queue()`<br>`q.put(item)`<br>`item = q.get()`|`from multiprocessing import Queue`<br>`q = Queue()`<br>`q.put([42, None, 'Hello'])`<br>`item = q.get()`|
|线程安全加锁|`from threading import Lock`<br>`lock = Lock()`<br>`with lock:`<br>`   # do something`|`from multiprocessing import Lock`<br>`lock = Lock()`<br>`with lock:`<br>`   # do something`|
|池化技术|`from concurrent.futures import ThreadPoolExecutor`<br><br>`with ThreadPoolExecutor as executor:`<br>`		#	方法一`<br>`		results = executor.map(func, [1,2,3])`<br>`		#	方法二`<br>`		future = executor.sumbit(func, 1）`<br>`		result = future.result()`|`from concurrent.futures import ProcessPoolExecutor`<br><br>`with ProcessPoolExecutor as executor:`<br>`		#	方法一`<br>`		results = executor.map(func, [1,2,3])`<br>`		#	方法二`<br>`		future = executor.sumbit(func, 1)`<br>`		result = future.result()`|


### 5 异步IO ###

#### 5.1 协程 ####
#### 5.2 创建任务 ####
#### 5.3 任务进阶 ####
