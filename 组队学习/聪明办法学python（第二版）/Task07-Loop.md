<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>聪明办法学 python（第二版）<br/><span>Task 06：循环</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年1月22日
</div>
<div>
 <!--  
    Author: zps1011 （欢乐一天）  
    Date: 2025-1-22 
    -->  
&nbsp;&nbsp;&nbsp;
</div>



**作业系统个人主页链接：https://hydro.ac/d/datawhale_p2s/user/48019**

### 第一题：N*M Matrix

题目描述：输入正整数`n`和`m`，需要编写程序来输出一个 n\*m 的全 1 矩阵。

输入格式：一个整型数 `n` 和一个整型数 `m`，用逗号隔开。

输出格式：输出一个 n\*m 的全 1 矩阵。

输入样例

```bash
2,2
```

输出样例

```bash
1 1
1 1
```

提示说明

- 由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
n, m = ast.literal_eval(input())
# 现在程序中有两个变量 n, m
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
n, m = ast.literal_eval(input())

# 嵌套循环
for i in range(n):
    for j in range(m):
        print(1,end = ' ')
    print()

# 列表推导式
'''
s = [[1] * m for _ in range(n)]
for row in s:
    print(*row)
'''
```

```python
# 方法二
def main():
    # 从标准输入读取输入的字符串
    input_str = input()
    # 将输入字符串按逗号分割并转换为整数
    n, m = map(int, input_str.split(','))
    # 循环生成矩阵
    for i in range(n):
        # 生成一行元素，元素之间用空格分隔
        row = ' '.join(['1'] * m)
        print(row)

if __name__ == "__main__":
    main()
```

> - `row = ' '.join(['1'] * m)`：在每一行中，使用列表推导式 `['1'] * m` ，生成一个包含 `m` 个 `1` 的列表，然后使用 `join` 方法将列表元素用空格连接起来形成字符串 `row`。



### 第二题：ReverseNumber

题目描述：给定一个自然数`n`，你需要编写程序，来翻转自然数`n`。

输入格式：一个整型数`n`。

输出格式：一个整型数，该数应该是输入数的一个翻转。

输入样例

```bash
9870
```

输出样例

```bash
789
```


提示说明：

- 在本题中不要使用**字符串索引**、**字符串方法**、**列表**、**列表索引**或**递归**。我们无法技术上强制约束你使用这些特性，请**自觉遵守学术诚信规定。** 助教会检查提交的代码，若**不符合本条要求，成绩会被取消**。
- 有兴趣的同学可以尝试分别使用`for`、`while`循环的方式来完成此题。
- 由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
n = ast.literal_eval(input())
# 现在程序中有一个变量n
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

- 以下是使用`题目模版`实现的代码

  ```python
  import ast
  n = ast.literal_eval(input())
  
  reverse_num = 0
  while n > 0:
      digit = n % 10
      reverse_num = reverse_num * 10 + digit
      n = n // 10
  
  print(reverse_num)
  
  # 字符串方法
  '''
  reverse_num = int(str(n)[::-1])
  print(reverse_num)
  '''
  ```

  

- 以下是使用 `for` 循环实现的代码

  ```python
  def main():
      n = int(input())
      num_str = str(n)
      length = len(num_str)
      reversed_num = 0
      for i in range(length):
          # 获取最后一位数字
          digit = n % 10
          # 将数字添加到结果中
          reversed_num += digit * 10 ** (length - 1 - i)
          # 去掉最后一位数字
          n = n // 10
      print(reversed_num)
  
  if __name__ == "__main__":
      main()
  ```
  
- 以下是使用 `while` 循环实现的代码

  ```python
  def main():
      n = int(input())
      reversed_num = 0
      while n > 0:
          # 获取最后一位数字
          last_digit = n % 10
          # 将最后一位数字添加到结果中
          reversed_num = reversed_num * 10 + last_digit
          # 去掉最后一位数字
          n = n // 10
      print(reversed_num)
  
  if __name__ == "__main__":
      main()
  ```
  
  > - `reversed_num = 0`：初始化一个变量 `reversed_num` 用于存储反转后的数字，初始值为 0。
  > - `last_digit = n % 10`：使用取模运算符 `%` 获取 `n` 的最后一位数字。
  > - `reversed_num = reversed_num * 10 + last_digit`：将 `last_digit` 加到 `reversed_num` 的末尾。将 `reversed_num` 乘 10 是为了将其原有的数字向左移动一位，以便添加新的数字。
  > - `n = n // 10`：使用整除运算符 `//` 去掉 `n` 的最后一位数字。




