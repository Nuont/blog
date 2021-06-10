---
title: Python基础_函数
date: 2021-06-06 15:00:42
author: L
summary: 基础-函数
categories: python
tags:
  - Python
---

## 函数

- 作用：封装代码，高效的代码重用函数就是将一段具有独立功能的代码块整合到一个整体并命名，在需要的位置调用这个名称即可完成对应的需求。

- 步骤：先定义后调用（函数代码块以 def 关键词开头，后接函数标识符名称和圆括号 ()）

- 参数：函数调用的时候可以传入真实数据，增大函数的使用的灵活性

  - 形参：函数定义时书写的参数(非真实数据)

  - 实参：函数调用时书写的参数(真实数据)

  - 位置参数

    1. 调用函数时根据函数定义的参数位置来传递参数
    2. 传递和定义参数的顺序及个数必须一致

  - 关键字参数

    - 函数调用，通过“键=值”形式加以指定。可以让函数更加清晰、容易使用，同时也清除了参数的顺序需求。
    - 函数调用时，如果有位置参数时，位置参数必须在关键字参数的前面，但关键字参数之间不存在先后顺序。

  - 不定长参数 *args 和  **kwargs

    -  *args：

      ```python
      # 前面加个 * ，表示为可变参数，其中 * 是规定的，args可用其他名称替换，但一般习惯用 args 来表示。可变参数在传入函数后，被封装成一个 tuple(元祖)来进行使用
      def numbers(*args):
          print(type(args))  # tuple
          for n in args:
              print(type(n))   # int
      numbers(1, 2, 3, 4)
      ```

    - **kwargs

      ```python
      # 是一个字典，传入的参数以键值对的形式存放到字典里。
      def name(**kwargs):
          print(kwargs)
      
      name(a = 1, b = 2, c = 3)
      name(a = 1, b= 2, c = 3, d = 4)
      ```

- 变量作用域

  - 全局：函数体内外都能生效(global)
  - 局部：当前函数体内部生效(在函数体内部，临时保存数据，即当函数调用完成后，则销毁局部变量。)

- 递归函数

  ```python
  # 函数内部自己调用自己
  # 必须有出口
  def fact(n):
      if n==1:
          return 1
      return n * fact(n - 1)
  ```

- 拆包

  - 对于可迭代对象，如元组、列表、字符串、集合、字典这些可迭代对象都可以被拆包，拆包是指将一个结构中的数据拆分为多个单独变量中

    ```python
    # 以变量的方式来接收
    # 用‘*’号
    def return_num():
        return 100, 200
    
    num1, num2 = return_num()
    print(num1)  # 100
    print(num2)  # 200
    ```

-  lambda 表达式

  - 如果一个函数有一个返回值，并且只有一句代码，可以使用 lambda简化

    ```python
    lambda 参数列表 ： 表达式
    ```

  - lambda表达式的参数可有可无，函数的参数在lambda表达式中完全适用。

  - lambda表达式能接收任何数量的参数但只能返回一个表达式的值。

    ```python
    # 函数
    def fn1():
        return 200
    
    print(fn1)
    print(fn1())
    
    # lambda表达式
    fn2 = lambda: 100
    print(fn2)
    print(fn2())
    ```

    > 注意：直接打印lambda表达式，输出的是此lambda的内存地址

  - 参数形式

    1. 无参数


    ```python
    fn1 = lambda: 100
    print(fn1())
    ```

    2. 一个参数

    ```python
    fn1 = lambda a: a
    print(fn1('hello world'))
    ```

    3. 默认参数

    ```python
    fn1 = lambda a, b, c=100: a + b + c
    print(fn1(10, 20))
    ```

    4. 可变参数：*args

    ```python
    fn1 = lambda *args: args
    print(fn1(10, 20, 30))
    ```

    > 注意：这里的可变参数传入到lambda之后，返回值为元组。

    4. 可变参数：**kwargs

    ```python
    fn1 = lambda **kwargs: kwargs
    print(fn1(name='python', age=20))
    ```

     lambda的应用

    带判断的lambda

    ```python
    fn1 = lambda a, b: a if a > b else b
    print(fn1(1000, 500))
    ```

    5. 列表数据按字典key的值排序

    ```python
    students = [
        {'name': 'TOM', 'age': 20},
        {'name': 'ROSE', 'age': 19},
        {'name': 'Jack', 'age': 22}
    ]
    
    # 按name值升序排列
    students.sort(key=lambda x: x['name'])
    print(students)
    
    # 按name值降序排列
    students.sort(key=lambda x: x['name'], reverse=True)
    print(students)
    
    # 按age值升序排列
    students.sort(key=lambda x: x['age'])
    print(students)
    ```

