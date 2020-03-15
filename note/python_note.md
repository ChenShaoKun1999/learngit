# 控制语句

## 循环，条件

```python
for var in iter:
    do_something(var)

while condition1:
    do_something()

    if condition2 and condition3:
        break
    elif condition4 or condition5:
        continue
    else:
        pass

# 单行条件语句
a = 1
b = a if a > 0 else 0
```

# 数据类型

## 字符串(string)

```python
# 定义
s1 = 'this is a string'
s2 = "this is also a string"
r = r'\string without escape character'
b = b'this is bytes, not string'

s1 + s2   # 字符串拼接
s1 == s2  # 比较是否相等
```

### 方法

| method  | description      |
| ------- | ---------------- |
| title   | 首字母大写       |
| upper   | 大写             |
| lower   | 小写             |
| rstrip  | 去除结束空白     |
| lstrip  | 去除开头空白     |
| strip   | 去除两侧空白     |
| split   | 按照指定序列分割 |
| encode  | 编码             |
| format  | 格式化字符串     |
| replace | 文本替换         |

### 对字符串的函数

| function | description            |
| -------- | ---------------------- |
| ord      | 获取字符的整数表示     |
| chr      | 把编码转换为对应的字符 |

## 列表(list)

```python
# 定义
L1 = [1, 2, 3]
L2 = list(iterable_obj)
L3 = [x **2 for x in range(1, 11) if x % 2 == 0]
L4 = [m + n for m in 'ABC' for n in 'XYZ']
    # 3、4称作list comprehension，列表解析，列表推导

# indexing
first = L1[0]
last = L1[-1]

# slicing，切片
middle = L1[1:-1]    # [start:stop:step]，注意实际的最后一个元素是stop的前一个
reversed = L1[::-1]  # start, stop缺省则一直到头
s = slice(1, -1)
middle = L1[slice]   # 先创建slice实例，之后直接引用它，合理使用能提高可读性
start, stop, step = s.start, s.stop, s.step
s.indices(len(L1))   # 自动调整长度，截掉超出范围的

# delete
del L1[1]
L1.remove(2)
L1.pop()
```

### 方法

| mothod  | description            |
| ------- | ---------------------- |
| append  | 在末尾添加元素         |
| insert  | 在指定位置插入元素     |
| extend  | 在末尾添加多个元素     |
| remove  | 删除指定位置元素       |
| pop     | 删除指定位置元素并返回 |
| sort    | 排序                   |
| reverse | 反序                   |

### 对列表的函数

```python
# 排序（原列表不变）
L = [('name', 2), ('foo', 3), ('bar', 1)]
s = sorted(L, key=lambda x: x[1], reverse=True )
# key是一个callable，通过它的结果来排序
# 使用collections.itemgetter或operator.attrgetter运行速度更快，而且支持多关键字排序
```

## 生成器(generator)

```python
# 定义
gen1 = (i**2 for i in range(0, 2))

def gen2():
    # generator function
    # 执行到yield时自动返回，下次执行时从上一次的yield处继续
    for i in range(0, 100):
        yield i

# 使用
next(gen1)  # 0
next(gen1)  # 1
next(gen1)  # raise StopIteration

for item in gen2():
    print(item)
```

## 元组(tuple)

同列表，不过使用圆括号()，且不能改变存储的值
只有一个元素的tuple要写成(elecment1 , )以和普通的括号区分

## 字典(dict)

```python
# 定义
d1 = {'1':1, '2':2}
d2 = {i:i**2 for i in range(0, 10)}

# 访问
d1['1']
d1.get('2')

# 添加，修改
d1['3'] = 3
d1['3'] = 'c'

# 删除
del d1['1']
d1.pop('2')

# 遍历
for key, value in d1.items():
    	pass
# 类似的，有dict.keys()和dict.values()
# 也可以直接for key in dict

# 判断key在不在字典中
'3' in d1
```

### 方法

```python
# 如果原本没有这个键，就设置为default，并返回default
# 否则，返回键对应的值
d = {}
d.setdefault('a', []).append(1)
```



## 集合(set)

无序的不重复元素序列