### 第三题：hasConsecutiveDigits

题目描述：给定一个整数 `n` (有可能是正数，也有可能是负数)，你需要判断这个数中是否存在两个相同的连续数字。

输入格式：一个整型数`n`。

输出格式：一个布尔类型的数，若存在返回 `True`，否则，返回 `False`。

输入样例 1

```bash
0
```

输出样例 1

```bash
False
```

输入样例 2

```bash
3003
```

输出样例 2

```bash
True
```

提示说明：

- 在本题中在本题中不要使用**字符串索引**、**字符串方法**、**列表**、**列表索引**或**递归**。我们无法技术上强制约束你使用这些特性，请**自觉遵守学术诚信规定。** 助教会检查提交的代码，若**不符合本条要求，成绩会被取消**。
- 由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
n = ast.literal_eval(input())
# 现在程序中有一个变量 n
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
n = ast.literal_eval(input())

def integer_num(num):

# 负数处理情况，将其转换为正数
    if num < 0:
        num = -num

# prev_digit 用于存储前一位数字
    prev_num = None

# 当 n > 0 时，可以避免负号（'-'）的干扰，继续循环
    while num > 0:
        digit = num % 10  # 取出num的最后一位数字

# 检查当前数字和前一位数字是否相同
        if prev_num is not None and digit == prev_num:
            return True
# 将 prev_num 更新为当前数字，以便和下一位数字进行比较
        prev_num = digit
# 移除最后一位数字
        num = num // 10   
    return False

print(integer_num(n))
```



```python
# 方法二
def main():
    n = int(input())
    prev_digit = None
    num = abs(n)
    while num > 0:
        # 获取当前数字的最后一位
        cur_digit = num % 10
        if prev_digit == cur_digit:
            return True
        prev_digit = cur_digit
        # 去掉最后一位数字
        num = num // 10
    return False

if __name__ == "__main__":
    print(main())
```

> - `prev_digit = None`：初始化 `prev_digit` 为 `None`，用于存储前一个数字。
> - `num = abs(n)`：将 `n` 的绝对值存储在 `num` 中，因为我们只关心数字本身的数字，而不关心正负。
> - `num, cur_digit = divmod(num, 10)`：使用 `divmod` 函数同时计算 `num` 除以 10 的商和余数，余数是当前数字。
> - `if prev_digit == cur_digit`：如果前一个数字等于当前数字，返回 `True`，表示存在相同的连续数字。
> - `prev_digit = cur_digit`：将当前数字存储为前一个数字，以便下一次比较。



### 第四题：nthPalindromicPrime

题目描述：**质数**（Prime number），又称**素数**，指在大于1的自然数中，除了 1 和该数自身外，无法被其他自然数整除的数（也可定义为只有 1 与该数本身两个因数的数）。**回文数**或**迴文数**是指一个像 14641 这样“对称”的数。同时满足**质数**和**回文数**的条件的数被称为回文素数。在本题中我们会输入一个整型数 `n`， 你需要编写程序，来返回第 n 个回文素数的值。前十个回文素数如下：2, 3, 5, 7, 11, 101, 131, 151, 181, 191。

输入格式：一个整型数 `n`。

输出格式：一个整型数。

输入样例

```bash
0
```

输出样例

```bash
2
```

提示说明

- 本题中 `n` 的范围为：**0 ≤ n < 21**
- 供参考的**回文素数表**如下：2, 3, 5, 7, 11, 101, 131, 151, 181, 191, 313, 353, 373, 383, 727, 757, 787, 797, 919, 929, 10301, 10501, 10601, 11311；
- 在本题中在本题中不要使用**字符串索引**、**字符串方法**、**列表**、**列表索引**或**递归**。我们无法技术上强制约束你使用这些特性，请**自觉遵守学术诚信规定。** 助教会检查提交的代码，若**不符合本条要求，成绩会被取消**。
- 由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```python
import ast
n = ast.literal_eval(input())
# 现在程序中有一个变量n
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
n = ast.literal_eval(input())