## 高级函数

1. 概述：把函数作为参数传入，化简代码

   1. map: 把迭代器中的元素作用到函数中

      ```python
      >>> def f(x):
      ...     return x * x
      ...
      >>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
      >>> list(r)
      [1, 4, 9, 16, 25, 36, 49, 64, 81]
      ```

   2. abs: 求绝对值
   3. round: 四舍五入
   4. reduce: 把序列中的元素从左到右进行累计计算
   5. filter: 把迭代器中的元素按照传入的函数进行过滤,返回值是filter对象

2. 迭代器

   1. 迭代器是一个可以记住遍历的位置的对象
   2. 迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退
   3. 两个基本的方法
      - __iter__() 
        - 返回一个特殊的迭代器对象， 这个迭代器对象实现了 __next__() 方法并通过 StopIteration 异常标识迭代的完成
      - __next__()
        - 返回下一个迭代器对象
        - StopIteration
          - StopIteration 异常用于标识迭代的完成，防止出现无限循环的情况，在 __next__() 方法中我们可以设置在完成指定循环次数后触发 StopIteration 异常来结束迭代。

3. 生成器

   - 生成器是迭代器的一种，使用yield返回值函数，每次调用yield会暂停，而可以使用next()函数和send()函数恢复生成器

## 文件操作步骤

#####  打开文件模式

| 模式 | 描述                                                         |
| :--: | ------------------------------------------------------------ |
|  r   | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
|  rb  | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。 |
|  r+  | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。 |
|  w   | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
|  wb  | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
|  w+  | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
|  a   | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
|  ab  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
|  a+  | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

``` python
f = open('test.txt', 'w')
```

> 注意：此时的`f`是`open`函数的文件对象。

- 文件对象方法
  - 写

    ```
    对象对象.write('内容')
    ```

    ```python
    # 1. 打开文件
    f = open('test.txt', 'w')
    
    # 2.文件写入
    f.write('hello world')
    
    # 3. 关闭文件
    f.close()
    ```

    > 注意：
    >
    > 1. `w	`和`a`模式：如果文件不存在则创建该文件；如果文件存在，`w`模式先清空再写入，`a`模式直接末尾追加。
    > 2. `r`模式：如果文件不存在则报错。

  - 读

    - read()

      ```
      文件对象.read(num)
      ```

      > num表示要从文件中读取的数据的长度（单位是字节），如果没有传入num，那么就表示读取文件中所有的数据。

    - readlines()

      readlines可以按照行的方式把整个文件中的内容进行一次性读取，并且返回的是一个列表，其中每一行的数据为一个元素。

      ```python
      f = open('test.txt')
      content = f.readlines()
      
      # ['hello world\n', 'abcdefg\n', 'aaa\n', 'bbb\n', 'ccc']
      print(content)
      
      # 关闭文件
      f.close()
      ```

    - readline()

      readline()一次读取一行内容。

      ```python
      f = open('test.txt')
      
      content = f.readline()
      print(f'第一行：{content}')
      
      content = f.readline()
      print(f'第二行：{content}')
      
      # 关闭文件
      f.close()
      ```

    - seek()

      作用：用来移动文件指针。

      ```python
      文件对象.seek(偏移量, 起始位置)
      ```

      > 起始位置：
      >
      > - 0：文件开头
      > - 1：当前位置
      > - 2：文件结尾

  - 关闭

    ```python
    文件对象.close()
    ```

##### 文件备份

需求：用户输入当前目录下任意文件名，程序完成对该文件的备份功能(备份文件名为xx[备份]后缀，例如：test[备份].txt)。

- 步骤
  - 接收用户输入的文件名
  - 规划备份文件名
  - 备份文件写入数据