```python
# 定义集合
s1 = set([1, 2, 3])   # 必须用一个iter
s2 = {'a', 'b', 'c'}  # 空集合不能这样，否则会初始成dict
s3 = {x for x in 'this is a set' if x not in 'abc'}
    # Set comprehension

# 交并补
s1 & s2    # 交集
s1 | s2    # 并集
s1 - s2    # 差集
s1 ^ s2    # 不重复的元素，即并集 - 交集

# 添加/删除元素
s1.add(4)      # 添加一个元素
s2.update(s1)  # 添加多个元素
s2.remove('a') # 移除一个元素
s3.discard(1)  # 移除一个元素，并且尝试移除不存在的元素时不报错
s3.clear()     # 清空集合
```

除了用运算符之外，还可以用方法来进行集合运算

| method               | description  |
| -------------------- | ------------ |
| intersection         | 交集         |
| union                | 并集         |
| difference           | 差集         |
| symmetric_difference | 不重复的元素 |
| issubset             | 判断是否子集 |
| issuperset           | 判断是否超集 |

# 函数(function)

```
def 函数名(实参表):
	函数内容

函数调用
1.位置参数	参数名
2.默认参数	参数名=默认值
3.可变参数	*args，调用时函数内部收到一个tuple
4.关键字参数	**kw，调用时函数内部收到一个dict
5.命名关键字参数	..., *, 参数名 #如果定义了命名关键字参数，必须采用关键字的形式调用

含有多种参数时，顺序为
位置参数，默认参数，可变参数，命名关键字参数，关键字参数（**参数名）
func(x1, ..., xn, y1=0, ...yn=0, *args, *, z1=0, ...zn=0, **kw)

注意：默认参数指向了一个固定的对象，所以要保证默认参数不会被更改。反例：
def add_end(L=[]):
    L.append('END')
    return L
反复用默认参数调用的时候，L会变成若干个'end'的list

传递列表时，可以选择传递原列表的切片（如：[:]）
这样原列表不会被修改

*列表，表示将列表的每个元素作为参数传入
**字典，将键-值对当作参数名-实参传入函数，要求键必须都是str

返回值
可以有多个返回值，如：
def move(x, y)
    ...
    return x1, y1
a, b=move(x, y)
实际上返回了一个tuple。在其他地方也可以这样批量赋值

将函数存储在模块中
将若干个函数储存在其他py文件中，用import语句导入
类型		导入方法				引用方法		注释
导入模块中全部函数	import module			module.function()	module不带.py	
导入特定函数	from module import function1, function2, ...	function()
给函数指定别名	from module import function as f		f()
给模块指定别名	import module as m			m.function()
导入模块中所有函数	from module import *			function()		不建议使用(防止函数重名)
```

## 匿名函数(lambda)

```
lambda variable: expression
等价于：
def variable:
    reutrn expression

例：
f = lambda x : x*x
```

## map, filter 和 reduce

```python
func = lambda x: return x
func2 = lambda x, y: return x*y
L = List()

map(func, L)       #返回一个迭代器，此迭代器产生每项是f(x[i])
filter(func, L)    #若函数返回值为False，筛掉列表对应项，返回相应Iterator

from functools import reduce
reduce(func2, L)   #f(x, y)应该接收两个参数
                   #按顺序把列表内每个元素都用函数算一遍（形如f(2, f(1, 0))）
```
## 闭包(closure)

闭包指引用了自由变量的函数，被引用的变量将和函数一同存在。应用中经常是产生一个包含了外部变量的函数，需要的时候可以不需要另外提供实参地调用

```python
def cmp(x, y):
    return lambda: True if x==y else False

def cmp_closure(arg):
    return lambda: True if arg[0]==arg[1] else False

arg = [1, 2]
func = cmp(*arg)
    # 由于传参时把数复制进去了，func内部没有引用外部变量，不是闭包
clos = cmp_closure(arg)
    # clos里面引用的arg和外面的是同一个变量，因此是闭包
    # 外面对arg的修改会反映到clos的结果
    # 闭包把arg的生存期增加到至少和闭包生存期一样长
func()   # False
clos()   # False

arg = [1, 1]
func()   # 仍然为False
clos()   # True
```

## 装饰器(decorator)

接受一个函数，返回一个函数的函数
@装饰器
def f(x) : ...
此时执行f(x)时自动执行被装饰过的f(x)

如果decorator需要参数，则需要嵌套。例：
```python
def log(text) :
    def decorator(func) :
        def wrapper() :
            #do something
        return wrapper
    return decorator
```
使用@log("execute")的方法装饰

