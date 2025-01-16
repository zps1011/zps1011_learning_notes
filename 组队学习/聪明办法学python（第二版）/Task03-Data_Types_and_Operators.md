<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>聪明办法学 python（第二版）<br/><span>Task 03：数据类型和操作</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年1月16日
</div>

<div>
 <!--  
    Author: zps1011 （欢乐一天）  
    Date: 2025-1-16 
    -->  
&nbsp&nbsp&nbsp
</div>

**作业系统个人主页链接：https://hydro.ac/d/datawhale_p2s/user/48019**

### 第一题：Is Number

题目描述：我们会为变量 `x` 赋值，你需要编写程序，判断其是否为数字数据类型，若是返回 `True`，否则返回 `False`。

输入格式：Python 数据类型。

输出格式：返回 `True` 或者 `False`

输入样例-1

```bash
100
```

输出样例-1

```bash
True
```

输入样例-2

```bash
'p2s'
```

输出样例-2

```bash
False
```


提示说明：

- 由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。


- 按照课程内容答题，**使用暴力枚举将会使你丢失所有分数**。


- 由于测试点要求，请务必使用该模版答题，否则会运行出错！


- 模板：


```python
import decimal as numpy
x = eval(input())
# 现在程序中有一个变量 x

# 在这行注释下面，编写代码，输出你的答案
```



**本题代码如下：**

```python
import decimal as numpy
import numbers # 包含了 python 中所有的数字类型

x = eval(input())

def isNumber(x):
    return isinstance(x, numbers.Number) # 可以应对任何类型的数字
print(isNumber(x))
```

> - 课程课件中有提及过相关知识点。 
> - 复数也属于数字。
> - `isinstance()` 比 `type()` 更具有 `稳健性（Robustness）`。
> - 这种做法更加符合 `面向对象编程` 中 `继承（inheritance）` 的思想。



### 第二题：Egg Cartons

题目描述：我们会输入一个非负整数 `eggs`，代表鸡蛋个数，你需要编写程序，输出存放这些鸡蛋的最小纸箱的数目，它应该是个整数。 **一个纸箱最多能容纳 12 枚鸡蛋**。

输入格式：一个非负整数，代表鸡蛋个数。

输出格式：一个整数，代表纸箱数。

输入样例-1

```bash
0
```


输出样例-1

```bash
0
```


输入样例-2

```bash
4
```

输出样例-2

```bash
1
```


输入样例-3

```bash
48
```


输出样例-3

```bash
4
```

提示说明：由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
eggs = ast.literal_eval(input())

# 现在程序中有一个整数，eggs

# 在这行注释下面，编写代码，输出你的答案
```



**本题代码入下：**

```python
import ast
import math
eggs = ast.literal_eval(input())
a = eggs / 12       # 需要装的箱数
b = math.ceil(a)    # 返回不小于a的最小整数。该函数功能是向上取整其最接近的整数
print(b)
```



### 第三题：Number Of Pool Balls

题目描述：台球按行排列，其中第一行包含 1 个台球，每一行最多比上一行多 1 个球，填满这一行之后才可以填充下一行。例如，3 行总共包含 6 个台球 （1+2+3）。输入一个 `int` 整数 `n`，代表台球的总行数，要求编写程序，输出 `n` 行的总台球数。

输入格式：一个 `int` 整数 n。

输出格式：一个 `int` 整数，代表总的台球数。

输入样例

```bash
3
```

输出样例

```bash
6
```

提示说明：

- 对于 n 的值，我们不会设置上限，也就是说可以包含无限数量的行。
- **本题不允许使用字符串索引、循环、列表与列表索引、递归**，但是我们不限制 `if` 的使用。我们无法技术上强制约束你使用这些特性，请 **自觉遵守学术诚信规定。** 助教会检查提交的代码，若 **不符合本条要求，成绩会被取消**。
- 由于难度问题，我们这里给出程序模板，请 在 **注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头。**

模板：

```python
import ast
n = ast.literal_eval(input())
# 现在程序中有一个整数，n

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
import ast
n = ast.literal_eval(input())   # 现在程序中有一个整数，n
total = n * (n + 1) / 2
print(int(total))
```



### 第四题：Number of Pool Ball Rows

题目描述：本题是 [`Number of Pool Ball`](https://hydro.ac/d/datawhale_p2s/p/P1013) 的相反操作。台球按行排列，其中第一行包含 1 个台球，每一行最多比上一行多 1 个球，填满这一行之后才可以填充下一行。例如，3 行最多包含 6 个台球 （1+2+3）。输入一个 `int` 整数 `n`，代表台球总数，要求编写程序，输出 `row` 代表台球的总行数。

输入格式：一个 `int` 整数 n，代表台球总数。

输出格式：一个 `int` 整数，代表台球的总行数

输入样例-1

```bash
6
```

输出样例-1

```bash
3
```

输入样例-2

```bash
4
```

输出样例-2

```bash
3
```

提示说明：

- 对于 n 的值，我们不会设置上限，也就是说可以包含无限数量的行。
- **本题不允许使用字符串索引、循环、列表与列表索引、递归**，但是我们不限制 `if` 的使用。我们无法技术上强制约束你使用这些特性，请 **自觉遵守学术诚信规定。** 助教会检查提交的代码，若 **不符合本条要求，成绩会被取消**。
- 由于难度问题，我们这里给出程序模板，请 在 **注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头。**

模板：

```python
import ast
n = ast.literal_eval(input())
# 现在程序中有一个整数 n

