---
title: Pyhton基础
date: 2021-06-06 00:12:38
author: L
summary: 基础
categories: python
tags:
  - Python
---

# **Python** 基础

- 注释

  1. 单行： `# 注释内容`，快捷键ctrl+/
  2. 多行：`""" 注释内容 """` 或 `''' 注释内容 '''`

- 变量

  1. 定义变量的语法

     ​	变量名 = 值

  2. 标识符

     1. 由数字、字母、下划线组成
     2. 不能数字开头
     3. 不能使用内置关键字
     4. 严格区分大小写

  3. 数据类型

  4. 输出

     1. 格式化符号

        | 格式符号 |           转换           |
        | :------: | :----------------------: |
        |  ==%s==  |       字符串(常用)       |
        |  ==%d==  | 有符号的十进制整数(常用) |
        |  ==%f==  |       浮点数(常用)       |
        |    %c    |           字符           |
        |    %u    |     无符号十进制整数     |
        |    %o    |        八进制整数        |
        |    %x    |  十六进制整数（小写ox）  |
        |    %X    |  十六进制整数（大写OX）  |
        |    %e    |  科学计数法（小写'e'）   |
        |    %E    |  科学计数法（大写'E'）   |
        |    %g    |       %f和%e的简写       |
        |    %G    |       %f和%E的简写       |

     - %06d，表示输出的整数显示位数，不足以0补全，超出当前位数则原样输出
     - %.2f，表示小数点后显示的小数位数。

     2. f-字符串

        格式化字符串除了%s，还可以写为`f'{表达式}'`

        ```python
        age = 18 
        name = 'TOM'
        weight = 75.5
        student_id = 1
        
        # 我的名字是TOM
        print('我的名字是%s' % name)
        
        # 我的学号是0001
        print('我的学号是%4d' % student_id)
        
        # 我的体重是75.50公斤
        print('我的体重是%.2f公斤' % weight)
        
        # 我的名字是TOM，今年18岁了
        print('我的名字是%s，今年%d岁了' % (name, age))
        
        # 我的名字是TOM，明年19岁了
        print('我的名字是%s，明年%d岁了' % (name, age + 1))
        
        # 我的名字是TOM，明年19岁了
        print(f'我的名字是{name}, 明年{age + 1}岁了')
        ```

     > f-格式化字符串是Python3.6中新增的格式化方法，该方法更简单易读。

     3. 转义字符

        - `\n`：换行。
        - `\t`：制表符，一个tab键（4个空格）的距离。

     4. 结束符

        ```python
        print('内容', end="")
        ```

  5. 输入

  6. 转换数据类型函数

     |          函数          |                        说明                         |
     | :--------------------: | :-------------------------------------------------: |
     |  ==int(x [,base ])==   |                  将x转换为一个整数                  |
     |     ==float(x )==      |                 将x转换为一个浮点数                 |
     | complex(real [,imag ]) |        创建一个复数，real为实部，imag为虚部         |
     |      ==str(x )==       |                将对象 x 转换为字符串                |
     |        repr(x )        |             将对象 x 转换为表达式字符串             |
     |    ==eval(str )==s     | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
     |     ==tuple(s )==      |               将序列 s 转换为一个元组               |
     |      ==list(s )==      |               将序列 s 转换为一个列表               |
     |        chr(x )         |           将一个整数转换为一个Unicode字符           |
     |        ord(x )         |           将一个字符转换为它的ASCII整数值           |
     |        hex(x )         |         将一个整数转换为一个十六进制字符串          |
     |        oct(x )         |          将一个整数转换为一个八进制字符串           |
     |        bin(x )         |          将一个整数转换为一个二进制字符串           |

     - int() 整形
     - float() 浮点型
     - str() 字符串
     - list() 列表
     - tuple() 元祖
     - eval()

  > 查看数据类型用 type