func.__name__可以显示函数本名
functools.wraps有更名工具，可用于装饰器防止函数签名混乱

## 偏函数

functools.partial(函数，[参数=]值)
将某些参数的值固定住创造一个新的偏函数

# 类(class)

```python
class Dog(object):

    def __init__(self, name, age):
        self.name=name
        self.age=age

    def method(self, *args, **kw):
        do_something()
        #self是特殊的变量，指代该实例自身。调用函数时会自动传入实参self
        #因此无需写出self
```

例子中，Dog.name, Dog.age成为类的属性(attribute)，类中的函数称为方法
可以用inst.attribute, inst.method()的方法引用
创建实例：my_dog=dog('willie', 6)
此时__init__方法被自动调用

类属性
在类中（且不在方法中）定义的变量成为类属性
每一个实例在创建时，自动获得所在类的（当前）属性
可以通过class_name.attr修改类属性，inst_name.attr修改实例属性

对类的函数

```
dir(instance)		返回一个列表，里面装着这个类型全部的属性和方法
hasattr(instance, "attribute_name")	判断一个实例有没有某个属性
getattr(同上)		返回该属性的值。多加一个参数，当没有该属性时返回它
setattr(同上)		设置
```



## 私有变量

```
属性名起成_var_name，该变量成为私有变量(private)，外部无法直接访问
实际上是解释器把变量名从_var改成了_class_name__var_name（依据解释器不同，可能有不同结果）
```

装饰器@property
例子：

```python
@property
def score(self) :
    return self._score
@score.setter
def score(self, value) :
    self._score=value
```

使用装饰器给类“添加”了一个属性score。如果不设置setter，则成为只读属性
使用时，直接用self.score就可以进行赋值和引用，但实际上是通过这两个函数进行操作

使用装饰器给类“添加”了一个属性score。如果不设置setter，则成为只读属性
使用时，直接用self.score就可以进行赋值和引用，但实际上是通过这两个函数进行操作

## 魔术方法

```
__init__(self , attr_name)
初始化函数，创建实例时被自动运行（用形如m=Dog("Daisy", 4)的方式创建）

__len__(self)
对类使用len()函数时调用class_name.__len__()

__slots__=(self, attr_name)
试图给类添加不在__slots__中的属性会引起AttributeError
如果子类没有slots,则父类的slots不起作用
如果有，则子类的限制是两个slots的并集

__str__(self)
print(实例)时执行

__repr__(self)
在命令行输入类/实例，或用repr()时执行

__getattr__(self, attr_name)
试图调用某个不存在的属性/方法时调用，返回该属性的值/某个函数（被当作方法使用）
例：
def __getarr__(self, attr):
    if attr=='score' :
        return 100
    if attr=='print_score'
        return lambda: 100
注意：如果调用__getarr__后还是没得到一个结果（没有从某处return），将返回None而不会有任何提示。可以通过raise AttributeError防止隐藏的错误

__call__(self[, args])
用inst()，即把实例当作函数时调用

类似list/dict的行为
__iter__(self)		使用for循环时调用，返回一个迭代对象（如self自身）
__next__(self)		循环中不断使用__next__获取下一个值，遇到StopIteration()错误时停止
			注：__next__的self指__iter__返回的对象。有__iter__和__next__的类被视作Iterator
__getitem__(self, key)		self[key]
			注：因为本质是函数，使用切片(slice)会报错，需要在函数内添加判断和产生切片功能
			slice有属性s.start, s.stop, s.step等属性，可以用if做出切片时的分支，返回一个列表
__setitem__(self, key, value)	self[key]=value
__delitem__(self, key)		del self[key]
__contians__(self, value)	value in self, value not in self

比较
__cmp__(self, something)
比较self和另外的某个东西（另一个实例，一个数字，或者别的什么）
当self<something时返回负值，self>something时返回正值，self==something时返回0
__eq__(self, something) ==
__ne__(self, something) !=
__lt__(self, something) <
__gt__(self, something) >=
__bool__(self)          判断对象布尔值时调用
正负
__pos__(self)	正号
__neg__(self)	负号
__abs__(self)	abs函数

四则运算
__add__(self, sth)	+
__sub__		-
__mul__		*
__floordiv__	//
__div__		/
__truediv__	真除法（？）
__mod__		%
__pow__		**
当self成为第二个操作数时，在方法名加上r（如，__rdiv__)
进行增量赋值（+=等），在方法名加上i（如，__iadd__）

类型转换
__int__(self)等
```