def is_prime(num):  
    # 检查一个数是否是素数。如果传入的数小于2，直接返回False，因为小于2的数不是素数
    if num < 2:  
        return False
    
    # 循环从2开始，到num的平方根（向上取整）结束，因为任何大于num平方根的因子必然有小于或等于平方根的配对因子。
    for i in range(2, int(num ** 0.5) + 1):
        # 如果num能被i整除，则num不是素数，返回False
        if num % i == 0:  
            return False
    # 如果循环结束都没有返回False，则num是素数，返回True
    return True  

# 检查一个数是否是回文数
def is_palindrome(num):
    # 将num转换为字符串，并与它的反转字符串比较，如果相等则是回文数，返回 True，否则返回 False
    return str(num) == str(num)[::-1]  

# 返回第n个回文素数的值 
def nth_palindrome_prime(n):  
    # 初始化计数器，用于记录找到的回文素数的数量 
    count = 0
    # 从2开始检查，因为2是最小的素数
    num = 2
    
    while count < n:  # 当找到的回文素数数量小于 n 时，继续循环  
        if is_prime(num) and is_palindrome(num):  # 如果当前数是回文素数  
            count += 1  # 计数器加1  
        num += 1  # 检查下一个数  
    return num -1  # 返回找到的第 n 个回文素数
 
result = nth_palindrome_prime(n + 1)
print(result)
```



```python
# 方法二
def is_prime(num):
    # 如果数字小于等于 1，则不是质数，返回 False
    if num <= 1:  
        return False
    
    # 2 和 3 是质数，返回 True
    if num <= 3:
        return True
    
    # 如果能被 2 或 3 整除，则不是质数，返回 False
    if num % 2 == 0 or num % 3 == 0:
        return False
    
    # 从 5 开始检查
    i = 5
    # 检查到 i 的平方小于等于 num
    while i * i <= num:
        # 检查是否能被 i 或 i + 2 整除，因为大于 3 的质数可以表示为 6k ± 1 的形式。
        if num % i == 0 or num % (i + 2) == 0:
            return False
        # 每次加 6 检查下一组可能的因数
        i += 6
    return True

def is_palindrome(num):
    original = num
    reversed_num = 0
    while num > 0:
        last_digit = num % 10
        reversed_num = reversed_num * 10 + last_digit
        num = num // 10
    return original == reversed_num

def main():
    n = int(input())
    count = 0
    num = 2
    while True:
        if is_prime(num) and is_palindrome(num):
            count += 1
        if count == n + 1:
            break
        num += 1
    print(num)

if __name__ == "__main__":
    main()
```

> - is_palindrome(num) 函数：
>   - `original = num`：存储原始数字。
>   - `reversed_num = 0`：存储反转后的数字。
>   - `while num > 0`：将数字反转。
>   - `last_digit = num % 10`：获取最后一位数字。
>   - `reversed_num = reversed_num * 10 + last_digit`：将最后一位数字添加到反转数字中。
>   - `num = num // 10`：去掉最后一位数字。
>   - `return original == reversed_num`：检查原始数字和反转数字是否相等。
> - main() 函数：
>   - `count = 0`：计数回文素数的数量。
>   - `num = 2`：从 2 开始检查。
>   - `while True`：不断检查数字。
>   - `if is_prime(num) and is_palindrome(num)`：如果是回文素数，计数加 1。
>   - `if count == n + 1`：当计数达到 `n + 1` 时，找到第 `n` 个回文素数，停止循环。
>   - `num += 1`：检查下一个数字。