-  代码实现

  - 接收用户输入目标文件名

    ```python
    old_name = input('请输入您要备份的文件名：')
    ```

  - 规划备份文件名

    - 提取目标文件后缀

    - 组织备份的文件名，xx[备份]后缀

      ```python
      # 2.1 提取文件后缀点的下标
      index = old_name.rfind('.')
      
      # print(index)  # 后缀中.的下标
      
      # print(old_name[:index])  # 源文件名（无后缀）
      
      # 2.2 组织新文件名 旧文件名 + [备份] + 后缀
      new_name = old_name[:index] + '[备份]' + old_name[index:]
      
      # 打印新文件名（带后缀）
      # print(new_name)
      ```

- 备份文件写入数据
  - 打开源文件 和 备份文件
  - 将源文件数据写入备份文件
  - 关闭文件

``` python
# 3.1 打开文件
old_f = open(old_name, 'rb')
new_f = open(new_name, 'wb')

# 3.2 将源文件数据写入备份文件
while True:
    con = old_f.read(1024)
    if len(con) == 0:
        break
    new_f.write(con)

# 3.3 关闭文件
old_f.close()
new_f.close()
```

##### 思考

如果用户输入`.txt`，这是一个无效文件，程序如何更改才能限制只有有效的文件名才能备份？

答：添加条件判断即可。

``` python
old_name = input('请输入您要备份的文件名：')

index = old_name.rfind('.')


if index > 0:
    postfix = old_name[index:]

new_name = old_name[:index] + '[备份]' + postfix

old_f = open(old_name, 'rb')
new_f = open(new_name, 'wb')

while True:
    con = old_f.read(1024)
    if len(con) == 0:
        break
    new_f.write(con)

old_f.close()
new_f.close()
```

##### 文件和文件夹的操作

在Python中文件和文件夹的操作要借助os模块里面的相关功能，具体步骤如下：

1. 导入os模块

``` python
import os
```

2. 使用`os`模块相关功能

``` python
os.函数名()
```

3. 文件重命名

``` python
os.rename(目标文件名, 新文件名)
```

4. 删除文件

``` python
os.remove(目标文件名)
```

5. 创建文件夹

``` python
os.mkdir(文件夹名字)
```

6. 删除文件夹

``` python
os.rmdir(文件夹名字)
```

7. 获取当前目录

``` python
os.getcwd()
```

8. 改变默认目录

``` python
os.chdir(目录)
```

9. 获取目录列表

``` python
os.listdir(目录)
```

##### 应用案例

需求：批量修改文件名，既可添加指定字符串，又能删除指定字符串。

- 步骤

1. 设置添加删除字符串的的标识
2. 获取指定目录的所有文件
3. 将原有文件名添加/删除指定字符串，构造新名字
4. os.rename()重命名

- 代码

``` python
import os

# 设置重命名标识：如果为1则添加指定字符，flag取值为2则删除指定字符
flag = 1

# 获取指定目录
dir_name = './'

# 获取指定目录的文件列表
file_list = os.listdir(dir_name)
# print(file_list)


# 遍历文件列表内的文件
for name in file_list:

    # 添加指定字符
    if flag == 1:
        new_name = 'Python-' + name
    # 删除指定字符
    elif flag == 2:
        num = len('Python-')
        new_name = name[num:]

    # 打印新文件名，测试程序正确性
    print(new_name)
    
    # 重命名
    os.rename(dir_name+name, dir_name+new_name)
```

## 异常

- 了解异常

当检测到一个错误时，解释器就无法继续执行了，反而出现了一些错误的提示，这就是所谓的"异常"。

- 异常的写法

  ```python
  try:
      可能发生错误的代码
  except:
      如果出现异常执行的代码
  ```

需求：尝试以`r`模式打开文件，如果文件不存在，则以`w`方式打开。

``` python
try:
    f = open('test.txt', 'r')
except:
    f = open('test.txt', 'w')
```

- 捕获指定异常

``` python
try:
    可能发生错误的代码
except 异常类型:
    如果捕获到该异常类型执行的代码
```

``` python
try:
    print(num)
except NameError:
    print('有错误')
```

> 注意：
>
> 1. 如果尝试执行的代码的异常类型和要捕获的异常类型不一致，则无法捕获异常。
> 2. 一般try下方只放一行尝试执行的代码。

- 捕获多个指定异常

