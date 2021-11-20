# ***Python基础语法***
## **标识符**
- 第一个字符必须是字母表中字母或下划线 _ 。
- 标识符的其他的部分由 *字母*、*数字* 和 *下划线* 组成。
- 标识符对*大小写* 敏感。
## **关键字**
关键字及保留字，我们不能把它们用作任何标识符名称

Python的标准库提供的 **keyword** 模块，可以输出当前版本的所有关键字：

```python
import keyword
print (keyword.kwlist)
```
以下都是Python的关键字，定义变量或函数的时候是不可以使用这些名字的
```python
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

## **注释**
Python中单行注释以 `#` 开头，实例如下：
```python
#输出 Hello，Python
print ("Hello，Python")
```
执行以上代码，输出结果为：
> Hello, Python!

多行注释可以用多个 `#` 号，还有 `'''` 和 `"""`：
```python
# 第一个注释
# 第二个注释
 
'''
第三注释
第四注释
'''
 
"""
第五注释
第六注释
"""
print ("Hello, Python!")
```
执行以上代码，输出结果为：
> Hello, Python!

## **代码块**
Python最具特色的就是使用缩进来表示代码块，不需要使用花括号 `{}` ，而是用冒号 `:`。

缩进的空格数是可变的，但是 **同一个代码块的语句必须包含 相同的缩进空格数**。实例如下：
```python
if True:
    print ("True")
else:
    print ("False")
```


缩进数的空格数不一致，会导致运行错误：
```python
if True:
    print ("Answer")
    print ("True")
else:
    print ("Answer")
  print ("False")    # 缩进不一致，会导致运行错误
```
以上程序由于缩进不一致，执行后会出现类似以下错误：
```python
File "test.py", line 6
    print ("False")   # 缩进不一致，会导致运行错误
												^
IndentationError: unindent does not match any outer indentation level
```

## **多行语句**
Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠 \ 来实现多行语句，例如：
```python
print('total = item_one + \
        item_two + \
        item_three')
```

多条语句之间使用分号 `;` 分割，以下是一个简单的实例：
```python
print('hello'); print("world")
```

## **数字（Number）类型**
Python中数字有四种类型：整数、布尔型、浮点数和复数。

- int (整数), 如 1、2
- bool (布尔), 如 True、False。（要么是1，要么是0）
- float (浮点数), 如 1.23、3E-2
- complex (复数), 如 1 + 2j、 1.1 + 2.2j
- None (空值)

**布尔值**
一个布尔值只有True、False两种值，要么是True（1），要么是False（0）；在Python中，可以直接用True、False表示布尔值（请注意大小写），也可以通过布尔运算计算出来：

> print (3 > 1 )
True
> print (3 > 5)
False

布尔值可以用<kbd>and</kbd>、<kbd>or</kbd>和<kbd>not</kbd>运算。

`and`运算是**与**运算，只有所有都为True，and运算结果才是True：
```python
>>> True and True
True
>>> True and False
False
>>> False and False
False
>>> 5 > 3 and 3 > 1
True
```

<kbd>or</kbd>运算是**或**运算，只要其中有一个为True，or运算结果就是True：

```python
>>> True or True
True
>>> True or False
True
>>> False or False
False
>>> 5 > 3 or 1 > 3
True
```

<kbd>not</kbd>运算是**非**运算，它是一个单目运算符，把True变成False，False变成True：

```python
>>> not True
False
>>> not False
True
>>> not 1 > 2
True
```

布尔值经常用在条件判断中，比如：
```python
if age >= 18:
    print('adult')
else:
    print('teenager')
```

**复数**
复数是由一个实数和一个虚数组合，表示为：x+yj

*虚数部分必须有后缀 j 或 J*
```python
aa=123-12j
print (aa.real)  # output 实数部分 123.0  
print (aa.imag)  # output虚数部分 -12.0 (imaginary number)
```
输出结果为：
> 123.0<br>
> -12.0

**空值**
空值是Python里一个特殊的值，用None表示。None不能理解为0，因为0是有意义的，而None是一个特殊的空值。

## **查看数据类型**
使用Python内置函数 `tpye()` 查看数据类型，如：
```python
print (type("hi"))
print (type(123))
print (type(3.14))
print (type(False))
print (type(1 + 2j))
```

```
<class 'str'>	     # 字符串
<class 'int'>		 # 整数	
<class 'float'>		 # 浮点
<class 'bool'>		 # 布尔
<class 'complex'>	 # 复数
```

## **字符串（String）**
- 用*单引号* 和*双引号* 使用完全相同
- 用三引号`(''' 或 """)`可以指定一个多行字符串。
- 转义符 \\，如：
	+ \r  **回车**
	+ \n  **换行**
	+ \t  **制表符**
	+ ......
- 反斜杠可以用来转义，使用 `r` 可以让反斜杠不发生转义。如:
	```python
	print (r"this is a line with \n") #则\n会显示，并不是换行。
	```
