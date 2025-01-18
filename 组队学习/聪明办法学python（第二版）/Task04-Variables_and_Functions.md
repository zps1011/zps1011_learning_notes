<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>聪明办法学 python（第二版）<br/><span>Task 04：变量与函数</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年1月18日
</div>
<div>
 <!--  
    Author: zps1011 （欢乐一天）  
    Date: 2025-1-18 
    -->  
&nbsp;&nbsp;&nbsp;
</div>
**作业系统个人主页链接：https://hydro.ac/d/datawhale_p2s/user/48019**

### 第一题：Square Root

题目描述：我们会输入一个数字 x1，你需要编写程序，求解其平方根并返回求解后的值。本题只考虑平方根存在的情形。 **注意，我们希望所有以浮点数输出的值都能够保留两位小数**

输入格式：一个数字，数字类型为整数或浮点数 。

输出格式：一个浮点数，代表该数的平方根值，结果保留两位小数。

输入样例-1

```bash
9
```

输出样例-1

```bash
3.00
```

提示说明：由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
x1 = ast.literal_eval(input())

# 现在程序中有一个变量x1

# 在这行注释下面，编写代码，输出你的答案

# 当你想在输出保留两位小数时，你可以这样做
y1 = 1.0000 #最后输出的变量
print('%.2f' %y1)
```

**本题代码如下（代码均通过评测）：**

```python
# 方法一
import ast
import math

x1 = ast.literal_eval(input())
y1 = math.sqrt(x1)
print('%.2f' %y1)

'''
输出也可以这么写：
print('{:.2f}'.format(y1))
'''
```

> 代码长度91 Bytes
>
> 递交时间2025-1-18 20:20:24
>
> 评测时间2025-1-18 20:20:27
>
> 分数100
>
> 总耗时266ms
>
> 峰值时间54ms
>
> 峰值内存5.1 MiB




```python
# 方法二
import math

def main():
    x1 = float(input())             # 将输入转换为浮点数
    result = math.sqrt(x1)          # 使用 math 库的 sqrt 函数计算平方根
    print("{:.2f}".format(result))  # 以保留两位小数的形式输出结果


if __name__ == "__main__":
    main()
```

> 代码长度208 Bytes
>
> 递交时间2025-1-18 20:23:18
>
> 评测时间2025-1-18 20:23:19
>
> 分数100
>
> 总耗时176ms
>
> 峰值时间36ms
>
> 峰值内存4 MiB



### 第二题：Square

题目描述：我们会输入一个数字`x1`，你需要编写程序，求解这个数字的平方。

输入格式：一个数字，数字类型为整型数或浮点型数。

输出格式：一个浮点数，保留两位小数。

输入样例-1

```bash
10
```

输出样例-1

```bash
100.00
```

输入样例-2

```bash
3.14
```

输出样例-2

```bash
9.86
```

提示说明：由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
x1 = ast.literal_eval(input())
# 现在程序中有一个变量x1

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
x1 = ast.literal_eval(input())
y1 = x1 ** 2
print('%.2f' %y1)   # 保留两位小数的格式输出结果
```

```python
# 方法二
def main():
    x1 = float(input())  # 将输入转换为浮点数，这一步是为了确保输入无论是整数还是浮点数，都能正确参与后续的计算。
    result = x1 ** 2 
    print("{:.2f}".format(result))  # 保留两位小数的格式输出结果


if __name__ == "__main__":
    main()
```

### 第三题：Odd number

题目描述：我们会输入一个整型数 `x1`，你需要编写程序，判断这些数是奇数还是偶数，并且返回相应格式的内容。

输入格式：一个`int`整型数。

输出格式：若是奇数则输出`True`，否则输出`False`。

输入样例-1

```bash
8
```

输出样例-1

```bash
False
```

输入样例-2

```bash
3
```

输出样例-2

```bash
True
```

提示说明：由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
x1 = ast.literal_eval(input())
# 现在程序中有一个变量x1

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
x = ast.literal_eval(input())
if x % 2 == 0:
    print('False')
else:
    print('True')
