<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>聪明办法学 python（第二版）<br/><span>Task 04：条件</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年1月20日
</div>
<div>
 <!--  
    Author: zps1011 （欢乐一天）  
    Date: 2025-1-20 
    -->  
&nbsp;&nbsp;&nbsp;
</div>


**作业系统个人主页链接：https://hydro.ac/d/datawhale_p2s/user/48019**

### 第一题：Output Letter Grade by Score

题目描述：请编写一个程序，提示用户输入一个分数，并将分数转换为一个字母等级（`A`、`B`、`C`），并在屏幕上输出相应的消息，当分数超出正常范围(<0||>100)时，应输出`error`

输入格式：一个整数，表示用户的分数。

输出格式：一个字符，表示用户的等级，`A`、`B`、`C` 当输入超出范围(<0||>100)时 输出 `error`

样例输入-1

```bash
93
```

样例输出-1

```bash
A
```

样例输入-2

```bash
103
```

样例输出-2

```bash
error
```

提示：以下是各等级的分数对应表。

| 字母等级 | 分数区间 |
| :------: | :------: |
|    A     |  80~100  |
|    B     |  60~79   |
|    C     |   0~59   |

由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
x1 = int(input())
# 现在程序中有一个变量x1

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
def main():
    try:
        score = int(input())     # 获取用户输入的分数并转换为整数
        if 80 <= score <= 100:   # 分数在 80 到 100 之间，等级为 A
            print("A")
        elif 60 <= score <= 79:  # 分数在 60 到 79 之间，等级为 B
            print("B")
        elif 0 <= score <= 59:   # 分数在 0 到 59 之间，等级为 C
            print("C")
        else:                    # 分数超出 0 到 100 的范围，输出 error
            print("error")
    except ValueError:           # 处理输入不是整数的情况
        print("请输入一个有效的整数分数。")


if __name__ == "__main__":
    main()
```

> - 首先，使用 `input` 函数获取用户输入的分数，并使用 `int` 函数将其转换为整数存储在变量 `score` 中。
> - 然后，使用 `if-elif-else` 条件判断语句来确定分数所属的等级范围。
> - 同时，使用 `try-except` 结构处理用户输入的不是整数的情况，若发生 `ValueError`，会输出 `请输入一个有效的整数分数。`
> - 最后，通过 `if __name__ == "__main__":` 确保代码作为脚本运行时调用 `main` 函数，避免作为模块导入时自动执行。

```python
# 方法二
x1 = int(input())

if 80 <= x1 <= 100:
    print("A")
elif 60 <= x1 <= 79:
    print("B")
elif 0 <= x1 <= 59:
    print("C")
else:
    print("error")
```



### 第二题：getInRange

题目描述：输入3个整数——x，bound1 和 bound2 ，其中 bound1 不一定小于 bound2 。如果 x 在两个边界之间，则输出 x 。否则，如果 x 小于下限，则输出下限，或者如果 x 大于上限，则输出上限。

输入格式：输入三个正整数，分别表示 x，bound1 和 bound2 ，以`,`隔开。

输出格式：如果 x 在两个边界之间，则输出 x 。否则，如果 x 小于下限，则输出下限，或者如果 x 大于上限，则输出上限。

样例输入-1

```bash
1,3,5
```

样例输出-1

```bash
3
```

样例输入-2

```bash
4,3,5
```

样例输出-2

```bash
4
```

提示：由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
x, bound1, bound2 = ast.literal_eval(input())

# 现在程序中有三个变量 x, bound1, bound2

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
x, bound1, bound2 = ast.literal_eval(input())

s1 = min(bound1, bound2)
s2 = max(bound1, bound2)

if s1 <= x <= s2:
    print(x)
elif x < s1:
    print(s1)
else:
    print(s2)
```

```python
# 方法二
def main():
    input_str = input() 
    x, bound1, bound2 = map(int, input_str.split(','))  # 将输入的字符串分割并转换为整数
    lower_bound = min(bound1, bound2)    # 找出下限
    upper_bound = max(bound1, bound2)    # 找出上限
    if lower_bound <= x <= upper_bound:  # 判断 x 是否在上下限范围内
        print(x)
    elif x < lower_bound:                # 如果 x 小于下限，输出下限
        print(lower_bound)
    else:                                # 如果 x 大于上限，输出上限
        print(upper_bound)


if __name__ == "__main__":
    main()
```



