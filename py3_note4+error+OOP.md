
## 错误 异常

* 语法错误
* 异常：语法正确、运行时检测到错误 typeerror nameerror等等
* 异常处理：try：   except (RuntimeError, TypeError, NameError):
* try except 语句还有一个可选的else子句，如果使用这个子句，那么必须放在所有的except子句之后。这个子句将在try子句没有发生任何异常的时候执行
* 使用 else 子句比把所有的语句都放在 try 子句里面要好，这样可以避免一些意想不到的、而except又没有捕获的异常。



```python
#测试
import sys

try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except OSError as err:
    print("OS error: {0}".format(err))
except ValueError:
    print("Could not convert data to an integer.")
except:
    print("Unexpected error:", sys.exc_info()[0])
    raise
```

    OS error: [Errno 2] No such file or directory: 'myfile.txt'
    


```python
#for arg in sys.argv[1:]:
 #   try:
  #      f = open(arg, 'r')
   # except IOError:
    #    print('cannot open', arg)
    #else:
     #   print(arg, 'has', len(f.readlines()), 'lines')
      #  f.close()
```


```python
def this_fails():
    x = 1/0

try:
    this_fails()
except ZeroDivisionError as err:##不直接用raise的好处
    #raise
    print('Handling run-time error:', err)
```

    Handling run-time error: division by zero
    


```python
raise NameError('HiThere')##raise直接抛出的异常
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-4-f92c75f2cdea> in <module>()
    ----> 1 raise NameError('HiThere')##raise直接抛出的异常
    

    NameError: HiThere


* 用户自定义异常
* 你可以通过创建一个新的exception类来拥有自己的异常。异常应该继承自 Exception 类，或者直接继承，或者间接继承


```python
class MyError(Exception):
    def __init__(self, value):
        self.value = value
    def __str__(self):
        return repr(self.value)
    
try:
    raise MyError(2*2)
except MyError as e:
    print('My exception occurred, value:', e.value)

    
raise MyError('oops!')
```

    My exception occurred, value: 4
    


    ---------------------------------------------------------------------------

    MyError                                   Traceback (most recent call last)

    <ipython-input-5-0974bd705851> in <module>()
         11 
         12 
    ---> 13 raise MyError('oops!')
    

    MyError: 'oops!'


* 当创建一个模块有可能抛出多种不同的异常时，一种通常的做法是为这个包建立一个基础异常类，然后基于这个基础类为不同的错误情况创建不同的子类


```python
#例子
class Error(Exception):
    """Base class for exceptions in this module."""
    pass

class InputError(Error):
    """Exception raised for errors in the input.

    Attributes:
        expression -- input expression in which the error occurred
        message -- explanation of the error
    """

    def __init__(self, expression, message):
        self.expression = expression
        self.message = message

class TransitionError(Error):
    """Raised when an operation attempts a state transition that's not
    allowed.

    Attributes:
        previous -- state at beginning of transition
        next -- attempted new state
        message -- explanation of why the specific transition is not allowed
    """

    def __init__(self, previous, next, message):
        self.previous = previous
        self.next = next
        self.message = message
```

* 定义清理行为
***
* try 语句还有另外一个可选的子句，它定义了无论在任何情况下都会执行的清理行为
* 不管 try 子句里面有没有发生异常，finally 子句都会执行。
* 如果一个异常在 try 子句里（或者在 except 和 else 子句里）被抛出，而又没有任何的 except 把它截住，那么这个异常会在 finally 子句执行后再次被抛出。


```python
try:
    raise KeyboardInterrupt
finally:
    print('Goodbye, world!')
```

    Goodbye, world!
    


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    <ipython-input-7-ee01f6bed4b3> in <module>()
          1 try:
    ----> 2     raise KeyboardInterrupt
          3 finally:
          4     print('Goodbye, world!')
    

    KeyboardInterrupt: 



```python
def divide(x, y):
    try:
        result = x / y
    except ZeroDivisionError:
        print("division by zero!")
    else:
        print("result is", result)
    finally:
        print("executing finally clause")
        
#divide(2, 1)
#divide(2, 0)
#divide("2", "1")
```

* 预定义清理
***
* 一些对象定义了标准的清理行为，无论系统是否成功的使用了它，一旦不需要它了，那么这个标准的清理行为就会执行。
* 关键词 with 语句就可以保证诸如文件之类的对象在使用完之后一定会正确的执行他的清理方法


```python
#以下这段代码执行完毕后，就算在处理过程中出问题了，文件 f 总是会关闭。
with open("test.txt") as f:
    for line in f: #输出每行
        print(line, end="")