当捕获多个异常时，可以把要捕获的异常类型的名字，放到except 后，并使用元组的方式进行书写。

``` python
try:
    print(1/0)

except (NameError, ZeroDivisionError):
    print('有错误')
```

- 捕获异常描述信息

``` python
try:
    print(num)
except (NameError, ZeroDivisionError) as result:
    print(result)
```

- 捕获所有异常

Exception是所有程序异常类的父类。

``` python
try:
    print(num)
except Exception as result:
    print(result)
```

- 异常的else

else表示的是如果没有异常要执行的代码。

``` python
try:
    print(1)
except Exception as result:
    print(result)
else:
    print('我是else，是没有异常的时候执行的代码')
```

- 异常的finally

finally表示的是无论是否异常都要执行的代码，例如关闭文件。

``` python
try:
    f = open('test.txt', 'r')
except Exception as result:
    f = open('test.txt', 'w')
else:
    print('没有异常，真开心')
finally:
    f.close()
```

- 异常的传递

体验异常传递

需求：

​	1. 尝试只读方式打开test.txt文件，如果文件存在则读取文件内容，文件不存在则提示用户即可。

​	2. 读取内容要求：尝试循环读取内容，读取过程中如果检测到用户意外终止程序，则`except`捕获异常并提示用户。

``` python
import time
try:
    f = open('test.txt')
    try:
        while True:
            content = f.readline()
            if len(content) == 0:
                break
            time.sleep(2)
            print(content)
    except:
        # 如果在读取文件的过程中，产生了异常，那么就会捕获到
        # 比如 按下了 ctrl+c
        print('意外终止了读取数据')
    finally:
        f.close()
        print('关闭文件')
except:
    print("没有这个文件")
```

- 自定义异常

在Python中，抛出自定义异常的语法为` raise 异常类对象`。

需求：密码长度不足，则报异常（用户输入密码，如果输入的长度不足3位，则报错，即抛出自定义异常，并捕获该异常）。

``` python
# 自定义异常类，继承Exception
class ShortInputError(Exception):
    def __init__(self, length, min_len):
        self.length = length
        self.min_len = min_len

    # 设置抛出异常的描述信息
    def __str__(self):
        return f'你输入的长度是{self.length}, 不能少于{self.min_len}个字符'


def main():
    try:
        con = input('请输入密码：')
        if len(con) < 3:
            raise ShortInputError(len(con), 3)
    except Exception as result:
        print(result)
    else:
        print('密码已经输入完成')


main()
```

## 面向对象

- 面向对象三大特性
  - 封装
    - 将属性和方法书写到类的里面的操作即为封装
    - 封装可以为属性和方法添加私有权限
  - 继承
    - 子类默认继承父类的所有属性和方法
    - 子类可以重写父类属性和方法
  - 多态
    - 传入不同的对象，产生不同的结果

- 多态
  - 了解多态

    多态指的是一类事物有多种形态，（一个抽象类有多个子类，因而多态的概念依赖于继承）。

    - 定义：多态是一种使用对象的方式，子类重写父类方法，调用不同子类对象的相同父类方法，可以产生不同的执行结果
    - 好处：调用灵活，有了多态，更容易编写出通用的代码，做出通用的编程，以适应需求的不断变化！
    - 实现步骤：
      - 定义父类，并提供公共方法
      - 定义子类，并重写父类方法
      - 传递子类对象给调用者，可以看到不同子类执行效果不同

  - 体验多态

    ```python
    class Dog(object):
        def work(self):  # 父类提供统一的方法，哪怕是空方法
            print('指哪打哪...')
    
    
    class ArmyDog(Dog):  # 继承Dog类
        def work(self):  # 子类重写父类同名方法
            print('追击敌人...')
    
    
    class DrugDog(Dog):
        def work(self):
            print('追查毒品...')
    
    
    class Person(object):
        def work_with_dog(self, dog):  # 传入不同的对象，执行不同的代码，即不同的work函数
            dog.work()
    
    
    ad = ArmyDog()
    dd = DrugDog()
    
    daqiu = Person()
    daqiu.work_with_dog(ad)
    daqiu.work_with_dog(dd)
    ```