### 第三题：Is Point Inside Square

题目描述：有一个正方形，四个角的坐标 (x, y) 分别是 (1, −1)、(1, 1)、(−1, −1)、(−1, 1)，x 是横轴，y 是纵轴。写一个程序，判断一个给定的点是否在这个正方形内（包括正方形边界）。

输入格式：输入一行，包括两个整数 x, y，以一个`,`分开，表示坐标 (x, y) 。

输出格式：输出一行，如果点在正方形内，则输出 `True`，否则输出 `False`。

输入样例-1

```bash
1,1
```

输出样例-1

```bash
True
```

提示说明：由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
n, m=ast.literal_eval(input())
# 现在程序中有两个变量n, m

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
n, m=ast.literal_eval(input()) # (n,m)

# 检查 n 和 m 是否在 -1 到 1 的范围内
if -1 <= n <= 1 and -1 <= m <= 1:
    print('True')
else:
    print('False')
```

```python
# 方法二
def main():
    input_str = input() 
    x, y = map(int, input_str.split(','))  # 将输入的字符串按逗号分割并转换为整数
    
    # 判断点是否在正方形范围内
    if -1 <= x <= 1 and -1 <= y <= 1:
        print("True")
    else:
        print("False")


if __name__ == "__main__":
    main()
```



### 第四题：Check Leap Year

题目描述：输入一个年份，判断这一年是否是闰年，如果是输出 `True`，否则输出 `False`。

输入格式：输入一个正整数 `x`，表示年份。

输出格式：输出一行。如果输入的年份是闰年则输出 `True`，否则输出 `False`。

样例输入-1

```bash
1944
```

样例输出-1

```bash
True
```

样例输入-2

```bash
1900
```

样例输出-2

```bash
False
```

提示：

**闰年**：

- **四年一闰百年不闰**：即如果 year 能够被 4 整除，但是不能被 100 整除，则 year 是闰年。
- **每四百年再一闰**：如果 year 能够被 400 整除，则 year 是闰年。

数据保证，1582 ≤ n ≤ 2020 且年份为自然数。

由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
x = int(input())
# 现在程序中有一个变量x

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**
```python
# 方法一
x = int(input())

if (x % 4 == 0 and x % 100 != 0) or (x % 400 == 0):
    print("True")
else:
    print("False")
```

```python
# 方法二
def main():
    year = int(input())  # 获取用户输入的年份并转换为整数
    # 判断是否为闰年。满足四年一闰百年不闰或每四百年再一闰的条件
    if (year % 4 == 0 and year % 100!= 0) or year % 400 == 0: 
        print("True")
    else:
        print("False")


if __name__ == "__main__":
    main()
```



### 第五题：Days in Month

题目描述：输入年份和月份，输出这一年的这一月有多少天。需要考虑闰年。

输入格式：输入两个正整数，分别表示年份 y 和月数 m ，以`,`隔开。

输出格式：输出一行一个正整数，表示这个月有多少天。

样例输入-1

```bash
1926,8
```

样例输出-1

```bash
31
```

样例输入-2

```bash
2000,2
```

样例输出-2

```bash
29
```

提示：

数据保证 1583 ≤ y ≤ 2020，1 ≤ m ≤ 12。你可以借鉴`Check Leap Year`中的闰年判断方法。

由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
y, m=ast.literal_eval(input())
# 现在程序中有两个变量y, m

# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
y, m=ast.literal_eval(input())

# 判断是否为闰年
if ((y % 4 == 0 and y % 100 != 0) or (y % 400 == 0)) and m == 2:
    days = 29

else:
    # 设置平年的月份及天数
    days_in_mouth = {
        1:31,2:28,3:31,4:30,5:31,6:30,7:31,8:31,9:30,10:31,11:30,12:31
        }
    days = days_in_mouth[m]

# 打印天数
print(days)
```