# 在这行注释下面，编写代码，输出你的答案
```



**本题代码如下：**

```python
import ast
import math
n = ast.literal_eval(input())
row = ((8 * n + 1) ** 0.5 - 1) / 2

print(math.ceil(row))
```



### 第五题：Get Kth Digit

题目描述：我们会输入 2 个非负的 `int` 整数 `n` 和 `k` 。你需要编写程序，返回整数 `n` 从右开始数的第 `k` 个数字（下标从 `0` 开始）

输入格式：2 个非负的 `int` 整数 `n` 和 `k`，以`,`分隔。

输出格式：1 个 `int` 整型数

输入样例

```bash
789,1
```


输出样例

```bash
8
```


提示说明

- **本题不允许使用字符串索引（如 `n[k]`）、循环、列表与列表索引、递归**，但是我们不限制 `if` 的使用。我们无法技术上强制约束你使用这些特性，请 **自觉遵守学术诚信规定。** 助教会检查提交的代码，若 **不符合本条要求，成绩会被取消**。
- 由于难度问题，我们这里给出程序模板，请 **在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
n, k = ast.literal_eval(input())
# 现在程序中有两个整数，n, k

# 在这行注释下面，编写代码，输出你的答案
```



**本题代码如下：**

```python
import ast
n, k = ast.literal_eval(input())
s = n // 10**k % 10
print(s)
```

> 从本题的输入输出可知，从右往左数的第 0 个数字为 9 ，第 1 个数字为 8 ，第 3 个数字为 7 。
>
> 



### 附录

1、`@` 运算符：用于矩阵乘法。

```python
import numpy as np

# 定义两个矩阵
matrix_a = np.array([[1, 2], [3, 4]])
matrix_b = np.array([[5, 6], [7, 8]])

# 使用 @ 运算符进行矩阵乘法
result_matrix = matrix_a @ matrix_b

print(result_matrix)
```

如果不使用 `@` 运算符，我们也可以使用 `numpy` 的 `dot` 函数来完成矩阵乘法。

```python
import numpy as np

# 定义两个矩阵
matrix_a = np.array([[1, 2], [3, 4]])
matrix_b = np.array([[5, 6], [7, 8]])

# 使用 dot 函数进行矩阵乘法
result_matrix = np.dot(matrix_a, matrix_b)

print(result_matrix)
```

> 在使用 `@` 运算符时，确保操作数是可以进行矩阵乘法的矩阵或数组，其维度要满足矩阵乘法的要求，即第一个矩阵的列数要等于第二个矩阵的行数。如果维度不匹配，会导致 `ValueError` 错误。

2、常用内置运算符

- 算术：`+`, `-`, `*`, `@`, `/`, `//`, `**`, `%`, `-` (一元算符), `+` (一元算符)
- 关系：`<`, `<=`, `>=`, `>`, `==`, `!=`
- 赋值： `+=`, `-=`, `*=`, `/=`, `//=`, `**=`, `%=`
- 逻辑：`and`, `or`, `not`

3、算术运算符优先级

| 运算符 |           说明           |
| :----: | :----------------------: |
|   +    |            加            |
|   -    |            减            |
|   *    |            乘            |
|   /    |            除            |
|   %    |  求余，即返回除法的余数  |
|   //   | 整除，即返回商的整数部分 |
|   **   |    幂，即返回x的y次方    |

算术运算符的优先级顺序：

- 首先，计算括号内的表达式，使用 `()`。
- 其次，进行幂运算 `**`。
- 然后，从左到右依次进行乘法 `*`、除法 `/`、取模 `%`、整除 `//` 运算。
- 最后，从左到右依次进行加法 `+` 和减法 `-` 运算。

> 在使用运算符时，要确保操作数的类型是兼容的。例如，不能将**字符串**和**数字**`直接使用`算术运算符进行运算，除非进行适当的类型转换。

4、`math` 库中的一些数学常量

$$
pi，数学常数 \pi = 3.141592...
$$

$$
e，数学常数 \mathrm{e} = 2.718281...
$$

$$
tau，数学常数 \tau = 6.283185...
$$

$$
inf，浮点正无穷大，等价于 float('inf')，负无穷大使用 -math.inf
$$



### 参考资料

- [Github - 聪明办法学 python（第二版）- 数据类型和操作](https://github.com/datawhalechina/learn-python-the-smart-way-v2/blob/main/slides/chapter_2-Data_Types_and_Operators.ipynb)
- [聪明办法学 python（第二版）视频资料 - Chap2 数据类型和操作](https://www.bilibili.com/video/BV1Ju4y1B73m?spm_id_from=333.788.videopod.sections)