## 魔术属性

```python
__module__   #模块名
__class__    #类名
__dict__     #对象或类的属性,dict形式
__dir__      #对象或类的属性，list形式，包括__dict__.
__mro__      #对象的继承顺序
__doc__      #对象或函数的描述信息
__file__     #文件的名字，其包含路径信息。
```

## 子类(subclass)与继承

子类继承了父类的全部方法，在子类中定义与父类重名的方法将会覆盖掉父类方法

```python
class MyInt(int):
    def __init__(self, i):
        super().__init__(i)
        # 用super()引用父类(super class)的方法
```

多重继承
class Dog(Animal, MammalMixIn)
常采用MixIn的方法，使类的继承具有一个主线+附加功能

## 元类(metaclass)

metaclass -> class -> instance
（元类创建出类，类创建出实例）

# IO

```python
fp = open(file, 'r')
fp.read()
# 其他读取方法：readline, readlines
fp.close()

with open(file, 'w') as fp:
    fp.write(string)
```

python把和文件类似的对象（能打开，能读能写）称作file-like，都用相同的方法处理

# 错误处理与调试

## try-except

```python
try :
    do_something()
except (KeyError, ValueError) as e:
    # 当运行try语句时遇到错误是被捕获，运行此处语句
    # 如果有as 变量，相应的错误报告存进变量里
    # 会捕获错误的子类
    # 不写错误类型则捕获任何错误。这通常是不好的
    print(repr(e))
else :
    # 当没有遇到错误时进入这里
    pass
finally :
    # 以上处理完毕后，不管有没有错误，运行finally的语句
    pass
```

## assert

```python
assert flag == 1, 'print this if True, else raise AssertionError'
```

启动解释器时用-O(大写字母O)关闭assert

## raise

抛出错误。例：

```python
raise ValueError('message')
```

# 多进程&多线程

多进程：用多个进程同时进行任务    
多线程：在一个进程下启动多个线程

一般而言，python的多进程适合计算密集型任务，多线程适合IO密集型任务

## 多进程

Unix/Linus系统可采用os.fork()，windows系统下使用multiprocessing等模块

因为进程的数量远远多于CPU的核心数，实质上是各个任务交替执行

os.getpid()	返回当前进程编号

### multiprocessing模块


Process类可以放一个进程
方法
Process(target=函数, args=(参数, ) )
start	开始子进程
join	等待该子进程结束后再继续主进程
terminate	终止进程


Pool可以装很多个子进程
Pool(同时执行的最大进程个数)
apply_async	(async=Asynchronous)，向池中添加一个进程
close		关闭池子，同时开始进程
join		等待到所有进程结束


Queue可以帮助子进程间通信
注：Pool和Queue不是类，是bound method BaseContext.Queue of <multiprocessing.context.DefaultContext object
Queue和Pool函数可以创建相应的object，这些对象有相应的方法
put	装入数据
get	取出数据
empty	清空数据
qsize	返回现有数据个数
注：如果读取的时候queue里没有数据，就等到有东西了再继续

特别注意：主进程的全部数据都是通过pickle序列化传入子进程，故很多时候multiprocessing失败是因为pickle失败了

### subprocess模块

函数
run(指令)	运行指令，等待到其结束，返回一个CompletedProcess instance
call(指令)	运行指令，等待到其结束，返回其return code
用Popen类及其communicate方法可以对子进程进行输入

## 多线程

Python的标准库提供了两个模块：_thread和threading，threading是高级模块，对_thread进行了封装。绝大多数情况下，只需要使用threading

### threading模块


函数
current_thread()	返回当前线程的Thread object


Thread类
Thread(target=可调用对象, name=名字, args=(参数,))	名字仅仅用于打印，主线程叫做MainThread，不起名则自动命名为Thread-1等
方法
start
join

多个线程共享进程内的变量，变量可能被不同线程修改。而且，如果若干个线程几乎同时修改一个变量，有可能造成难以预估的错误
Lock()函数
创建一个_thread.lock object，当一个线程获得被锁上的(locked)锁，其他线程将被暂停，等待到这把锁解开为止
方法
acquire	上锁
release	解锁
locked	返回是否上锁