- 字符串连接，有以下几种方法：
	```python
	print ("this" "is" "string") 	# 第一句

	print ("this" + "is" + "string") # 第二句

	print ("this","is","string") 	# 逗号，会有一个空格
	```
	输出结果：
	> thisisstring &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;前两句输出一样<br>
	> this is string&nbsp;&nbsp;&nbsp;&nbsp;最后一句输出结果

- 字符串用 `*` 运算符重复，如：
	```python
	print ("You are lovely" * 3) # 将会输出三次‘You are lovely’
	```

- Python 中的字符串有两种索引方式，**从 左往右 以 0 开始，从 右往左 以 -1 开始。**
- 字符串的截取的语法格式如下：变量`[头下标:尾下标:步长]` 左边开区间，右边闭区间
	```python
	str='123456789'
 
	print (str)                 # 输出 字符串
	print (str[0])              # 输出 第一个字符
	print (str[2:5])            # 输出 从第三个开始 到 第五个的字符
	print (str[4:])             # 输出 从第五个开始 到 最后一个
	print (str[0:-3])           # 输出 第一个 到 倒数第四个 中间的字符
	print (str[1:8:2])		   # 输出 第二个 到第八个，但是隔一个来执行的
	```
	输出结果：
	> 123456789<br>
	> 1<br>
	> 345<br>
	> 56789<br>
	> 123456<br>
	> 2468
	
- Python中的字符串不能改变。
	```python
	str = 'hello'

	str[0:2] = "hi"		# 我想将“he” 变成 “hi”
	print(str)
	```
	报错了噢！因为字符串是不可变性

	Python中的 数字、字符串、元组是不可变的；列表和字典可以完全自由地改变。后面我会讲到，你别急！
	```python
	Traceback (most recent call last):
		File "test.py", line 3, in <module>
			str[0:3] = "hi"
	TypeError: 'str' object does not support item assignment
	```

## **print 输出**
print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 end=""：

print()会依次打印每个字符串，遇到逗号“,”会输出一个空格，因此，输出的字符串是这样拼起来的：

![](https://www.liaoxuefeng.com/files/attachments/1017032122300544/l)

print()也可以打印整数，或者计算结果：

```python
print (300)
```
> 300
 
```python
print (100 + 200)
```
> 300

因此，我们可以把计算100 + 200的结果打印得更漂亮一点：
```python
print ('100 + 200 =', 100 + 200)
```
> 100 + 200 = 300

**注意，对于100 + 200，Python解释器自动计算出结果300，但是，'100 + 200 ='是字符串而非数学公式，Python会把它视为字符串**

## **input() 等待用户输入**
执行下面的程序在按回车键后就会等待用户输入：
```python
input ("按下 enter 键后退出。")
```
以上代码中 ，一旦用户按下 enter 键时，程序将退出。

>input() 是 Python 的内置函数，用于从控制台读取用户输入的内容。<br>input() 函数总是以字符串的形式来处理用户输入的内容，所以用户输入的内容可以包含任何字符；当然返回值的类型也是字符串

```python
a = input ("按下 enter 键后退出。")
print (type(a))
```
执行结果：
```python
按下 enter 键后退出。
<class 'str'>
```

可以让用户输入字符串，并存放到一个变量里。比如输入用户的名字：
```python
name = input()
```

**什么是变量**

变量，可以理解为一个容器，就是用来存放东西的

有了输入和输出，我们就可以把上次打印 'hello, world' 的程序改成有点意义的程序了：

运行上面的程序，第一行代码会让用户输入任意字符作为自己的名字，然后存入name变量中；<br>第二行代码会根据用户的名字向用户说hello，比如输入 Jchan：
```python
name = input()
print('Hello,', name)
```
执行结果：
> Hello, Jchan

但是，没有任何提示信息告诉用户：“ 嘿，赶紧输入你的名字 ”，这样显得很不友好。幸好，input()可以让你显示一个字符串来提示用户，于是我们把代码改成：
```python
name = input('Please enter your name: ')
print('Hello,', name)
```

再次运行这个程序，你会发现，程序一运行，会首先打印出 `Please enter your name:` ，这样，用户就可以根据提示，输入名字后，得到 `Hello, xxx` 的输出：
```python
Please enter your name: Jchan
Hello, Jchan
```
每次运行该程序，根据用户输入的不同，输出结果也会不同。


## **import 与 from...import**
在 Python 用 import 或者 from...import 来导入相应的模块。

将整个模块(somemodule)导入，格式为： `import somemodule`

从某个模块中导入某个函数,格式为： `from somemodule import somefunction`

从某个模块中导入多个函数,格式为： `from somemodule import firstfunc, secondfunc, thirdfunc`

将某个模块中的全部函数导入，格式为： `from somemodule import *`