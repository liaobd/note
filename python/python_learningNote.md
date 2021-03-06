## Python的数据结构



### 列表



### 元组



### 字符串



### 序列



### 字典



### 集合











## 函数



### python函数的定义

python中函数的定义大概如下：

```
def func(arg...):
	function_body
	return value
```

在python中，定义一个函数使用关键字`def`，紧接着跟函数名、一对`()`，`()`里面跟着参数（可选的），之后就是一个`:`冒号，函数体是一个使用四个空格缩进的代码块，以及带有返回值的`return`语句（可选的）；下面分为3部分来讲解：

- 函数参数：**后面详细讲解**
- 函数体：被封装于这个函数里的代码，是一个使用四个空格缩进的代码块
- 返回值：在python里不需要定义函数的返回值类型，函数可以返回不同类型的值或组合，如果没有返回值（没有写`return`语句），则默认返回`None`
  - 没有返回值时，即没有写`return`语句，默认返回`None`
  - 返回一个值时，可以是任意类型，因此不需要显式表示返回值类型
  - 返回多个值时，默认以**元组**的形式对返回值**打包**，当然也可以使用别的容器对多个返回值进行打包再返回，如列表

**函数文档：**

给函数写的文档就叫做函数文档，其目的是说明函数的功能、参数意义、返回值以及一些调用的注意事项。

函数文档的举例如下：

```python
def functionalDoc(arg1, arg2):
    """
    功能：函数功能说明，必须使用三对双引号包起来
    参数：
    arg1：参数1说明
    arg2：参数2说明
    返回值：
    返回一个由两个值构成的元组
    """
    return arg1, arg1 + arg2
```

函数说明文档其意义类似于注释（python注释使用`#`开头），但却不等同于注释：

- 函数说明文档，也会当成函数的一部分被保存起来；
- 文档可以通过特殊属性获取：`__doc__`，如`print(functionalDoc.__doc__)`
- 在python命令行中可以通过`help(functionalDoc)`来获取文档

**类型注释**

首先展示代码：

- 函数参数里面，冒号`:`前面的是变量名，后面的是变量类型
- 参数外面带一个`->`这样的箭头，后面跟着的是返回值类型
- 这只是一个建议，python不会在语法层面上阻止调用者传入混乱的参数

```python
def func(s:str, n:int = 3) -> str:
    return s * n
```

**内省**

其实就是函数的信息，分门别类的展示，例如：

- `__doc__`：展示函数文档
- `__name__`：展示函数名字
- `__annotations__`：展示函数类型注释
- ...，小技巧：可在python控制台打出一个函数再加上成员运算符就知道有什么属性那么多了

### 函数的参数

首先什么是函数参数？

函数参数：函数调用者发送给函数的信息，参数有形式参数`parameter`和实际参数`argument`之分。

形式参数：简称形参，是函数定义中小括号里的参数。

实际参数：简称实参，函数在调用过程中传递进来的参数。

python函数的参数灵活多变，主要分为以下五种形式：

- 位置参数
- 关键字参数
- 默认参数
- 收集参数（可变参数）：`*args`
- 字典参数：`**kwargs`

**位置参数**

定义：把参数的名字、位置确定下来的参数，就叫做位置参数，它的特点是位置是固定的，对于函数调用者来说，**需要按照确定的顺序传入参数**

```python
def personal_info(name, age):
    print(f"{name}'s age is {age}")

# 函数调用时需要按照参数的位置填入正确的、对应意义的值
personal_info("Yookie", 26)

# 调用输出：Yookie's age is 26
```

位置参数存在的问题是，函数调用者不按照作者意图的参数顺序传入，则可能会引发意想不到的BUG，如上面的例子中假如是这么调用：

```python
personal_info(26, "Yookie")
# 调用输出：26's age is Yookie
```

程序不会报错，但是输出结果却是截然不同，根本不符合函数作者的预期。

位置参数存在的缺陷，有没有办法弥补呢？答案是有的，就是关键字参数！

**关键字参数**

定义：关键字参数就是在传入实参时，明确地指出了形参的变量名，那么就不存在参数的先后顺序的问题了！