```

```python
# 方法二
def main():
    x1 = int(input())  # 将用户输入的内容转换为整数，我们输入的数据是字符串形式，需要将其转换为整数才能进行数学运算。
    if x1 % 2 == 1:    # 判断 x1 除以 2 的余数是否为 1，进而判断奇偶性。
        print(True)
    else:
        print(False)


if __name__ == "__main__":
    main()
```



### 第四题：Range

题目描述：我们会输入两个数字`x1`和`x2`，你需要编写程序，求解其上界与下界并返回相应的值。

输入格式：输入两个`int`整型数，用逗号分隔。

输出格式：分别输出下界和上界，中间以空格隔开，具体见输出样例。

输入样例-1

```bash
19,12
```

输出样例-1

```bash
12 19
```

输入样例-2

```bash
10,20
```


输出样例-2

```bash
10 20
```


提示说明：由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
x1, x2 = ast.literal_eval(input())
# 现在程序中有变量x1和x2

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
x1,x2 = ast.literal_eval(input())
print(min(x1,x2),max(x1,x2))
```

```python
# 方法二
def main():
    x1, x2 = map(int, input().split(','))  # 从输入中读取两个以逗号分隔的整数，分别存储在 x1 和 x2 中
    lower_bound = min(x1, x2)        # 求出两个数中的较小值作为下界
    upper_bound = max(x1, x2)        # 求出两个数中的较大值作为上界
    print(lower_bound, upper_bound)  # 输出下界和上界


if __name__ == "__main__":
    main()
```

> `map(int, input().split(','))`：这部分代码的作用是从用户输入中读取数据。`input()` 函数接收用户输入的字符串，`split(',')` 方法将该字符串以逗号为分隔符拆分成多个子字符串，然后 `map(int,...)` 函数将这些子字符串转换为整数。最终将转换后的两个整数分别存储在 `x1` 和 `x2` 中。



### 第五题：circlesIntersect

题目描述：我们会输入 6 个数字 `x1`，`y1`，`r1`，`x2`，`y2`，`r2` 它们代表两个圆，圆心分别为 `(x1, y1)` 和 `(x2, y2)` ，半径分别为 `r1` 和 `r2`。你需要编写程序，判断两个圆是否相交，若相交则返回 `True` ，否则返回 `False`。（相交指两个圆在一个或多个点接触或重叠）。

输入格式：六个数字，数字类型为整数或者浮点数，以逗号分隔。

输出格式：`True` 或者 `False`，判断两个圆是否相交。

输入样例

```bash
0,0,2,3,0,2
```

输出样例

```bash
True
```

提示说明：由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
x1, y1, r1, x2, y2, r2 = ast.literal_eval(input())
# 现在程序中有六个变量，x1, y1, r1, x2, y2, r2

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
x1, y1, r1, x2, y2, r2 = ast.literal_eval(input())
d = ((x2-x1) ** 2 + (y2-y1) **2) **0.5

#判断两个圆是否相交
if d <= r1 + r2 and d >= abs(r1 - r2):
    print('True')
else:
    print('False')
```

```python
# 方法二
import math

def main():
    x1, y1, r1, x2, y2, r2 = map(float, input().split(','))  # 将输入的六个数字以逗号分隔并转换为浮点数
    # 计算两个圆心之间的距离
    distance = math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
    # 判断两圆是否相交，相交条件为：|r1 - r2| <= 距离 <= r1 + r2
    if abs(r1 - r2) <= distance <= r1 + r2:
        print(True)
    else:
        print(False)


if __name__ == "__main__":
    main()