- 类属性和实例属性

  - 类属性

    - 设置和访问类属性

      - 类属性就是 **类对象** 所拥有的属性，它被 **该类的所有实例对象 所共有**。

        - 类属性可以使用 **类对象** 或 **实例对象** 访问。

          ```python
          class Dog(object):
              tooth = 10
          
          
          wangcai = Dog()
          xiaohei = Dog()
          
          print(Dog.tooth)  # 10
          print(wangcai.tooth)  # 10
          print(xiaohei.tooth)  # 10
          ```

          > 类属性的优点
          >
          > - **记录的某项数据 始终保持一致时**，则定义类属性。
          > - **实例属性** 要求 **每个对象** 为其 **单独开辟一份内存空间** 来记录数据，而 **类属性** 为全类所共有 ，**仅占用一份内存**，**更加节省内存空间**。

    - 修改类属性

      类属性只能通过类对象修改，不能通过实例对象修改，如果通过实例对象修改类属性，表示的是创建了一个实例属性。

      ```python
      class Dog(object):
          tooth = 10
      
      
      wangcai = Dog()
      xiaohei = Dog()
      
      # 修改类属性
      Dog.tooth = 12
      print(Dog.tooth)  # 12
      print(wangcai.tooth)  # 12
      print(xiaohei.tooth)  # 12
      
      # 不能通过对象修改属性，如果这样操作，实则是创建了一个实例属性
      wangcai.tooth = 20
      print(Dog.tooth)  # 12
      print(wangcai.tooth)  # 20
      print(xiaohei.tooth)  # 12
      ```

  - 实例属性

    ```python
    class Dog(object):
        def __init__(self):
            self.age = 5
    
        def info_print(self):
            print(self.age)
    
    
    wangcai = Dog()
    print(wangcai.age)  # 5
    # print(Dog.age)  # 报错：实例属性不能通过类访问
    wangcai.info_print()  # 5
    ```

- 类方法和静态方法

  - 类方法

    - 类方法特点

      - 需要用装饰器`@classmethod`来标识其为类方法，对于类方法，**第一个参数必须是类对象**，一般以`cls`作为第一个参数。

    - 类方法使用场景

      - 当方法中 **需要使用类对象** (如访问私有类属性等)时，定义类方法

      - 类方法一般和类属性配合使用

        ```python
        class Dog(object):
            __tooth = 10
        
            @classmethod
            def get_tooth(cls):
                return cls.__tooth
        
        
        wangcai = Dog()
        result = wangcai.get_tooth()
        print(result)  # 10
        ```

  - 静态方法

    - 静态方法特点

      - 需要通过装饰器`@staticmethod`来进行修饰，**静态方法既不需要传递类对象也不需要传递实例对象（形参没有self/cls）**。
      - 静态方法 也能够通过 **实例对象** 和 **类对象** 去访问。

    - 静态方法使用场景

      - 当方法中 **既不需要使用实例对象**(如实例对象，实例属性)，**也不需要使用类对象** (如类属性、类方法、创建实例等)时，定义静态方法

      - **取消不需要的参数传递**，有利于 **减少不必要的内存占用和性能消耗**

        ```python
        class Dog(object):
            @staticmethod
            def info_print():
                print('这是一个狗类，用于创建狗实例....')
        
        
        wangcai = Dog()
        # 静态方法既可以使用对象访问又可以使用类访问
        wangcai.info_print()
        Dog.info_print()
        ```

- 继承的概念

  生活中的继承，一般指的是子女继承父辈的财产。

  - 拓展1：经典类或旧式类

    不由任意内置类型派生出的类，称之为经典类。

- 拓展2：新式类

```python
class 类名(object):
  代码
```



Python面向对象的继承指的是多个类之间的所属关系，即子类默认继承父类的所有属性和方法，具体如下：

``` python
# 父类A
class A(object):
    def __init__(self):
        self.num = 1

    def info_print(self):
        print(self.num)

# 子类B
class B(A):
    pass


result = B()
result.info_print()  # 1
```

> 在Python中，所有类默认继承object类，object类是顶级类或基类；其他子类叫做派生类。

- 单继承

> 故事主线：一个煎饼果子老师傅，在煎饼果子界摸爬滚打多年，研发了一套精湛的摊煎饼果子的技术。师父要把这套技术传授给他的唯一的最得意的徒弟。