```python
# 传入实参时明确地指出了形参的变量名，那么就不存在参数传入的顺序了，因此以下两条函数调用是等效的
personal_info(name="Yookie", age=26)
personal_info(age=26, name="Yookie")

# 调用输出：Yookie's age is 26
```

需要注意的是：**！！！**

- 可以位置参数和关键字参数混合使用的方式来进行函数调用

- 位置参数必须在关键字参数之前，否则程序会报错！！！

  ```python
  # 正确的调用
  personal_info("Yookie", age=26)
  
  # 错误的调用
  personal_info(name="Yookie", 26)
  # 报错信息：SyntaxError: positional argument follows keyword argument
  ```

**默认参数**

定义：函数定义时，为参数指定默认值就叫做函数默认参数，函数调用时，如果没有传递实参，则采用默认参数值

```python
def weather(date, temp, unit="C"):
    print(f"Today is {date}, temperature is {temp}{unit}")


# 使用默认参数
weather("2022.7.12", 28.2)
# 不使用默认参数
weather("2022.7.12", 84.8, "F")
```

默认参数和关键字参数结合使用，可以使得函数的调用变得更灵活

```python
# 使用默认参数
weather(date="2022.7.12", temp=28.2)
# 不使用默认参数
weather(temp=28.2, date="2022.7.12", unit="F")
```

位置参数、默认参数和关键字参数结合使用，需要注意在定义时必须确保位置参数在默认参数前面，否则会报错：

```python
# 该定义是错的，必须确保位置参数在默认参数前面
def weather(date, unit="C", temp):
```

> 补充知识：
>
> 打开python控制台，输入`help(abs)`，可以看到其输出为：
>
> ```
> abs(x, /)
>     Return the absolute value of the argument.
> ```
>
> 那么第二个参数的`/`是什么呢？
>
> 这个符号表示：
>
> - 左侧的参数必须传递位置参数
> - 右侧没有限制
>
> 那么自定义函数也可以用上这个符号去限定参数传入规则：
>
> ```python
> def abc(a, /, b, c):
>     print(a, b, c)
> 
> # 正确传入
> abc(1, 2, 3)
> abc(1, b=2, c=3)
> # 错误传入，报错：TypeError: abc() got some positional-only arguments passed as keyword arguments: 'a'
> abc(a=1, b=2, c=3)
> ```
>
> 那么除了符号`/`可以限制传入参数规则之外，还可以使用`*`
>
> `*`表示：
>
> - 左侧只能是位置或关键字参数
> - 右侧只能是关键字参数
>
> 例子如下：
>
> ```python
> def abc(a, *, b, c):
>     print(a, b, c)
>     
> # 正确的传入
> abc(1, b=2, c=3)
> abc(a=1, b=2, c=3)
> # 错误的传入，报错：TypeError: abc() takes 1 positional argument but 3 were given
> abc(1, 2, 3)
> ```

**收集参数**

定义：收集参数也叫做可变参数，即函数调用时传入的参数个数是不确定的、数量可变的；它的语法是：

`def test(*args):`，仅需在形参前面加一个`*`就表示该参数是可变参数、收集参数。

```python
def sum(*args):
    print(f"有{len(args)}个参数")
    print(f"这些参数是：{args}")
```

本质：python是把这些收集参数打包成一个**元组**，所以收集参数的本质就是一个元组！！

在函数定义中，收集参数前面的`*`星号起到的作用就是“打包”操作，即将传入的多个实参打包起来组成一个元组。

但是星号`*`在实参中的作用则恰恰相反，起到了解包的作用。

```python
# 函数定义中，*星号的作用是打包，即将传入的多个参数打包成一个元组
def func(*args):
    pass

# 定义一个元组
num = (1, 2, 3, 4, 5)
# 在函数调用中，*星号的作用是解包，将一个序列里的元素分解传给函数
func(*num)
# 解包操作也适用于其它序列类型
func("hello")
```

如果在收集参数后面还定义了其它参数，那么在调用函数的时候就应该使用关键字参数来指定了，否则python就会把实参都纳入到收集参数中。此外如果存在收集参数，建议使用默认参数，并且收集参数放在最前面。

```python
def regiter_info(*args, sex):
	pass

# 函数调用时，对于sex参数需要使用关键字参数来赋值
regiter_info("liao", 26, 175, 128, sex="男")
# 没有使用关键字参数来指定最后一个参数，导致python把实参都纳入到收集参数中，从而造成参数过少异常抛出
regiter_info("liao", 26, 175, 128, "男")
```