- 运算符

  1. 算数运算符	

     | 运算符 |  描述  | 实例                                                  |
     | :----: | :----: | ----------------------------------------------------- |
     |   +    |   加   | 1 + 1 输出结果为 2                                    |
     |   -    |   减   | 1-1 输出结果为 0                                      |
     |   *    |   乘   | 2 * 2 输出结果为 4                                    |
     |   /    |   除   | 10 / 2 输出结果为 5                                   |
     |   //   |  整除  | 9 // 4 输出结果为2                                    |
     |   %    |  取余  | 9 % 4 输出结果为 1                                    |
     |   **   |  指数  | 2 ** 4 输出结果为 16，即 2 * 2 * 2 * 2                |
     |   ()   | 小括号 | 小括号用来提高运算优先级，即 (1 + 2) * 3 输出结果为 9 |

     > 注意：混合运算优先级顺序：`()`高于 `**` 高于 `*` `/` `//` `%` 高于 `+` `-`

  2. 赋值运算符

     | 运算符 | 描述 | 实例                                |
     | ------ | ---- | ----------------------------------- |
     | =      | 赋值 | 将`=`右侧的结果赋值给等号左侧的变量 |

     - 单个变量赋值
     - 多个变量赋值
     - 多变量赋相同值

  3. 复合赋值运算符

     | 运算符 | 描述           | 实例                       |
     | ------ | -------------- | -------------------------- |
     | +=     | 加法赋值运算符 | c += a 等价于 c = c + a    |
     | -=     | 减法赋值运算符 | c -= a 等价于 c = c- a     |
     | *=     | 乘法赋值运算符 | c *= a 等价于 c = c * a    |
     | /=     | 除法赋值运算符 | c /= a 等价于 c = c / a    |
     | //=    | 整除赋值运算符 | c //= a 等价于 c = c // a  |
     | %=     | 取余赋值运算符 | c %= a 等价于 c = c % a    |
     | **=    | 幂赋值运算符   | c ** = a 等价于 c = c ** a |

  4. 比较运算符

     | 运算符 | 描述                                                         | 实例                                                        |
     | ------ | ------------------------------------------------------------ | ----------------------------------------------------------- |
     | ==     | 判断相等。如果两个操作数的结果相等，则条件结果为真(True)，否则条件结果为假(False) | 如a=3,b=3，则（a == b) 为 True                              |
     | !=     | 不等于 。如果两个操作数的结果不相等，则条件为真(True)，否则条件结果为假(False) | 如a=3,b=3，则（a == b) 为 True如a=1,b=3，则(a != b) 为 True |
     | >      | 运算符左侧操作数结果是否大于右侧操作数结果，如果大于，则条件为真，否则为假 | 如a=7,b=3，则(a > b) 为 True                                |
     | <      | 运算符左侧操作数结果是否小于右侧操作数结果，如果小于，则条件为真，否则为假 | 如a=7,b=3，则(a < b) 为 False                               |
     | >=     | 运算符左侧操作数结果是否大于等于右侧操作数结果，如果大于，则条件为真，否则为假 | 如a=7,b=3，则(a < b) 为 False如a=3,b=3，则(a >= b) 为 True  |
     | <=     | 运算符左侧操作数结果是否小于等于右侧操作数结果，如果小于，则条件为真，否则为假 | 如a=3,b=3，则(a <= b) 为 True                               |

  5. 逻辑运算符

     | 运算符 | 逻辑表达式 | 描述                                                         | 实例                                     |
     | ------ | ---------- | ------------------------------------------------------------ | ---------------------------------------- |
     | and    | x and y    | 布尔"与"：如果 x 为 False，x and y 返回 False，否则它返回 y 的值。 | True and False， 返回 False。            |
     | or     | x or y     | 布尔"或"：如果 x 是 True，它返回 True，否则它返回 y 的值。   | False or True， 返回 True。              |
     | not    | not x      | 布尔"非"：如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not True 返回 False, not False 返回 True |

     拓展

     数字之间的逻辑运算

     ```python
     a = 0
     b = 1
     c = 2
     
     # and运算符，只要有一个值为0，则结果为0，否则结果为最后一个非0数字
     print(a and b)  # 0
     print(b and a)  # 0
     print(a and c)  # 0
     print(c and a)  # 0
     print(b and c)  # 2
     print(c and b)  # 1
     
     # or运算符，只有所有值为0结果才为0，否则结果为第一个非0数字
     print(a or b)  # 1
     print(a or c)  # 2
     print(b or c)  # 1
     ```

