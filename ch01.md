# 第一章 用pythonic方式来思考

## python 2移植到python3

- 2to3,[six](https://pythonhosted.org/six/)等工具可以帮助大家把代码轻松地适配到python3及其后续版本上面

## python运行时环境

- CPython,Jython,IronPython以及PyPy等

## 遵循PEP8风格指南(8号python增提案)

- 使用空格来表示缩进,而不要用tab(制表符)
- 和语法相关的每一层缩进都用4个空格来表示
- 每行的字符数不应超过79
- 对于占据多行的表达式来说,除了首行之外,其余各行都应该在通常的缩进级别上再加上4个空格
- 文件中的函数与类之间应该用两个空行隔开
- 在同一个类中,各方法之间应该用一个空行隔开
- 在使用下标来获取列元素,调用函数或给关键字参数赋值的时候,不要在两旁添加空格
- 为变量赋值的时候,赋值符号在左侧和右侧应该各自写上一个空格,而且只写一个就好.

### 命名

> PEP8提倡采用不同的命名风格来编写python代码中的各个部分,以便在阅读代码时可以根据这些名称看出它们在python语言中的角色.

- 函数,变量及属性应该用小写字母来拼写,各单词之间以下划线连接.如lowercase_underscore
- 受保护的实例属性,应该以单个下划线开头,例如,\_leading\_underscore
- 私有的实例属性,应该以两个下划线开头,例如,_\_double\_leading\_underscore
- 类与异常,应该以每个单词首字母均大写的形式来命名,例如,CapicalizedWord
- 模块级别的常量,应该全部采用大写字母来拼写,各单词之间以下划线相连,例如,ALL_CAPS
- 类中的实例方法(instance method),应该把首个参数命名为self,以表示该对象自身
- 类方法(class method)的首个参数,应该命名为cls,以表示该类自身

### 表达式与语句

- 采用内联形式的否定词,而不要把否定词放在整个表达式的前面,例如,应该写if a is not b
- 不要通过检测长度的方法(如if len(somelist) == 0)来判断somelist是否为[]或''等空值,而是采用if not somelist这种写法来判断,它会假定:空值将自动评估为False
- 检测somelist是否为[1]或'hi'等非空值时,亦应该如此,if somelist语句默认会把非空的值判断为True
- 不要编写单行的if语句,for循环,while循环及except复合语句,而是应该把这些语句分成多行来书写,以示清晰
- import语句应该总是放在文件头
- 引入模块时,总是应该使用绝对名称,而不应该根据当前模块的路径来使用相对名称,例如,引入bar包中的foo模块时,应该完整地写出from bar import foo,而不应该写import foo
- 如果一定要以相对名称编写import语句,那就采用明确的写法: from.import foo
- 文件中的这些import语句应该按顺序划分成三个部分,分别表示标准库模块,第三方模块以及自用模块.在每一部分中,各import语句应该按模块的字母顺序来排列.

> [pylint](http://www.pylint.org/)是一款流行的python源码静态分析工具.它可以自动检查受测代码是否符合PEP8风格指南,而且还能找出Python程序里的多种常见错误.

### bytes,str和Unicode的区别

- python3有两种表示字符序列的类型:bytes和str.前者的实例包含原始的8位值,后者的实例包含Unicode字符.
- python2有两种表示字符序列的类型:str和Unicode.str的实例包含原始的8位值,而Unicode实例则包含Unicode字符.

**把Unicode字符表示成二进制数据(也就是原始8位值)有许多种方法.最常见的编码方式就是UTF8.要想把Unicode字符转换成二进制数据,就必须使用encode方法.反之则使用decode方法.**

> python代码中经常会出现两种常见的使用情境:
>
> - 开发者需要原始8位值,这些8位值表示以UTF8格式(或其他编码形式)来编码的字符
> - 开发者需要操作没有特定编码形式的Unicode字符
>
> 编写两个辅助(helper)函数,以便在这两种情况之间转换.(参见data)

常见问题:

- [python2]如果str只包含7位的ASCII字符,那么Unicode和str实例似乎成了同一种类型.可以用+操作符把这种str与Unicode连接连接.可以用等价与不等价操作符,在这种str实例与Unicode实例之间进行比较.
- [python2]在格式化字符串中,可以用'%s'等形式来代表unicode实例

#### 总结

- 二进制字符串和Unicode字符串不能通过>和+等操作符来混同操作
- 从文件中读取二进制数据,或向其中写入二进制数据时,总应该以'rb'或'wb'等二进制模式来开启文件

### 用辅助函数来取代复杂的表达式

- 开发者很容易过度运用python的语法特性,从而写出那种特别复杂并且难以理解的单行表达式
- 把复杂的表达式移入辅助函数之中,如果要反复使用相同的逻辑,更应该这么做
- 使用if/else表达式,要比用or或and这样的boolean操作符写成的表达式更加清晰.

### 了解切割序列的方法

- 从列表开头获取切片,不要在start处写上0,应写成a[:5]
- 切片一直要取到列表末尾,把end留空,如a[5:]
- 由于切片操作越界访问也没有问题,所以可以通过限定访问元素的个数来避免越界访问,如获取开头的20个元素a[:20],或者获取末尾的20个元素a[-20:]
- 单个元素进行访问时,会存在越界访问报错问题.    
- 切片会产生新的对象
- 对list赋值的时候,如果被赋值的是使用切片操作的变量,那么赋值操作就是把源列表中的切片操作指定的位置替换成新的值.如b=a,a[:] = [1,2,3], 这个时候,b is a依然成立.

### 在单次切片操作内,不要同时指定start,end和stride

somelist\[start:end\:stride]

```python
#偶数
a[::2]
#奇数
a[1::2]
#反转
a[::-1]
```

- 既有start和end,又有stride的切割操作,可能会令人费解
- 尽量使用stride为正数,且不带start或end索引的切割操作.尽量避免用负数做stride
- 在同一个切片操作内,不要同时使用start,end和stride.如果确实需要执行这种操作,那就考虑将其拆解为两条赋值语句,其中一条做范围切割,另一条做步进切割,或考虑使用内置itertools模块的islice(这个是什么?)

### 用列表推到来取代map和filter

- 列表推导要比内置的map和filter函数清晰,因为它无需额外编写lambda表达式
- 列表推导可以跳过输入列表中的某些元素,如果改用map来做,那就必须辅以filter方能实现

### 不要使用含有两个以上的表达式的列表推导

- 列表推导支持多级循环,每一级循环也支持多项条件

  - filtered = [[x for x in row if x%3==0] for row in matrix if sum(row) >= 10]

- 超过两个表达式的列表推导是很难理解的,应该尽量避免

  > flat = []
  >
  > for sublist1 in my_lists:
  >
  > ​	for sublist2 in sublist1:
  >
  > ​		flat.extend(sublist2)

### 用生成器表达式来改写数据量较大的列表推导

> 列表推导缺点:在推导过程中,对于输入序列中的每个值来说,可能都要创建仅含一项元素的全新列表.当输入的数据比较少时,性能ok.但是大数量的时候,则会出现问题:
>
> 如:输出一个很大的文件的每行的字数value = [len(x) for x in open('/tmp/my_file.txt')]

解决大数据量问题,使用python生成器表达式:

```python
#区别于列表推导式,最外层使用圆括号而不是方括号
it = (len(x) for x in open('/tmp/my_file.txt'))
#一次获取每行的字数
print(next(it))
#生成器可以相互组合
roots = ((x, x**0.5) for x in it)
print(next(roots))
```

- 当输入的数据量较大时,列表推导可能会因为占用太多内存而出问题.
- 由生成器表达式所返回的迭代器,可以逐次产生输出值,从而避免了内存用量问题
- 把某个生成器表达式所返回的迭代器,放在另一个生成器表达式的for 子表达式中,即可将二者组合起来
- 串在一起的生成器表达式执行速度很快

### 尽量用enumerate取代range

- enumerate函数提供了一种精简的写法,可以在遍历迭代器时获知每个元素的索引
- 尽量用enumerate来改写那种将range与下标访问相结合的序列遍历代码
- 可以用enumerate提供第二个参数,以指定开始计数时所用的值(默认为0)

### 用zip函数同时遍历两个迭代器

- 内置zip函数可以平行地遍历多个迭代器
- python3中的zip相当于生成器,会在遍历过程中逐次产生元组,而python2中的zip则是直接把这些元组完全生成好,并一次性地返回整份列表.
- 如果提供的迭代器长度不等,那么zip就会自动提前终止
- itertools内置模块中的zip_longest函数可以平行地遍历多个迭代器,而不用在乎它们的长度是否相等

### 不要在for和while循环后面写else块

```python
for i in range(3):
    print('loop %d' % i)
else:
    #在for循环正确执行结束后,执行else语句;如果for循环通过break跳出,则不执行else语句
    print('Else block!')
    
'''
Loop 0
Loop 1
Loop 2
Else block!
'''
```

- python有种特殊语法,可在for及while循环的内部语句块之后紧跟一个else块
- 只有当整个循环主体都没遇到break语句时,循环后面的else块才会执行
- 不要在循环后面使用else块,因为这种写法既不直观,又容易引人误解

### 合理利用try/except/else/finally/结构中的每个代码块

``` python
#finally块
'''如果既要将异常向上传播,又要在异常发生时执行清理工作,就可以使用try/finally结构;常见用途是确保程序能够可靠地关闭文件句柄.在这段代码中,read方法所抛出的异常会向上传播给调用方.'''
handle = open('/tmp/random_data.txt') #May raise IOError
try:
    data = handle.read()  #May raise UnicodeDecodeError
finally:
    handle.close()  #Always runs after try
    
#else块
'''
try/except/else结构可以清晰的描述出哪些异常会由字的代码来处理,哪些异常会传播到上一级.
如果try块没有发生异常,那么就执行else块
'''
def load_json_key(data, key):
    try:
        result_dict = json.loads(data)  #May raise ValueError
    except ValueError as e:
        raise KeyError from e
    else:
        return result_dict[key] #May raise KeyError,else语句块出现异常时,异常会向上传播到except语句块进行处理
    
#混合使用
UNDEFINED = object()

def divide_json(path):
    handle = open(path, 'r+') #May raise IOError
    try:
        data = handle.read()    #May raise UnicodeDecodeError
        op = json.loads(data)    #May raise ValueError
        value = (op['numerator']/op['denominator'])  #May raise ZeroDvisionError
    except ZeroDivisionError as e:
        return UNDEFINED
    else:
        op['result'] = value
        result = json.dumps(op)
        handle.seek(0)
        handle.write(result)  #May raise IOError
        return value
    finally:
        handle.close()  #Always runs
```

- 无论try块是否发生异常,都可利用try/finally符合语句中的finally块来执行清理工作
- else块可以用来缩减try块中的代码量,并把没有发生异常时所要执行的语句与try/except代码块隔开
- 顺利运行try块后,若想使某些操作能在finally块的清理代码之前执行,则可将这些操作写到else块中.