小结：

- 收集参数的`*`在函数定义中含义是打包，在函数调用中是解包
- 收集参数的本质是元组
- 函数定义中，若在收集参数后面还定义了其它参数，则需要使用关键字参数来进行函数调用，并且建议使用默认参数
- 收集参数应该放在定义的最前面

**字典收集参数**

定义：字典参数也是收集参数的一种，它的特点是：

- 使用两个星号`**`
- 传参时使用关键字参数的方式传参，等号`=`左边是键，右边是值

```python
def func(**kwargs):
    print(kwargs)
 
# 必须使用关键字参数形式传参
# 将所有关键字参数打包成字典
func(male="男", age="26", weight="128", height="174")
```

收集参数、字典参数的混合使用：注意事项！！！

### 变量的作用域

作用域：程序运行时，变量可被访问的范围。

**局部变量**

定义：定义在函数内部的变量就叫做局部变量，局部变量的作用域范围只能在函数内部生效，不能在函数外访问。

如下代码中，局部变量是`celisius`、`fahrenith`，全局变量是`newTemp`：

- 函数的形参是局部变量
- 在函数内部定义的变量是局部变量
- 试图在函数外访问局部变量，报错`NameError`

```python
def temperature(celisius):
    fahrenith = celisius * 1.8 + 34
    return fahrenith

newTemp = temperature(33.2)
print(newTemp)

# 试图在外面访问局部变量，会报错：NameError: name 'fahrenith' is not defined
print(fahrenith)
```

**全局变量**

定义：在函数外面定义的变量是全局变量，全局变量拥有更大的作用域，因此可以在函数中访问到他们

如下代码，全局变量是`oldTemp`、`newTemp`

- 全局变量拥有更大的作用域，因此可以在函数内部访问到它

- 试图在函数内部修改一个全局变量，不会成功

  > 这是python的保护机制，防止全局变量被随意修改，python做的处理是创建一个新的局部变量替代全局变量，这个局部变量的名字与全局变量的名字相同，因此全局变量的值不会改变

```python
oldTemp = float(input("请输入温度："))

def temperature(celisius):
    # 在函数内部试图修改全局变量 oldTemp
    oldTemp = 35.0
    print(f"old temperature is {oldTemp}")
    fahrenith = celisius * 1.8 + 34
    return fahrenith

# 全局变量并没有改动
print(f"old temperature is {oldTemp}")
newTemp = temperature(oldTemp)
print(newTemp)
```

那么有没有办法在函数内部修改全局变量呢？

使用`global`关键字

```python
count = 5

def func():
    # 使用 global 修饰全局变量 count，使得它在函数内部可以被修改
    global count
    count = 10

# 修改成功
func()
print(count)
```

**内嵌函数**

python的函数定义是支持嵌套的，即在定义函数时，允许在函数内部再定义另一个函数，这种函数称为内部函数。

注意事项：

- 内部函数的作用域仅在外部函数之内，即只有在外部函数体里才可以调用内部函数，在外面调用是报错的
- 内部函数可以引用外部函数的局部变量

```python
def func_outter():
    print("outter...")
    
    # 内部函数的局部变量
    x = 1024
    
    # 在函数内部定义另一个函数
    # 此时func_inner函数就称为内部函数
    def func_inner():
        print("inner...")
        # 内部函数可以引用外部函数的局部变量
        print(x)
    
    func_inner()

func_outter()
# 只有在外部函数体里才可以调用内部函数，在外面调用是报错的：NameError: name 'func_inner' is not defined
func_inner()
```

**LEGB原则**

对于名字一样，作用域不同的变量引用，python引入了LEGB原则进行规范：

- Local：函数内的命名空间
- Enclosing function locals：嵌套函数中外部函数的命名空间
- Global：函数定义所在模块的命名空间（文件）
- Builtin：python内置模块的命名空间

变量查找的顺序是：L --> E --> G --> B

```python
# G
x = 520

def func_outter():
    # E
    x = 250
    
    def func_inner():
        # L
        x = 110
        print(x)
	
    func_inner()

func_outter()
```