- 条件语句

  1. if 语法	

     ```python
     if 条件:
         条件成立执行的代码
     ```

  2. if...else...

     ```python
     if 条件:
         条件成立执行的代码
     else:
         条件不成立执行的代码
     ```

  3. 多重判断

     ```python
     if 条件1:
         条件1成立执行的代码
     elif 条件2:
         条件2成立执行的代码
     else:
         以上条件都不成立执行的代码
     ```

  4. if嵌套

     ```python
     if 条件1:
     
         条件1成立执行的代码
     
         if 条件2:
     
             条件2成立执行的代码
     
             ....
     ```

  5. 三目运算符

     语法如下：

     ```python
     值1 if 条件 else 值2
     ```

- 循环

  1. while语法

  ```python
  while 条件:
      条件成立重复执行的代码1
      条件成立重复执行的代码2
      ......
  ```

  - while循环嵌套语法

  ```python
  while 条件1:
      条件1成立执行的代码
      ......
      while 条件2:
          条件2成立执行的代码
          ......
  ```

  - for循环语法

  ```python
  for 临时变量 in 序列:
      重复执行的代码1
      重复执行的代码2
      ......
  ```

  - break退出整个循环
  - continue退出本次循环，继续执行下一次重复执行的代码
  - else
    - while和for都可以配合else使用
    - else下方缩进的代码含义：当循环正常结束后执行的代码
    - break终止循环不会执行else下方缩进的代码
    - continue退出循环的方式执行else下方缩进的代码

## 字符串

1. 概述：字符串是 Python 中最常用的数据类型。我们一般使用引号来创建字符串。创建字符串很简单，只要为变量分配一个值即可。即它是不可变类型

2. 下标又称索引，用来快速定位对应的数据

3. 切片：是指对操作的对象截取其中一部分的操作。字符串、列表、元组都支持切片操作，（序列[开始位置下标:结束位置下标:步长]）默认步长为1。