### 第五题：carrylessAdd

题目描述：众所周知，我们常见的加法规则是类似与 8 + 7 = 15 这种，但是现在我们需要设计一种全新的加法运算规则：忽略进位的加法计算。例如输入18 和 27，答案会是 35，而非正常的 45。输入两个正整数 `x1`和`x2`，返回此方法下计算后的结果。

输入格式：两个整型数`x1`和`x2`，用逗号隔开。

输出格式：一个整型数。

输入样例

```bash
785,376
```

输出样例

```bash
51
```

提示说明：

- 在本题中在本题中不要使用**字符串索引**、**字符串方法**、**列表**、**列表索引**或**递归**。我们无法技术上强制约束你使用这些特性，请**自觉遵守学术诚信规定。** 助教会检查提交的代码，若**不符合本条要求，成绩会被取消**。
- 由于难度问题，我们这里给出程序模板，请**在注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头**。

模板：

```bash
import ast
x1, x2 = ast.literal_eval(input())
# 现在程序中有两个变量x1, x2
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
import ast
x1, x2 = ast.literal_eval(input())

def ignore_carry_addition(x1, x2):  
    # 将两个整数转换为字符串，方便逐位处理  
    str_x1 = str(x1)  
    str_x2 = str(x2)  
      
    # 确保两个字符串长度相同，较短的字符串前面补0  
    max_len = max(len(str_x1), len(str_x2))  
    str_x1 = str_x1.zfill(max_len)  
    str_x2 = str_x2.zfill(max_len)  
      
    # 初始化结果字符串  
    result_str = ""  
      
    # 逐位进行加法运算，并忽略进位  
    for i in range(max_len - 1, -1, -1):  
        # 获取当前位的数字字符，并转换为整数  
        digit1 = int(str_x1[i])  
        digit2 = int(str_x2[i])  
          
        # 对当前位的数字进行加法运算，需要%10，因为9+9=18，18%10=8  
        sum_digit = (digit1 + digit2) % 10
          
        # 将加法结果转换为字符串，并添加到结果字符串中  
        result_str = str(sum_digit) + result_str  
      
    # 将结果字符串转换为整数并返回  
    return int(result_str)   
 
print(ignore_carry_addition(x1, x2))
```

```python
# 方法二
def main():
    # 从标准输入读取输入的字符串，并将其按逗号分割，然后转换为整数
    x1, x2 = map(int, input().split(','))
    result = 0
    base = 1
    while x1 or x2:
        # 取出 x1 和 x2 的最后一位
        digit1 = x1 % 10
        digit2 = x2 % 10
        # 计算忽略进位的和
        digit_sum = (digit1 + digit2) % 10
        # 将结果累加到 result 中
        result += digit_sum * base
        # 去掉 x1 和 x2 的最后一位
        x1 //= 10
        x2 //= 10
        base *= 10
    print(result)

if __name__ == "__main__":
    main()
```

> - `result = 0`：初始化结果为 0。
> - `base = 1`：用于记录当前位的权重，从个位开始，权重为 1，十位为 10，百位为 100，以此类推。
> - `while x1 or x2`：只要 `x1` 或 `x2` 不为 0，就继续执行循环。
>   - `digit1 = x1 % 10` 和 `digit2 = x2 % 10`：使用取模运算符 `%` 分别取出 `x1` 和 `x2` 的最后一位数字。
>   - `digit_sum = (digit1 + digit2) % 10`：计算这两个数字相加忽略进位的结果，通过取模 10 实现。
>   - `result += digit_sum * base`：将计算结果乘以当前位的权重并累加到结果中。
>   - `x1 //= 10` 和 `x2 //= 10`：使用整除运算符 `//` 去掉 `x1` 和 `x2` 的最后一位数字。
>   - `base *= 10`：更新下一位的权重，将其乘以 10。
>
> - 上述代码通过不断取 `x1` 和 `x2` 的最后一位，计算忽略进位的和，并将结果累加到 `result` 中，最终得到忽略进位的加法结果。