### 闭包

闭包是啥？背景：

根据前面的内嵌函数知道，想要调用内层函数，只能通过外层函数来间接调用，那有没有什么办法可以直接调用呢？可以通过将内层函数当作返回值返回，在外面使用一个变量绑定起来，就实现了直接调用。

内层函数可以直接访问外层函数作用域下的变量，当想要修改时可以使用关键字`nonlocal`来修饰，再结合前面的将内层函数作为返回值返回时，外层函数作用域的一些信息就被保存下来了。

利用以上两个特点，就实现了闭包！

python中闭包的形式为：在嵌套函数中，内部函数引用了外部函数的局部变量，再将内部函数作为返回值返回，那么内部函数就会被认为是闭包。

```python
# python中一个闭包的表现形式
def func_outter(x):
    # x是外部函数的局部变量
    
    def func_inner(y):
        # 由于内部函数引用了 x
        return x * y
    
    # 将内部函数作为返回值返回
    return func_inner
```

在闭包中（内部函数），外部函数的局部变量对应于内部函数，其实就相当于全局变量与局部变量的关系，因此在内部函数中，依然只能访问外部函数的局部变量，但是不能修改。

但是如果希望可以在内部函数中修改外部函数的局部变量，可以使用`nonlocal`关键字修饰这个变量

```python
def func_outter():
    # 外部函数的局部变量
    x = 5
    
    def func_inner():
        # 内部函数中使用nonlocal关键字修饰，就可以修改外部函数的局部变量了
        nonlocal x
        x = x + 41
        return x
    
    return func_inner

# 将临时变量temp与内部函数绑定起来，实现了内部函数的直接调用
temp = func_outter()
temp()
```

闭包的作用：

- 将一些特定功能的信息隐藏起来，使得只能通过闭包去访问
- 具有良好的封装性

案例：游戏角色移动开发

```python
def creat(pos_x=0, pos_y=0):
    def pos_cal(dir, step):
        nonlocal pos_x, pos_y
        pos_x = pos_x + dir[0] * step
        pos_y = pos_y + dir[1] * step
        print(pos_x, pos_y)
    return pos_cal

move = creat()
move([1, 0], 50)
move([-1, 1], 25)
```

该案例中，通过闭包将角色的位置信息隐藏起来了，使得只有通过绑定的`move`才能修改访问。

### 装饰器decorator

装饰器是什么？

被装饰函数？

背景：

- 一个已经完成基本功能的函数，现在需求变更了，需要增加一些功能，如何在不改变原函数的情况下将功能新增进去呢？
- 将完成基本功能的函数当成被装饰函数，将完成补充功能的函数叫做装饰器
- 将被装饰函数当作参数传给装饰器，装饰器利用闭包的原理，再把加工后的被装饰函数返回

```python
# 装饰器
def log(func):
    # 装饰器的业务核心
    def wrapper():
        print("start log...")
        func()
        print("end log...")
    # 返回被包装之后的被装饰函数
    return wrapper

# 被装饰函数
def eat():
    print("eating...")

# 业务升级处理之前
eat()

# 业务升级处理之后
eat = log(eat)
eat()
```

语法糖(syntactic sugar)：在计算机语言中添加某种语法，这种语法对功能没有影响，但是更方便程序员使用。

因此利用"@语法糖"，对上面程序中`eat = log(eat)`的调用进行处理，可以使得程序更简洁，可读性更高，如下：

```python
# 装饰器
def log(func):
    # 装饰器的业务核心
    def wrapper():
        print("start log...")
        func()
        print("end log...")
    # 返回被包装之后的被装饰函数
    return wrapper

#  利用语法糖实现装饰器
@log
def eat():
    print("eating...")
    
eat()
```

这样就省去了手动将函数传递给包装器再返回来重新赋值的步骤了。

多个装饰器应用在同一个函数上：调用顺序是`add(cube(square(cal)))`，即从下往上调用

```python
def add(func):
    def add_num():
        x = func()
        return x + 1
    return add_num

def cube(func):
    def cube_num():
        x = func()
        return x * x * x
    return cube_num

def square(func):
    def square_num():
        x = func()
        return x * x
    return square_num

@add
@cube
@square
def cal():
    return 2

print(cal())
```

装饰器如何传递参数：