分析：徒弟是不是要继承师父的所有技术？

``` python
# 1. 师父类
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')

        
# 2. 徒弟类
class Prentice(Master):
    pass


# 3. 创建对象daqiu
daqiu = Prentice()
# 4. 对象访问实例属性
print(daqiu.kongfu)
# 5. 对象调用实例方法
daqiu.make_cake()
```

- 多继承

> 故事推进：daqiu是个爱学习的好孩子，想学习更多的煎饼果子技术，于是，在百度搜索到黑马程序员，报班学习煎饼果子技术。

所谓多继承意思就是一个类同时继承了多个父类。

``` python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


# 创建学校类
class School(object):
    def __init__(self):
        self.kongfu = '[黑马煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


class Prentice(School, Master):
    pass


daqiu = Prentice()
print(daqiu.kongfu)
daqiu.make_cake()
```

> 注意：当一个类有多个父类的时候，默认使用第一个父类的同名属性和方法。

- 子类重写父类同名方法和属性

> 故事：daqiu掌握了师父和培训的技术后，自己潜心钻研出自己的独门配方的一套全新的煎饼果子技术。

``` python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


class School(object):
    def __init__(self):
        self.kongfu = '[黑马煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


# 独创配方
class Prentice(School, Master):
    def __init__(self):
        self.kongfu = '[独创煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


daqiu = Prentice()
print(daqiu.kongfu)
daqiu.make_cake()

print(Prentice.__mro__)
```

> 子类和父类具有同名属性和方法，默认使用子类的同名属性和方法。

- 子类调用父类的同名方法和属性

> 故事：很多顾客都希望也能吃到古法和黑马的技术的煎饼果子。

``` python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


class School(object):
    def __init__(self):
        self.kongfu = '[黑马煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


class Prentice(School, Master):
    def __init__(self):
        self.kongfu = '[独创煎饼果子配方]'

    def make_cake(self):
        # 如果是先调用了父类的属性和方法，父类属性会覆盖子类属性，故在调用属性前，先调用自己子类的初始化
        self.__init__()
        print(f'运用{self.kongfu}制作煎饼果子')

    # 调用父类方法，但是为保证调用到的也是父类的属性，必须在调用方法前调用父类的初始化
    def make_master_cake(self):
        Master.__init__(self)
        Master.make_cake(self)

    def make_school_cake(self):
        School.__init__(self)
        School.make_cake(self)


daqiu = Prentice()

daqiu.make_cake()

daqiu.make_master_cake()

daqiu.make_school_cake()

daqiu.make_cake()
```

- 多层继承

> 故事：N年后，daqiu老了，想要把所有技术传承给自己的徒弟。

``` python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


class School(object):
    def __init__(self):
        self.kongfu = '[黑马煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


class Prentice(School, Master):
    def __init__(self):
        self.kongfu = '[独创煎饼果子配方]'

    def make_cake(self):
        self.__init__()
        print(f'运用{self.kongfu}制作煎饼果子')

    def make_master_cake(self):
        Master.__init__(self)
        Master.make_cake(self)

    def make_school_cake(self):
        School.__init__(self)
        School.make_cake(self)


# 徒孙类
class Tusun(Prentice):
    pass


xiaoqiu = Tusun()

xiaoqiu.make_cake()

xiaoqiu.make_school_cake()

xiaoqiu.make_master_cake()

```

- super()调用父类方法

``` python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')


class School(Master):
    def __init__(self):
        self.kongfu = '[黑马煎饼果子配方]'

    def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')

        # 方法2.1
        # super(School, self).__init__()
        # super(School, self).make_cake()

        # 方法2.2
        super().__init__()
        super().make_cake()


class Prentice(School):
    def __init__(self):
        self.kongfu = '[独创煎饼果子技术]'

    def make_cake(self):
        self.__init__()
        print(f'运用{self.kongfu}制作煎饼果子')

    # 子类调用父类的同名方法和属性：把父类的同名属性和方法再次封装
    def make_master_cake(self):
        Master.__init__(self)
        Master.make_cake(self)

    def make_school_cake(self):
        School.__init__(self)
        School.make_cake(self)

    # 一次性调用父类的同名属性和方法
    def make_old_cake(self):
        # 方法一：代码冗余；父类类名如果变化，这里代码需要频繁修改
        # Master.__init__(self)
        # Master.make_cake(self)
        # School.__init__(self)
        # School.make_cake(self)

        # 方法二: super()
        # 方法2.1 super(当前类名, self).函数()
        # super(Prentice, self).__init__()
        # super(Prentice, self).make_cake()

        # 方法2.2 super().函数()
        super().__init__()
        super().make_cake()


daqiu = Prentice()

daqiu.make_old_cake()
```