python解释器的GIL(Global Interpreter Lock)锁导致多线程无法利用多核，想有效利用必须要多进程

ThreadLocal可以帮助参数在不同线程中传递

# 内建模块

## collections

```python
import collections
```

### 队列

deque([iterable[, maxlen]]) --> deque object

在队列两端插入或删除元素时间复杂度都是o(1)，而在列表的开头插入或删除元素的时间复杂度为O(N)

### defaultdict

将字典的值默认初始化为指定类型

```python
from collections import defaultdict

d = defaultdict(list)
d['a'].append(1)

# 其实可以用dict.setdefault实现相同的功能
```

### OrderDict

迭代、序列化时维持插入的顺序。修改不会改变顺序。内部用一个链表保存数据，所以大小是普通dict的两倍

### Counter

```python
from collections import Counter

count = Counter(hashable_iter)
a_count = count['a']         # 查看元素出现次数
top3 = count.most_common(3)  # 返回出现最多的元素和其出现次数
count.update(some_more_elements)   # 增加计数

# 计数求和求差
count2 = Counter(whatever)
count + count2
count - count2  # 好像不会减出负数
```



## configparse

一般建议把配置（比如说登录的端口，etc.）单独放进一个文件而不是硬编码在程序里面，此模块就是专门用来解析config.ini文件的

### 配置文件格式

```ini
[Simple Values]
key=value
spaces in keys=allowed
spaces in values=allowed as well
spaces around the delimiter = obviously
you can also use : to delimit keys from values

[All Values Are Strings]
values like this: 1000000
or this: 3.14159265359
are they treated as numbers? : no
integers, floats and booleans are held as: strings
can use the API to get converted values directly: true

[Multiline Values]
chorus: I'm a lumberjack, and I'm okay
    I sleep all night and I work all day

[No Values]
key_without_value
empty string value here =

[You can use comments]
# like this
; or this

# By default only in an empty line.
# Inline comments can be harmful because they prevent users
# from using the delimiting characters as parts of values.
# That being said, this can be customized.

    [Sections Can Be Indented]
        can_values_be_as_well = True
        does_that_mean_anything_special = False
        purpose = formatting for readability
        multiline_values = are
            handled just fine as
            long as they are indented
            deeper than the first line
            of a value
        # Did  I mention we can indent comments, too?
```

### 使用例

```python
import configparser

config = configparser.ConfigParser()
config.read('example.ini')

# get a list of all sections
config.sections()

# you can treat config as dict
config['Simple Values']['key']

# get int/float/boolean values
# boolean value from 'yes'/'no', 'on'/'off', 'true'/'false' and '1'/'0'
# 可以自定义converter
config['All Values Are Strings'].getint('values like this')
config.getfloat('All Values Are Strings', 'or this')

# edit config
config['DEFAULT'] = {'ServerAliveInterval': '45',
                     'Compression': 'yes',
                     'CompressionLevel': '9'}

# write config file
with open('example.ini', 'w', encoding='utf-8') as fp:
    config.write(fp)
```

## datetime

```python
from datetime import datetime, timedelta

# 指定时间
some_time = datetime(year=2020, month=2, day=2)
# 当前时间
now = datetime.now()
# 时间偏移量
tomorrow = now + timedelta(days=1)

# 时间戳(float, in seconds)
timestamp = now.timestamp()
now_fromstamp = datetime.fromtimestamp(timestamp)
now_utc = datetime.utcfromtimestamp(timestamp)

# 时间字符串
# %Y, %m, %d, %H, %M, %S 时分秒
# %a, %A  简化/完整星期名称（如：Sun, Sunday）
# %b, %B  简化/完整月份名称（如：Jan, January）
# %c	例：Sun May 20 17:01:14 2018
format_s = '%Y.%m.%d %H:%M:%S'
time_str = now.strftime(format_s)
fromstr = datetime.strptime('2020.02.02', '%Y.%m.%d')
```

## hashlib

包括sha256, sha512等哈希算法

```python
import hashlib
h = hashlib.md5(b'some bytes')
h.update(b"update this hash object's state")
b = h.digest()         #得到bytes类型的hash
x = b.hexdigest()      #得到16进制字符串
```