```python
import time

def log(msg):
    def time_master(func):
        def call_fun():
            start = time.time()
            func()
            stop = time.time()
            print(f"{msg}一共花费了{(stop - start):.2f}")
        return call_fun
    return time_master

@log(msg="线程")
def thread():
    time.sleep(1)
    print("执行完毕")
    
thread()
```

### 函数式编程

函数式编程的概念？

**lambda表达式**

一句话代表一个函数，例子如下：

```python
# lambda表达式
func_lamb = lambda x : 2 * x + 1

# 调用
func_lamb(3)

# 等价于
def func(x):
    return 2 * x + 1

# 调用
func(3)
```

lambda表达式创建了一个匿名函数，需要用一个变量来接住这个匿名函数才能调用，其语法如下：

- 第一个单词是`lambda`
- 使用冒号分隔参数和返回值，冒号`:`左边是参数，如果有多个参数，使用逗号`,分隔；右边是返回值
- 需要一个临时的变量来绑定返回的匿名函数对象，才能调用

### 生成器

生成器的概念，生成器推导式





### 高阶函数





















## 异常处理


























## 类和对象



### 类的概念和封装

Python中处处皆是对象，一切皆是对象，包括我们平时用的`int`、`float`等等看似普通的数据类型，其实也是对象，证据如下：

- 输入`type(int)`，输出得到：`<class 'type'>`，输入`x = 500, type(x)`，输出得到`<class 'int'>`，说明哪怕是我们的`int`也是一个`class`
- 自定义一个类，使用`type`来观察，如输入`type(Myclass)`，输出得到`<class 'type'>`，输入`instance = Myclass(), type(instance)`，输出得到`<class '__modulename__.Myclass'>`
- 以上说明，我们用`type`来判断时，如果参数是类型名，输出是`class type`，如果参数是实例名，输出是`class 类型`

面向对象是代码的一种封装形式，面向对象首先要设计一个对象的类，这个类封装了对象的属性和行为（方法），创建一个类使用关键字`class`，类名一般约定俗成使用大写字母开头，如下创建了一个鱼类：

```python
class Fish:
    pass
```

同一个类创建出来的对象是否共享数据？

属性独立，方法共享。即：

- 同一个类实例化出来的对象，他们的属性是独立的，互不干扰
- 方法是共享的，属于类的，任何对象都可以使用他们

既然方法共享，那么方法是如何识别哪个对象在调用自己呢？

答案是使用`self`变量，如下代码所示：

```python
class Duck:
    legs = 2
    mouth = 1
    
    def swim(self):
        print(f"My name is {self}")
        print("I'm swimming~")
```

运行代码，可以看到：

```python
d1 = Duck()
d2 = Duck()
```

![1657865060945](重构.assets/1657865060945.png)

`d1`和`d2`的属性是互相独立的，彼此互相不干扰。

运行代码，可以看到：

```python
d1.swim()
d2.swim()
```

![1657865463571](重构.assets/1657865463571.png)

虽然都运行同一个方法，但是`self`各自不一样，说明python是通过`self`变量来区分到底是哪个对象调用方法的，并且对象里面带了决定自己属性的所有信息。

那么现在搞清楚了`self`是实例对象本身，它的作用是在函数调用时用来绑定对象和方法。

那么如果类中的方法不带形参`self`，此时这个方法就属于这个类的，不能被任何对象调用，只能通过类名来调用。

一个对象可以动态的创建属性，如以下代码和现象：

```python
d1.eyes = 2
```

![1657865698064](重构.assets/1657865698064.png)

只有对象`d1`创建了属性`eyes`，对象`d2`没有，使用`dir`函数可以查看对象的属性，如`dir(d1)`

**总结：**

1. 属性：要分得清对象间的，也要分得清类和对象的，比如：

   ```python
   class A:
       a = 1
       b = 2
       s = "jk"
   
   o1 = A()
   o2 = A()
   ```

   通过`__dict__`来查看：

   ![1657869686049](重构.assets/1657869686049.png)

   得到结论如下：虽然创建了实例对象，但是实例对象里没有属于自己的属性，那些属性都属于类的，只不过是对象可以借用而已，但是当运行`o1.a = 100`时得到如下：

   ![1657869804312](重构.assets/1657869804312.png)

   可以发现，`o1`有了一个属性了，`o2`还是空的，因此只要修改类的属性，就可以把类属性转移到自己身上来，但我猜测实际上不是转移，而是对象它自己动态创建了一个属性，只不过和类的属性同名而已。

2. `self`实际上就是对象本身

### 继承和组合

继承的语法：

```python
class A:
    pass