> 注意：使用super() 可以自动查找父类。调用顺序遵循 `__mro__` 类属性的顺序。比较适合单继承使用。

- 私有权限

  1. 定义私有属性和方法

     在Python中，可以为实例属性和方法设置私有权限，即设置某个实例属性或实例方法不继承给子类。

     > 故事：daqiu把技术传承给徒弟的同时，不想把自己的钱(2000000个亿)继承给徒弟，这个时候就要为`钱`这个实例属性设置私有权限。

     ```python
     class Master(object):
         def __init__(self):
             self.kongfu = '[古法煎饼果子配方]'
     
         def make_cake(self):
             print(f'运用{self.kongfu}制作煎饼果子')
     
     
     class School(object):
         def __init__(self):
             self.kongfu = '[黑马煎饼果子配方]'
     
         def make_cake(self):
             print(f'运用{self.kongfu}制作煎饼果子')
     
     
     class Prentice(School, Master):
         def __init__(self):
             self.kongfu = '[独创煎饼果子配方]'
             # 定义私有属性
             self.__money = 2000000
     
         # 定义私有方法
         def __info_print(self):
             print(self.kongfu)
             print(self.__money)
     
         def make_cake(self):
             self.__init__()
             print(f'运用{self.kongfu}制作煎饼果子')
     
         def make_master_cake(self):
             Master.__init__(self)
             Master.make_cake(self)
     
         def make_school_cake(self):
             School.__init__(self)
             School.make_cake(self)
     
     
     # 徒孙类
     class Tusun(Prentice):
         pass
     
     
     daqiu = Prentice()
     # 对象不能访问私有属性和私有方法
     # print(daqiu.__money)
     # daqiu.__info_print()
     
     xiaoqiu = Tusun()
     # 子类无法继承父类的私有属性和私有方法
     # print(xiaoqiu.__money)  # 无法访问实例属性__money
     # xiaoqiu.__info_print()
     ```

  2. 在属性名和方法名 前面 加上两个下划线 __。

  3. 获取和修改私有属性值

     在Python中，一般定义函数名`get_xx`用来获取私有属性，定义`set_xx`用来修改私有属性值。

     ```python
     class Master(object):
         def __init__(self):
             self.kongfu = '[古法煎饼果子配方]'
     
         def make_cake(self):
             print(f'运用{self.kongfu}制作煎饼果子')
     
     
     class School(object):
         def __init__(self):
             self.kongfu = '[黑马煎饼果子配方]'
     
         def make_cake(self):
             print(f'运用{self.kongfu}制作煎饼果子')
     
     
     class Prentice(School, Master):
         def __init__(self):
             self.kongfu = '[独创煎饼果子配方]'
             self.__money = 2000000
     
         # 获取私有属性
         def get_money(self):
             return self.__money
     
         # 修改私有属性
         def set_money(self):
             self.__money = 500
     
         def __info_print(self):
             print(self.kongfu)
             print(self.__money)
     
         def make_cake(self):
             self.__init__()
             print(f'运用{self.kongfu}制作煎饼果子')
     
         def make_master_cake(self):
             Master.__init__(self)
             Master.make_cake(self)
     
         def make_school_cake(self):
             School.__init__(self)
             School.make_cake(self)
     
     
     # 徒孙类
     class Tusun(Prentice):
         pass
     
     
     daqiu = Prentice()
     
     xiaoqiu = Tusun()
     # 调用get_money函数获取私有属性money的值
     print(xiaoqiu.get_money())
     # 调用set_money函数修改私有属性money的值
     xiaoqiu.set_money()
     print(xiaoqiu.get_money())
     ```

​				
​						
​					
​					
​							
​									
​							
​					
​				
​				
​				
​			
​		
​	




​	
​	