```

    菜鸟教程 1
    菜鸟教程 2菜鸟教程 3
    菜鸟教程 4asdfghjkl我们时间asdfghjkl我们时间

## OOP

####  面向对象技术简介
* 类(Class): 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。
* 类变量：类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。
* 数据成员：类变量或者实例变量用于处理类及其实例对象的相关的数据。
* 方法重写：如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。
* 实例变量：定义在方法中的变量，只作用于当前实例的类。
* 继承：即一个派生类（derived class）继承基类（base class）的字段和方法。继承也允许把一个派生类的对象作为一个基类对象对待。例如，有这样一个设计：一个Dog类型的对象派生自Animal类，这是模拟"是一个（is-a）"关系（例图，Dog是一个Animal）。
* 实例化：创建一个类的实例，类的具体对象。
* 方法：类中定义的函数。
* 对象：通过类定义的数据结构实例。对象包括两个数据成员（类变量和实例变量）和方法。
* Python中的类提供了面向对象编程的所有基本功能：类的继承机制允许多个基类，派生类可以覆盖基类中的任何方法，方法中可以调用基类中的同名方法

#### 类对象

* 类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的第一个参数名称, 按照惯例它的名称是 self


```python
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
        
x = Complex(3.0, -4.5)
print(x.r, x.i)   # 输出结果：3.0 -4.5
```

    3.0 -4.5
    

#### 类的方法

* 在类地内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self, 且为第一个参数，self 代表的是类的实例。

#### 继承 and 多继承
* 当基类有多个时，注意输入的顺序。需要注意圆括号中基类的顺序，若是基类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找基类中是否包含方法


```python
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))

        #单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
 
s = student('ken',10,60,3)
s.speak()
```

    ken 说: 我 10 岁了，我在读 3 年级
    


```python
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 #单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
 #另一个类，多重继承之前的准备
class speaker():
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))
 
 #多重继承
class sample(student,speaker):
    a =''
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)

        # 实例化类
test = sample("Tim",25,80,4,"Python")
test.speak()   #方法名同，默认调用的是在括号中排前地父类的方法
```

    Tim 说: 我 25 岁了，我在读 4 年级
    

#### 方法重写

* 如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法，实例如下


```python
class Parent:        # 定义父类
    def myMethod(self):
        print ('调用父类方法')

        
class Child(Parent): # 定义子类
    def myMethod(self):
        print ('调用子类方法')

c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
```

    调用子类方法
    

#### 类的属性 与方法
***
* 类的私有属性

\_\_private\_attrs：两个下划线开头，声明该属性为私有，不能在类地外部被使用或直接访问。在类内部的方法中使用时 self.\_\_private\_attrs。

* 类的方法
* 在类地内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self，且为第一个参数，self 代表的是类的实例。
* self 的名字并不是规定死的，也可以使用 this，但是最好还是按照约定是用 self。

* 类的**私有**方法

\_\_private\_method：两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类地外部调用。self.\_\_private\_methods。


```python
class JustCounter:
    __secretCount = 1  # 私有变量
    publicCount = 0    # 公开变量
 
    def count(self):
        self.__secretCount += 1
        self.publicCount += 1
        print (self.__secretCount)

#类的实例化
counter = JustCounter()
counter.count()
counter.count()
print (counter.publicCount)
print (counter.__secretCount)  # 报错，实例不能访问私有变量
```

    2
    3
    2
    


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-14-2a85e71f3a5d> in <module>()
         13 counter.count()
         14 print (counter.publicCount)
    ---> 15 print (counter.__secretCount)  # 报错，实例不能访问私有变量
    

    AttributeError: 'JustCounter' object has no attribute '__secretCount'



```python
#私有 公有
class Site:
    def __init__(self, name, url):
        self.name = name       # public
        self.__url = url   # private
 
    def who(self):
        print('name  : ', self.name)
        print('url : ', self.__url)
 
    def __foo(self):          # 私有方法
        print('这是私有方法')
 
    def foo(self):            # 公共方法
        print('这是公共方法')
        self.__foo()

x = Site('菜鸟教程', 'www.runoob.com')
x.who()        # 正常输出
x.foo()        # 正常输出
x.__foo()      # 报错
```

    name  :  菜鸟教程
    url :  www.runoob.com
    这是公共方法
    这是私有方法
    


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-15-05e069112d6b> in <module>()
         19 x.who()        # 正常输出
         20 x.foo()        # 正常输出
    ---> 21 x.__foo()      # 报错
    

    AttributeError: 'Site' object has no attribute '__foo'


#### 类的专有方法：
* \_\_init\_\_ : 构造函数，在生成对象时调用
* \_\_del\_\_ : 析构函数，释放对象时使用
* \_\_repr\_\_ : 打印，转换
* \_\_setitem\_\_ : 按照索引赋值
* \_\_getitem\_\_: 按照索引获取值
* \_\_len\_\_: 获得长度
* \_\_cmp\_\_: 比较运算
* \_\_call\_\_: 函数调用
* \_\_add\_\_: 加运算
* \_\_sub\_\_: 减运算
* \_\_mul\_\_: 乘运算
* \_\_div\_\_: 除运算
* \_\_mod\_\_: 求余运算
* \_\_pow\_\_: 乘方

#### 运算符重载
* Python同样支持运算符重载，我们可以对类的专有方法进行重载


```python
class Vector:
    def __init__(self, a, b):
        self.a = a
        self.b = b
 
    def __str__(self):
        return 'Vector (%d, %d)' % (self.a, self.b)
   
    def __add__(self,other):
        return Vector(self.a + other.a, self.b + other.b)

v1 = Vector(2,10)
v2 = Vector(5,-2)
print (v1 + v2)#重载就是正在做这个类中重新定义一种运算符计算方法
```

    Vector (7, 8)
    