## heapq

### 最大/最小元素

nlargest(n, iterable, key=None)
Find the n largest elements in a dataset. Equivalent to:  sorted(iterable, key=key, reverse=True)[:n]

nsmallest(n, iterable, key=None)
Find the n smallest elements in a dataset.

弹出堆的前N个元素，复杂度NlogM，M为堆大小。查找最大/最小，用min/max较快；N比较小时用nlargest/nsmallest较快；N几乎和M一样大时，排序再切片比较快

## html

```python
from html.parser import HTMLParser

class MyParser(HTMLParser):
    
    def handle_starttag(self, tag, attrs):
        print('start tag: ', tag, attrs)

    def handle_endtag(self, tag):
        print('end tag: ', tag)

    def handle_startendtag(self, tag, attrs):
        print('start end tag: ', tag, attrs)

    def handle_data(self, data):
        print('data: ', data)

    def handle_comment(self, data):
        print('comment: ', data)

    def handle_entityref(self, name):
        print('entityref: ', name)

    def handle_charref(self, name):
        print('charref: ', name)

parser = MyParser()
parser.feed('example.html')
    # feed之后立即分析整个文档，每当遇到相应元素时，就会调用对应handle函数
```

## json

```python
import json    # json = JavaScript Object Notation

# 读写文件
obj = json.load(fp)
json.dump(obj, fp, ensure_ascii=False, indent=2)

# json字符串
json_str = dumps(obj)
obj = loads(json_str)
```

dump函数的ensure_ascii是python独有的，不是通用编码，所以在非python环境再load一个json字符串会出问题

把一个对象先dump再load后，不保证对象不变，比如说dict会变成list、dict的数值键都会变成字符串键，如果有同一个数的数值键和字符串键，其中一个会被覆盖掉

可以通过加入参数default=函数（对于dump和dumps）或object_hook（load和loads），其中的函数能将创建的类转化成一个合法的类型（如dict），或者直接`json.dumps(obj.__dict__)`

## logging

| level    | usage                          |
| -------- | ------------------------------ |
| debug    | 调试信息                       |
| info     | 一般信息，软件正常工作         |
| warning  | 发生意外，但软件还是在正常工作 |
| error    | 错误，软件不能正常执行         |
| critical | 严重错误，软件不能继续运行     |

```python
import logging
logging.basicConfig(level=logging.INFO,
                    filename='save_images.log',
                    format='%(asctime)s %(levelname)s:%(name)s: %(message)s')

logger = logging.getlogger(logger_name)  # 建议用__name__
logger.setLevel(logging.INFO)   # 只有大于这个等级的才输出
logger.info('message')
```

## os(operating system)

| function  | usage                                                      |
| --------- | ---------------------------------------------------------- |
| mkdir     | 创建目录                                                   |
| makedirs  | 创建目录（中间路径被一并创建）                             |
| rmdir     | 删除目录                                                   |
| listdir   | 返回目录下全部文件、文件夹                                 |
| remove    | 删除文件                                                   |
| rename    | 重命名文件、文件夹                                         |
| stat      | 查询文件信息，stat(file).st_size, stat(file).st_mtime, ... |
| walk      | 递归遍历全部子文件、文件夹，生成(path, dirs, files)        |
| startfile | 打开文件                                                   |

### os.path

| function | usage              |
| -------- | ------------------ |
| abspath  | 绝对路径           |
| relpath  | 相对路径           |
| join     | 连接多段路径       |
| split    | 分割路径           |
| isdir    | 判断是否合法文件夹 |
| isfile   | 判断是否合法文件   |
| splitext | 分割扩展名         |

## pickle

python对象的二进制序列化和反序列化。pickling与其他编程语言不兼容，且不同版本的python之间也未必兼容

pickle是不安全的，有办法构建恶意pickle数据，使得在解封时执行任意的代码。不要解封不信任来源的数据、可能被篡改过的数据。需要考虑安全性时，应该用hmal做数字签名

```python
import pickle

obj = pickle.load(fp)
pickle.dump(obj, fp)

pickle_bytes = pickle.dumps(obj)
obj = pickle.loads(pickle_bytes)
```

## re(regular expression)

函数