4. 常用操作方法：

   - ##### 查找

     - find

       ```python
       字符串序列.find(子串, 开始位置下标, 结束位置下标)
       检测某个子串是否包含在这个字符串中，如果在返回这个子串开始的位置下标，否则则返回-1。
       ```

     - Index

       ```python
       字符串序列.index(子串, 开始位置下标, 结束位置下标)
       检测某个子串是否包含在这个字符串中，如果在返回这个子串开始的位置下标，否则则报异常。
       ```

     - count

       ```
       字符串序列.count(子串, 开始位置下标, 结束位置下标)
       返回某个子串在字符串中出现的次数
       ```

   - ##### 修改

     - replace  ---> 替换 (字符串序列.replace(旧子串, 新子串, 替换次数)

     - split ---> 按照指定字符分割字符串 (字符串序列.split(分割字符, num))

     - Join ---> 用一个字符或子串合并字符串，即是将多个字符串合并为一个新的字符串 (字符或子串.join(多字符串组成的序列))

     - capitalize()

       将字符串第一个字符转换成大写。

       ```python
       mystr = "hello world and itcast and itheima and Python"
       
       # 结果：Hello world and itcast and itheima and python
       print(mystr.capitalize())
       ```

       > 注意：capitalize()函数转换后，只字符串第一个字符大写，其他的字符全都小写。
       >
       > title()：将字符串每个单词首字母转换成大写。

     - 判断

       - startswith

         ```
         字符串序列.startswith(子串, 开始位置下标, 结束位置下标)
         检查字符串是否是以指定子串开头，是则返回 True，否则返回 False。如果设置开始和结束位置下标，则在指定范围内检查。
         ```

       - endswith

         ```
         字符串序列.endswith(子串, 开始位置下标, 结束位置下标)
         检查字符串是否是以指定子串结尾，是则返回 True，否则返回 False。如果设置开始和结束位置下标，则在指定范围内检查
         ```

       - isalpha
       - 如果字符串至少有一个字符并且所有字符都是字母则返回 True, 否则返回 False。
       - isdigit
         - 如果字符串只包含数字则返回 True 否则返回 False
       
       - isspace
         - 如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True,否则返回 False
       
       - isalnum
         - 如果字符串中只包含空白，则返回 True，否则返回 False

     - ##### 拓展

       - format() ---> 格式化字符串

       - title() ---> 将字符串每个单词首字母转换成大写

       - lower() ---> 将字符串中大写转小写

       - strip() ---> 删除前后空格

       - upper() ---> 将字符串中小写转大写

       - lstrip() ---> 删除字符串左侧空白字符

       - rstrip() ---> 删除字符串右侧空白字符。

       - ljust():  返回一个原字符串左对齐,并使用指定字符(默认空格)填充至对应长度 的新字符串。

         ```python
         字符串序列.ljust(长度, 填充字符)
         ```

       - rjust()：返回一个原字符串右对齐,并使用指定字符(默认空格)填充至对应长度 的新字符串，语法和ljust()相同。

       - center()：返回一个原字符串居中对齐,并使用指定字符(默认空格)填充至对应长度 的新字符串，语法和ljust()相同。

         ```
         字符串序列.replace(旧子串, 新子串, 替换次数)
         ```

       > 注意：替换次数如果查出子串出现次数，则替换次数为该子串出现次数。

     - [https://www.runoob.com/python/python-strings.html]: 

## 列表

1. 概述: 列表可以一次性存储多个数据，且可以为不同数据类型, 即它是可变类型

2. 常用操作

   - ##### 查

     - index

       ```
       列表序列.index(数据, 开始位置下标, 结束位置下标)
       返回指定数据所在位置的下标
       ```

     - count ---> 统计指定数据在当前列表中出现的次数

     - Len ---> 访问列表长度，即列表中数据的个数

     - 判断

       1. in：判断指定数据在某个列表序列，如果在返回True，否则返回False
       2. not in：判断指定数据不在某个列表序列，如果不在返回True，否则返回False

   - ##### 增

     - append

       ```
       列表序列.append(数据)
       列表结尾追加数据
       ```

       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       name_list.append('xiaoming')
       
       # 结果：['Tom', 'Lily', 'Rose', 'xiaoming']
       print(name_list)
       ```

       > 列表追加数据的时候，直接在原列表里面追加了指定数据，即修改了原列表，故列表为可变类型数据。

       如果append()追加的数据是一个序列，则追加整个序列到列表

       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       name_list.append(['xiaoming', 'xiaohong'])
       
       # 结果：['Tom', 'Lily', 'Rose', ['xiaoming', 'xiaohong']]
       print(name_list)
       ```

     - extend()

       列表结尾追加数据，如果数据是一个序列，则将这个序列的数据逐一添加到列表。

       语法

       ```python
       列表序列.extend(数据)
       ```

       1.  单个数据


       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       name_list.extend('xiaoming')
       
       # 结果：['Tom', 'Lily', 'Rose', 'x', 'i', 'a', 'o', 'm', 'i', 'n', 'g']
       print(name_list)
       ```
    
       2. 序列数据
    
       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       name_list.extend(['xiaoming', 'xiaohong'])
       
       # 结果：['Tom', 'Lily', 'Rose', 'xiaoming', 'xiaohong']
       print(name_list)
       ```
    
     - insert()
    
       指定位置新增数据。
    
       ```python
       列表序列.insert(位置下标, 数据)
       ```
    
       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       name_list.insert(1, 'xiaoming')
       
       # 结果：['Tom', 'xiaoming', 'Lily', 'Rose']
       print(name_list)
       ```

   - ##### 删

     - del

       ```python
       del 目标
       ```

       1. 删除列表


       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       # 结果：报错提示：name 'name_list' is not defined
       del name_list
       print(name_list)
       ```
    
       2. 删除指定数据
    
       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       del name_list[0]
       
       # 结果：['Lily', 'Rose']
       print(name_list)
       ```
    
     - pop()
    
       删除指定下标的数据(默认为最后一个)，并返回该数据。
    
       ```python
       列表序列.pop(下标)
       ```
    
       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       del_name = name_list.pop(1)
       
       # 结果：Lily
       print(del_name)
       
       # 结果：['Tom', 'Rose']
       print(name_list)
       ```
    
     - remove()
    
       移除列表中某个数据的第一个匹配项。
    
       ```python
       列表序列.remove(数据)
       ```
    
       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       name_list.remove('Rose')
       
       # 结果：['Tom', 'Lily']
       print(name_list)
       ```
    
     - clear()
    
       清空列表
    
       ```python
       name_list = ['Tom', 'Lily', 'Rose']
       
       name_list.clear()
       print(name_list) # 结果： []
       ```

   - ##### 改

     1. 修改指定下标数据

        ```python
        name_list = ['Tom', 'Lily', 'Rose']
        
        name_list[0] = 'aaa'
        
        # 结果：['aaa', 'Lily', 'Rose']
        print(name_list)
        ```

     2. 逆置：reverse()

        ```python
        num_list = [1, 5, 2, 3, 6, 8]
        
        num_list.reverse()
        
        # 结果：[8, 6, 3, 2, 5, 1]
        print(num_list)
        ```

     3. 排序：sort()

        ```python
        列表序列.sort( key=None, reverse=False)
        ```

        > 注意：reverse表示排序规则，**reverse = True** 降序， **reverse = False** 升序（默认）

        ```python
        num_list = [1, 5, 2, 3, 6, 8]
        
        num_list.sort()
        
        # 结果：[1, 2, 3, 5, 6, 8]
        print(num_list)
        ```

     1. 复制

        copy()

        ```python
        name_list = ['Tom', 'Lily', 'Rose']
        
        name_li2 = name_list.copy()
        
        # 结果：['Tom', 'Lily', 'Rose']
        print(name_li2)
        ```

     2. 循环遍历和列表嵌套

        1. while

           ```python
           name_list = ['Tom', 'Lily', 'Rose']
           
           i = 0
           while i < len(name_list):
               print(name_list[i])
               i += 1
           ```

        2. for

           ```python
           name_list = ['Tom', 'Lily', 'Rose']
           
           for i in name_list:
               print(i)
           ```

        3. 列表嵌套

           所谓列表嵌套指的就是一个列表里面包含了其他的子列表。

           应用场景：要存储班级一、二、三三个班级学生姓名，且每个班级的学生姓名在一个列表。

           ```python
           name_list = [['小明', '小红', '小绿'], ['Tom', 'Lily', 'Rose'], ['张三', '李四', '王五']]	
           ```

## 元祖

1. 元组特点：定义元组使用==小括号==，且==逗号==隔开各个数据，数据可以是不同的数据类型。

   ```python
   # 多个数据元组
   t1 = (10, 20, 30)
   
   # 单个数据元组
   t2 = (10,)
   ```

   > 注意：如果定义的元组只有一个数据，那么这个数据后面也好添加逗号，否则数据类型为唯一的这个数据的数据类型

2. 常见操作

   元组数据不支持修改，只支持查找

   查找

   1. 按下标查找数据

      ```python
      tuple1 = ('aa', 'bb', 'cc', 'bb')
      print(tuple1[0])  # aa
      ```

   2. index()

      查找某个数据，如果数据存在返回对应的下标，否则报错，语法和列表、字符串的index方法相同。

      ```python
      tuple1 = ('aa', 'bb', 'cc', 'bb')
      print(tuple1.index('aa'))  # 0
      ```

   3. count()

      统计某个数据在当前元组出现的次数。

      ```python
      tuple1 = ('aa', 'bb', 'cc', 'bb')
      print(tuple1.count('bb'))  # 2
      ```

   4. len()

      统计元组中数据的个数。

      ```python
      tuple1 = ('aa', 'bb', 'cc', 'bb')
      print(len(tuple1))  # 4
      ```

      > 注意：元组内的直接数据如果修改则立即报错

      ```python
      tuple1 = ('aa', 'bb', 'cc', 'bb')
      tuple1[0] = 'aaa'
      ```

      > 但是如果元组里面有列表，修改列表里面的数据则是支持的，故自觉很重要。

      ```python
      tuple2 = (10, 20, ['aa', 'bb', 'cc'], 50, 30)
      print(tuple2[2])  # 访问到列表
      
      # 结果：(10, 20, ['aaaaa', 'bb', 'cc'], 50, 30)
      tuple2[2][0] = 'aaaaa'
      print(tuple2)
      ```

## 字典

1. 字典特点：

   字典的每个键值 key=>value 对用冒号 : 分割，每个键值对之间用逗号 , 分割，整个字典包括在花括号 {} 中 

   - 符号为==大括号==

   - 数据为==键值对==形式出现

   - 各个键值对之间用==逗号==隔开

     ```python
     # 有数据字典
     dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
     
     # 空字典
     dict2 = {}
     
     dict3 = dict()
     ```

     > 注意：一般称冒号前面的为键(key)，简称k；冒号后面的为值(value)，简称v。

2. 常见操作

   1. 增

      ==字典序列[key] = 值==

      > 注意：如果key存在则修改这个key对应的值；如果key不存在则新增此键值对。

      ```python
      dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
      
      dict1['name'] = 'Rose'
      # 结果：{'name': 'Rose', 'age': 20, 'gender': '男'}
      print(dict1)
      
      dict1['id'] = 110
      
      # {'name': 'Rose', 'age': 20, 'gender': '男', 'id': 110}
      print(dict1)
      ```

      > 注意：字典为可变类型。

   2. 删

      1. del() / del

         删除字典或删除字典中指定键值对。

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         
         del dict1['gender']
         # 结果：{'name': 'Tom', 'age': 20}
         print(dict1)
         ```

      2. clear()

         清空字典

      3. pop() 

         删除字典给定键 key 所对应的值，返回值为被删除的值。key值必须给出。 否则，返回default值。 

      4. popitem()  

         返回并删除字典中的最后一对键和值 

   3. 查

      cmp() 比较两个字典元素 

      如果两个字典的元素相同返回0，如果字典dict1大于字典dict2返回1，如果字典dict1小于字典dict2返回-1 

      1. key值查找

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         print(dict1['name'])  # Tom
         print(dict1['id'])  # 报错
         ```

         > 如果当前查找的key存在，则返回对应的值；否则则报错。

      2. get()

         语法

         ```python
         字典序列.get(key, 默认值)
         ```

         > 注意：如果当前查找的key不存在则返回第二个参数(默认值)，如果省略第二个参数，则返回None。

         快速体验

         ```python 
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         print(dict1.get('name'))  # Tom
         print(dict1.get('id', 110))  # 110
         print(dict1.get('id'))  # None
         ```

      3. keys()

         以列表返回一个字典所有的键 

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         print(dict1.keys())  # dict_keys(['name', 'age', 'gender'])
         ```

      4. values()

         以列表返回字典中的所有值 

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         print(dict1.values())  # dict_values(['Tom', 20, '男'])
         ```

      5. items()

         以列表返回可遍历的(键, 值) 元组数组 

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         print(dict1.items())  # dict_items([('name', 'Tom'), ('age', 20), ('gender', '男')])
         ```

   4. 循环遍历

      1. 遍历字典的key

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         for key in dict1.keys():
             print(key)
         ```

      2. 遍历字典的value

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         for value in dict1.values():
             print(value)
         ```

      3. 遍历字典的元素

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         for item in dict1.items():
             print(item)
         ```

      4. 遍历字典的键值对

         ```python
         dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
         for key, value in dict1.items():
             print(f'{key} = {value}')
         ```

## 集合

1. 创建集合

   创建集合使用`{}`或`set()`， 但是如果要创建空集合只能使用`set()`，因为`{}`用来创建空字典。

   特点：

   1. 集合可以去掉重复数据；
   2. 集合数据是无序的，故不支持下标

2. 常见操作

   1. 增

      1. add()

         ```python
         s1 = {10, 20}
         s1.add(100)
         s1.add(10)
         print(s1)  # {100, 10, 20}
         ```

         > 因为集合有去重功能，所以，当向集合内追加的数据是当前集合已有数据的话，则不进行任何操作。

      2. update()

         追加的数据是序列。

         ```python
         s1 = {10, 20}
         # s1.update(100)  # 报错
         s1.update([100, 200])
         s1.update('abc')
         print(s1)
         ```

   2. 删

      1. remove()，删除集合中的指定数据，如果数据不存在则报错
      2. discard()，删除集合中的指定数据，如果数据不存在也不会报错。
      3. pop()，随机删除集合中的某个数据，并返回这个数据。

   3. 判断

      1. in：判断数据在集合序列
      2. not in：判断数据不在集合序列

## 公共操作

1. 运算符

   | 运算符 |      描述      |      支持的容器类型      |
   | :----: | :------------: | :----------------------: |
   |   +    |      合并      |    字符串、列表、元组    |
   |   *    |      复制      |    字符串、列表、元组    |
   |   in   |  元素是否存在  | 字符串、列表、元组、字典 |
   | not in | 元素是否不存在 | 字符串、列表、元组、字典 |

   ## +

   ```python
   # 1. 字符串 
   str1 = 'aa'
   str2 = 'bb'
   str3 = str1 + str2
   print(str3)  # aabb
   
   
   # 2. 列表 
   list1 = [1, 2]
   list2 = [10, 20]
   list3 = list1 + list2
   print(list3)  # [1, 2, 10, 20]
   
   # 3. 元组 
   t1 = (1, 2)
   t2 = (10, 20)
   t3 = t1 + t2
   print(t3)  # (10, 20, 100, 200)
   ```

   ## *

   ```python
   # 1. 字符串
   print('-' * 10)  # ----------
   
   # 2. 列表
   list1 = ['hello']
   print(list1 * 4)  # ['hello', 'hello', 'hello', 'hello']
   
   # 3. 元组
   t1 = ('world',)
   print(t1 * 4)  # ('world', 'world', 'world', 'world')
   ```

   ##  in或not in

   ```python
   # 1. 字符串
   print('a' in 'abcd')  # True
   print('a' not in 'abcd')  # False
   
   # 2. 列表
   list1 = ['a', 'b', 'c', 'd']
   print('a' in list1)  # True
   print('a' not in list1)  # False
   
   # 3. 元组
   t1 = ('a', 'b', 'c', 'd')
   print('aa' in t1)  # False
   print('aa' not in t1)  # True
   ```

2. 公共方法

   | 函数                    | 描述                                                         |
   | ----------------------- | ------------------------------------------------------------ |
   | len()                   | 计算容器中元素个数                                           |
   | del 或 del()            | 删除                                                         |
   | max()                   | 返回容器中元素最大值                                         |
   | min()                   | 返回容器中元素最小值                                         |
   | range(start, end, step) | 生成从start到end的数字，步长为 step，供for循环使用           |
   | enumerate()             | 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。 len() |

   ## len()

   ```python
   # 1. 字符串
   str1 = 'abcdefg'
   print(len(str1))  # 7
   
   # 2. 列表
   list1 = [10, 20, 30, 40]
   print(len(list1))  # 4
   
   # 3. 元组
   t1 = (10, 20, 30, 40, 50)
   print(len(t1))  # 5
   
   # 4. 集合
   s1 = {10, 20, 30}
   print(len(s1))  # 3
   
   # 5. 字典
   dict1 = {'name': 'Rose', 'age': 18}
   print(len(dict1))  # 2
   ```

   ## del()

   ```python
   # 1. 字符串
   str1 = 'abcdefg'
   del str1
   print(str1)
   
   # 2. 列表
   list1 = [10, 20, 30, 40]
   del(list1[0])
   print(list1)  # [20, 30, 40]
   ```

   ## max()

   ```python
   # 1. 字符串
   str1 = 'abcdefg'
   print(max(str1))  # g
   
   # 2. 列表
   list1 = [10, 20, 30, 40]
   print(max(list1))  # 40
   ```

   ## min()

   ```python
   # 1. 字符串
   str1 = 'abcdefg'
   print(min(str1))  # a
   
   # 2. 列表
   list1 = [10, 20, 30, 40]
   print(min(list1))  # 10
   ```

   ## range()

   ```python
   # 1 2 3 4 5 6 7 8 9
   for i in range(1, 10, 1):
       print(i)
   
   # 1 3 5 7 9
   for i in range(1, 10, 2):
       print(i)
   
   # 0 1 2 3 4 5 6 7 8 9
   for i in range(10):
       print(i)
   ```

   > 注意：range()生成的序列不包含end数字。

3. 容器类型转换

   1. tuple()

      将某个序列转换成元组

      ```python
      list1 = [10, 20, 30, 40, 50, 20]
      s1 = {100, 200, 300, 400, 500}
      
      print(tuple(list1))
      print(tuple(s1))
      ```

   2. list()

      将某个序列转换成列表

      ```python
      t1 = ('a', 'b', 'c', 'd', 'e')
      s1 = {100, 200, 300, 400, 500}
      
      print(list(t1))
      print(list(s1))
      ```

   3. set()

      将某个序列转换成集合

      ```python
      list1 = [10, 20, 30, 40, 50, 20]
      t1 = ('a', 'b', 'c', 'd', 'e')
      
      print(set(list1))
      print(set(t1))
      ```

      > 注意：
      >
      > ​	1.集合可以快速完成列表去重
      >
      > ​	2.集合不支持下标

## 推导式

```
列表推倒式: [结果 for 变量 in 可迭代对象 if 条件筛选]

生成器表达式: (结果 for 变量 in 可迭代对象 if 条件筛选), 没有元组推倒式

字典推倒式: {结果(k:v) for 变量 in 可迭代对象 if 条件筛选}

集合推倒式: {结果(k) for 变量 in 可迭代对象 if 条件筛选} 自带去重功能
```