```python
# 方法二
def main():
    input_str = input() 
    year, month = map(int, input_str.split(','))  # 将输入的字符串按逗号分割并转换为整数
    days_in_month = 0                     # 存储该月的天数；初始化该月的天数为 0
    if month in [1, 3, 5, 7, 8, 10, 12]:  # 大月有 31 天
        days_in_month = 31
    elif month in [4, 6, 9, 11]:          # 小月有 30 天
        days_in_month = 30
    elif month == 2:                      # 对于 2 月，需要判断是否为闰年
        if (year % 4 == 0 and year % 100!= 0) or year % 400 == 0: 
            days_in_month = 29
        else:
            days_in_month = 28
    print(days_in_month)


if __name__ == "__main__":
    main()
```

在本节课中，我们初步接触到了 if-else 推导式，以下代码结合了相关知识点，以及`三元表达式`进行练习。

```python
# 方法三
def main():
    year, month = map(int, input().split(','))
    days = 31 if month in [1, 3, 5, 7, 8, 10, 12] else 30 if month!= 2 else (29 if (year % 4 == 0 and year % 100!= 0) or year % 400 == 0 else 28)
    print(days)

if __name__ == "__main__":
    main()
```



### 附录

1、单分支结构：分支结构是程序根据条件判断结果而选择不同向前执行路径的一种运行方式。其语法格式为：

```python
if <条件>:
    <语句块>
```

> 案例：输入学生分数，如果大于60分，则提醒学生考试通过，并输出具体的分数。

2、二分支结构：是根据条件判断的不同而选择不同执行路径的一种分支结构。其语法格式为：

```python
if <条件>:
    <语句块_1>
else:
    <语句块_2>
```

> 案例：输入学生分数，如果大于60分，则提醒学生考试通过，否则提醒学生考试不及格。

3、多分支结构：一般用于判断同一个条件或一类条件的多个执行路径。依次判断条件并执行第一个条件为 True 的语句。若没有条件成立，执行 else 下面的语句。其语法格式为：

```python
if <条件_1>:
	<语句块_1>
elif <条件_2>:
	<语句块_2>
elif <条件_3>:
	<语句块_3>
......
else:
	<语句块_N>
```

> 输入学生分数，将学生分数转换为等级：
>
> - 90-100--->A
> - 80-90 --->B
> - 70-80 --->C
> - 60-70 --->D
> - 0-60 --->E

4、match-case 语句

- match-case 语法‌是 **Python 3.10 及更高版本**中引入的一种新的结构化控制流语句，用于替代传统的 if-elif-else 结构，提供了一种更清晰和优雅的方式来处理多值判断。其语法格式为：

```python
match subject:
    case <pattern_1>:
        <action_1>
    case <pattern_2>:
        <action_2>
    case <pattern_3>:
        <action_3>
    case _:
        <action_wildcard>
```

> 参数说明：
>
> - `match`语句后跟一个表达式，然后使用`case`语句来定义不同的模式。
> - `case`后跟一个模式，可以是具体值、变量、通配符等。
> - 可以使用`if`关键字在`case`中添加条件。
> - `_`通常用作通配符，匹配任何值。

- 实例

  ```python
  def match_example(value):
      match value:
          case 1:
              print("匹配到值为1")
          case 2:
              print("匹配到值为2")
          case _:
              print("匹配到其他值")
  
  match_example(1)  # 输出: 匹配到值为1
  match_example(2)  # 输出: 匹配到值为2
  match_example(3)  # 输出: 匹配到其他值
  ```

  

### 参考资料

- [Github - 聪明办法学 python（第二版）- 条件](https://github.com/datawhalechina/learn-python-the-smart-way-v2/blob/main/slides/chapter_4-Conditionals.ipynb)
- [聪明办法学 python（第二版）视频资料 - Chap4 条件](https://www.bilibili.com/video/BV1k94y1z7MY?spm_id_from=333.788.videopod.sections)
- [菜鸟教程 - Python 条件语句](https://www.runoob.com/python/python-if-statement.html)
- [菜鸟教程 - Python3 条件控制](https://www.runoob.com/python3/python3-conditional-statements.html)
- [菜鸟教程 - Python match...case 语句](https://www.runoob.com/python3/python-match-case.html)
- [菜鸟教程 - Python 推导式](https://www.runoob.com/python3/python-comprehensions.html)
- [菜鸟教程 - python 列表推导式](https://www.runoob.com/w3cnote/list-comprehension.html)
- [初学者必知的Python中优雅的用法](https://www.runoob.com/w3cnote/python-start-skill.html)
- [HTTP 响应状态码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)