```
match(pattern, string)		从字符串开头开始匹配，如果匹配成功返回相应Match对象，否则返回None
fullmatch			匹配整个字符串
search			如果查找到，返回pattern第一次出现的Match对象
sub(pattern, repl, string)	将string中符合pattern的部分替换为repl。repl can be either a string or a callable object which returns a string
subn			同sub，返回(替换后字符串, 替换次数)
split(pattern, string, maxsplit=0)	按照pattern分割
findall			返回所有满足pattern的部分的List
finditer			Return an iteraort yielding Match objects
compile			编译pattern字符串
purge			clear regular expression cache
escape(pattern)		在pattern中非字母，数字，_的字符前加\
```

### Match类

```
re.match(pattern, string)匹配成功返回一个Match对象
方法
group		如果pattern中有分组，则返回第n个分组对应的子串。0则返回整个字符串
groups		返回（从1开始）的group对应字串组成的tuple
span(group)	返回一个二元素tuple，装着相应group的span（开始、结束位置）
start(group)		index of the start of the substring matched by group
```

### Pattern类

```
compile(pattern)
方法
match	按照pattern匹配字符串，类似re.match
```

## time

time更接近于操作系统层面，datetime则对time进行了封装，并提供更多功能

函数

```
time	返回当前的时间戳（函数与模块重名）
localtime	返回时间戳对应的当前时区的struct_time
sleep	推迟调用线程（单位：s）
```

## urllib

```python
from urllib import request, parse

# simple GET request
req = request.Request(url)
req.add_header(key, value)
response = request.urlopen(req)  #也可以直接用url做参数，但是这样就不能加header
response.getcode()  # HTTP status code
response.read()     # 网页内容

# POST request
login_data = parse.urlencode(data).encode('utf-8')
req = request.Request('https://passport.weibo.cn/sso/login')
response = request.urlopen(req, data=login_data)

# Proxy
proxy_handler = request.ProxyHandler({'http': 'http://www.example.com:3128/'})
opener = request.build_opener(proxy_handler)
response = opener.open(request)
request.install_opener(opener)  # install at module level

# download network object
# deprecated, maybe better use fp.write(response.read())
def hook(chunk_num, chunksize, totalsize):
    # hook将在开始时和每加载一个chunk后调用
    pass
request.urlretrieve(url, filename, hook)

# parse url
foo = parse.urlparse('https://www.google.com')
foo.netloc  #'www.google.com'
    # see repr(foo) for more attr
foo.geturl()

parse.urlsplit(url)
parse.urljoin(base, url)
```

## warnings

```python
import warnings

warnings.warn('mesage', UserWarning)
# available warning classes: UserWarning, DeprecationWarning,
# SyntaxWarning, RuntimeWarning, ResourceWarning, FutureWarning.
```

# 有用的第三方库

## pydoc-markdown

从docstring直接生成markdown文档

```
pydocmd simple modulename+ > doc.md
```

注意加号不可省略。生成多个模块的文档只需要同时给多个模块名字（用空格隔开）

# 杂项

## 内建功能

```python
# 运算符
# **乘方
# //整除

# del与垃圾回收
del var
# python中的回收策略以引用计数为主，当一个对象的引用数为0时将其回收
# 可以用sys.getrefcount看引用计数
# del语句并不删除对象，而是删除了一个引用

# 求长度，可以用于对字符串、列表、字典、元组、集合等对象
# 乃至任何有__len__方法的对象
len(obj)

#空值
None

# 接受输入
# 输入内容被理解为字符串
var = input('prompt')

range(start，end，step)
# 到达end时结束，实际最后一个元素为end前一个，而不是end
for i in range(0, 10, 2):
    print(i)

# 获得变量类型
type(var)
```

## 文档字符串(docstring)

### google风格

```python
"""Example Google style docstrings

Parameters:
    param1: this is the first param
    param2: this is a second param

Returns:
    This is a description of what is returned

Raises:
    KeyError: raises an exception
"""
```

# 奇技淫巧

```python
# 解压iterable object
it = [1, 2, 3, 4]
a, b, c, d = it       # 个数必须匹配
first, *middle, last  # *middle匹配多个元素

# zip
zip([1, 2, 3], ['a', 'b', 'c'])
    # 获得一个生成器，(1, 'a'), (2, 'b'), (3, 'c')
transposed = list(zip(*matrix))  # 矩阵转置
```