### 附录

1、for 循环

- 从数据集合中`逐一`提取元素，放在`循环变量`中，对于每个提取的元素执行一次语句块。for语句的循环执行次数是根据集合中元素个数确定的。

  > 在 python 中，for 循环执行速度较慢，耗时较长。

- 语法格式

  ```python
  for 变量名 in 数据集合:
      <语句>
  ```

- 遍历方式

  ```python
  for <循环变量> in <字符串变量>:
      <语句块>
  ```

  ```python
  for <循环变量> in range(<循环次数>):
      <语句块>
  ```

2、while 循环

- 程序执行 while 语句时，判断条件，若为 True，执行循环体语句，语句结束后返回再次判断条件；直到条件为 False 时，循环终止，执行与 while 同级别的后续语句。

  > while 适用于无法确定循环次数的情况下适用。

- 语法格式

  ```python
  while <条件>:
      <代码块>
  ```

3、break 与 continue

- break 语句 ：跳出`最内层`循环，终止该循环后，**从循环后的代码继续执行**。
- continue 语句：用来结束`当前当次`循环，即跳出循环体中下面尚未执行的语句，但**不跳出当前循环**。

4、for 循环的扩展模式

- 当 for 循环正常执行之后，程序会继续执行 else 语句中内容。else 语句`只在`循环`正常执行`之后才执行并结束。

- 语法格式

  ```python
  for 变量名 in 集合:
      <语句块1>
  else:
      <语句块2>
  ```

  ```python
  # 实例
  for i in "PY":
      print("循环执行中:" + i)
  else:
      i = "循环正常结束"
      
  print(i)
  
  '''
  运行结果如下：
  循环执行中:P
  循环执行中:Y
  循环正常结束
  '''
  ```

5、while 循环的扩展模式

- 当 while 循环正常执行之后，程序会继续执行 else 语句中内容。else 语句`只在`循环`正常执行`之后才执行并结束。

- 语法格式

  ```python
  while <条件>:
      <语句块1>
  else:
      <语句块2>
  ```

  ```python
  # 实例
  n = 0
  while n < 10:
  	n = n + 3
  	print(n)
  	
  else:
  	print("循环正常结束")
      
  '''
  运行结果如下：
  3
  6
  9
  12
  循环正常结束
  '''
  ```

6、range() 函数

- range() 函数是 python 的内置函数，常在 for 循环使用。
- 在 range(start, stop, [step]) 中，我们可以传递三个参数。
  - `start`：计数从 start 开始。默认是从 0 开始。例如 range(5) 等价于 range(0， 5)
  - `stop`：计数到 stop 结束，但**不包括** stop。例如：range(0， 5) 是 [0, 1, 2, 3, 4] ，没有 5
  - `step`：步长，默认为 1。例如：range(0， 5) 等价于 range(0, 5, 1)



### 参考资料

- [Github - 聪明办法学 python（第二版）- 循环 Loop](https://github.com/datawhalechina/learn-python-the-smart-way-v2/blob/main/slides/chapter_5-Loop.ipynb)
- [聪明办法学 python（第二版）视频资料 - Chap5 循环](https://www.bilibili.com/video/BV1Tk4y1c7fX?spm_id_from=333.788.videopod.sections)
- [菜鸟教程 - Python3 循环语句](https://www.runoob.com/python3/python3-loop.html)
- [菜鸟教程 - Python3 range() 函数用法](https://www.runoob.com/python3/python3-func-range.html)