```



### 附录

1、变量命名规则

- 必须以**字母**或**下划线（`_`）**开头
- 命名可由字母、数字和下划线组成
- 大小写敏感 （windows系统对大小写不敏感）
- 尽量**避免使用保留字**命名

2、变量是一个`标签`，而不是盒子。变量可以赋给值标签，也可以说变量指向特定的值。

3、保留字

执行以下代码后，可以看到我们当前所使用的 python 版本下的保留字。

```python
import keyword
keyword.kwlist
```

['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return',  'try', 'while', 'with', 'yield']

> `__peg_parser__` 是 Python 3.9 引入的一个特殊的保留字。其主要是 Python 解释器内部使用的一个特殊标记，它的存在更多的是为了 Python 解释器在解析源代码时的内部机制和处理，帮助 Python 解释器更有效地将 Python 代码解析为抽象语法树（AST）等内部表示形式。

4、函数

- 概念：函数是一段具有特定功能的、可重复利用的语句组，通过函数名来表示和调用的语句。

- 语法格式：

  ```python
  def <函数名> (<参数列表>):
      <函数体>
  ```

  > - 括号后面的冒号不能少；
  > - 即使该函数不需要接收任何参数，也必须保留一对空括号；
  > - 函数体相对于 def 关键字必须保持一定的空格缩进。
  > - 函数形参不需要声明其类型，也不需要指定函数的返回类型；

- 函数调用：定义后的函数不能直接运行，需要经过“调用”才能运行。




5、函数的参数传递

- 位置参数：是比较常用的形式，调用函数时实参和形参的顺序必须一致，并且数量相同。

- 默认值参数：函数的参数在定义时也可以指定默认值，函数调用时若该位置没有给定实
  际参数，则使用默认值代替。但需要注意**可选参数应当放在非可选参数后面**。

  ```python
  def <函数名> (<非可选参数列表>, <可选参数> = <默认值>):
      <函数体>
  ```

- 关键字参数：Python 语言同时支持函数按照参数名称方式传递参数，此种传参方式不必考虑实参和形参的顺序。

  ```python
  def <函数名> (<参数名称> = <实际值>):
      <函数体>
  ```

  ```python
  def zps1011(x, y):
      return(x * y)
      
  print(zps1011(y = 520, x = 1314))
  ```

- 可变长度参数：

- 除了以上传参方式以外，当我们不确定会接受多少个参数的时候可以利用可变长度参数解决。

   - \*param 接收任意多个参数放在一个**元组**中
   
     ```python
     def zps1011(*p):
         print(p)
     
     zps1011(1,2,3)
     
     # 输出：(1, 2, 3)
     ```
   
     
   
   - \**param 接收任意多个关键字参数放入**字典**中
   
     ```python
     def zps1011(**p):
         print(p)
     
     zps1011(z = 1,p = 2,s = 3)
     
     # 输出：{'z': 1, 'p': 2, 's': 3}
     ```



6、变量的作用域

- 根据程序中变量所在的位置和作用范围，变量分为**局部变量**和**全局变量**。局部变量仅在函数内部，且作用域也在函数内部，全局变量的作用域跨越多个函数。
- 局部变量：指在函数内部使用的变量，仅在函数内部有效，当函数退出时变量将不再存在。
- 全局变量：指在函数之外定义的变量，在程序执行全过程有效。全部变量在函数内部使用时，需要提前使用保留字 global 声明。
- 当局部变量与全局变量同名时，函数内部会优先使用局部变量。

7、函数的返回值

- return 语句用来**结束函数**并将程序**返回**到函数被调用的位置继续执行。
- return 语句可以出现在函数中的**任何部分**。
- return 可以同时返回 **0 个**或**多个**函数运算的**结果**给函数被调用处的**变量** 。
- 当 return 返回多个值时，返回的值形成元组数据类型。
- 函数也可以没有 return 代表无返回值。

8、匿名函数

- 匿名函数适合处理临时需要一个类似于函数的功能但又不想定义函数的场合，可以省去函数的定义过程和考虑函数的命名，让代码更加简洁，可读性更好。

- 表达式：

  ```python
  <函数对象名> = lambda <形式参数列表>:<表达式>
  ```

  ```python
  def fun(x, y):
      return x + y
  等价于匿名函数：
  fun = lambda x, y : x + y
  ```

  




### 参考资料

- [Github - 聪明办法学 python（第二版）-  变量与函数](https://github.com/datawhalechina/learn-python-the-smart-way-v2/blob/main/slides/chapter_3-Variables_and_Functions.ipynb)
- [聪明办法学 python（第二版）视频资料 - Chap3 变量与函数](https://www.bilibili.com/video/BV1A84y1f788?spm_id_from=333.788.videopod.sections)
- [菜鸟教程 - python 变量类型](https://www.runoob.com/python/python-variable-types.html)
- [菜鸟教程 - python 函数](https://www.runoob.com/python/python-functions.html)