class B(A):
    pass
```

可以看到继承只需要加`()`，并在里面写上父类名即可。

假如父类和子类的属性、方法同名了怎么办？

子类覆盖父类，如下：

```python
class A:
    x = 10
    def hello(self):
        print("A say hello")

class B(A):
    x = 1000
        
b = B()
b.x --> 1000
b.hello() --> A say hello
```

可以看到，访问`b`对象中的同名属性时，是直接读取`b`对象的，覆盖了父类的属性，而访问方法时，由于B没有该方法，因此是访问了从A继承过来的方法。

多重继承，语法如下：

```python
class A:
    pass

class B():
    pass

class C(A, B):
    pass
```

多继承和单继承概念基本上差不多，属性和方法中有同名的现象时，对象访问这些同名成员的顺序是：对象本身的 --> 继承时从左往右继承过来的父类的（如：--> A --> B)

两个有用的BIF函数：

- `isinstance(对象， 类)`：用于判断参数中对象是否属于类的
- `issubclass(子类， 父类)`：用于判断参数中子类是否属于父类的

组合：

一个类中的属性成员是另一个类的对象，即多个别的类的实例对象构成了这个类，如下：

```python
class Turtle():
    pass

class Dog():
    pass

class Cat():
    pass

# 三个类的示例对象构成了Garden类
class Garden():
    t = Turtle()
    d = Dog()
    c = Cat()
```

### 构造函数

构造函数：`__init__()`

作用：

- 实例化对象时会自动调用的
- 实例化对象的同时实现个性化定制，这样创建出来的对象就拥有属于自己的属性

```python
class C:
    # 构造函数，这样实例化对象时对象就拥有了x，y这两个属性，但是类不会带有这两个属性
    # 可以用dir函数或__dict__属性去验证
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def add(self):
        return self.x + self.y
    def mul(self):
        return self.x * self.y
```

继承中发生的重写行为：

继承发生时，对类中的同名属性、方法的覆盖行为就叫做重写

```python
class D(C):
    def __init__(self, x, y, z):
        # 这样的调用叫做未绑定对象的方法调用，需要显示传入self进去
        # 通过调用父类的构造函数来初始化对象的x，y属性，然后再单独初始化属性z
        C.__init__(self, x, y)
        self.z = z
    def add(self):
        # 同理，通过调用父类的add函数来计算一部分值，再将返回值和新增的值进行计算
        return C.add(self) + self.z
    def mul(self):
        return C.mul(self) * self.z
```

砖石继承：

- 可以看到，子类B1、B2继承了A类，并且调用A的构造函数来初始化自己的对象
- 子类C继承了B1、B2，同时调用了B1、B2的构造函数来初始化自己的对象

```python
class A:
    def __init__(self):
        print("A .......")
        
class B1(A):
    def __init__(self):
        A.__init__(self)
        print("B1 .......")
        
class B2(A):
    def __init__(self):
        A.__init__(self)
        print("B2 .......")
        
class C(B1, B2):
    def __init__(self):
        B1.__init__(self)
        B2.__init__(self)
        print("C ............")
```

会造成什么后果呢？

![1657875529047](重构.assets/1657875529047.png)

可以看到A的构造函数被调用了两次，从代码上能理解，但这不是我们想要的结果。

怎么解决这个问题？使用`super()`函数，这个函数能在父类中自动搜索指定的方法，并自动绑定好`self`参数。

如以下代码所示：

```python
class A:
    def __init__(self):
        print("A .......")
        
class B1(A):
    def __init__(self):
        super().__init__()
        print("B1 .......")
        
class B2(A):
    def __init__(self):
        super().__init__()
        print("B2 .......")
        
class C(B1, B2):
    def __init__(self):
        super().__init__()
        print("C ............")
```

使用mro函数可以看到类的继承关系：

![1657876783347](重构.assets/1657876783347.png)



































