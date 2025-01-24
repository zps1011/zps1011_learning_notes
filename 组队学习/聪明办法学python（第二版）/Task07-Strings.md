<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>聪明办法学 python（第二版）<br/><span>Task 07：</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年1月25日
</div>
<div>
 <!--  
    Author: zps1011 （欢乐一天）  
    Date: 2025-1-25 
    -->  
&nbsp;&nbsp;&nbsp;
</div>





**作业系统个人主页链接：https://hydro.ac/d/datawhale_p2s/user/48019**

### 第一题：Find You

题目描述：输入一串字符串`S`和一个单词`W`，要求编写程序，判断单词`W`是否存在于字符串`S`当中，并输出响应内容。

输入格式：一串非空字符串`S`和一个单词`W`，中间以逗号隔开。

输出格式：详见输入输出样例。

输入样例-1

```bash
crazyThurvme50,me
```

输出样例-1

```bash
The starting point of crazyThurvme50 is 11
```

输入样例-2

```bash
crazyThurvme50,KFC
```

输出样例-2

```bash
not found
```

提示说明：由于难度问题，我们这里给出程序模板，请在 **注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头。**

模板：

```python
s,w = input().split(",")
# 现在程序中有两个变量，s和w
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
# 读取输入并按逗号分割
s,w = input().split(",")

if w in s:
    index = s.index(w)
    print(f'The starting point of {s} is {index + 1}')
else:
    print('not found')
```

```python
# 方法二
def main():
    S, W = input().split(',')
    i = 0
    while i <= len(S) - len(W):
        j = 0
        while j < len(W):
            if S[i + j] != W[j]:
                break
            j = j + 1
        if j == len(W):
            print(f"The starting point of {S} is {i + 1}")
            return
        i = i + 1
    print("not found")


if __name__ == "__main__":
    main()
```

> - 外层 `while` 循环 `while i <= len(S) - len(W)`：遍历字符串 `S` 中所有可能的起始位置。
> - 内层 `while` 循环 `while j < len(W)`：从当前起始位置开始，逐字符比较 `S` 和 `W` 的字符。
> - `if S[i + j] != W[j]`：如果发现不匹配的字符，则跳出内层循环。
> - `if j == len(W)`：如果内层循环正常结束（即 `j` 等于 `W` 的长度），说明找到了完整的单词 `W`，打印起始位置并返回。

### 第二题：isPalindrome

题目描述：输入三串字符串(逗号隔开)`str1`,`str2`,`str3`，要求编写程序，判断这些字符串是否为回文字符串，如果是则输出`T`，否则输出`F`。

输入格式：三串非空字符串，中间以逗号空格开。

输出格式：输出`T`或者`F`，中间以逗号空格开。

输入样例-1

```bash
abc,bbb,abcdcba
```

输出样例-1

```bash
F,T,T
```

输入样例-2

```bash
111,yyds,dddd
```


输出样例-2

```bash
T,F,T
```

提示说明：由于难度问题，我们这里给出程序模板，请在**注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头。**

模板：

```python
str1,str2,str3 = input().split(",")
# 现在程序中有三个字符串，str1,str2,str3
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
str1,str2,str3 = input().split(",")

result1 = 'T' if str1 == str1[::-1] else 'F'
result2 = 'T' if str2 == str2[::-1] else 'F'
result3 = 'T' if str3 == str3[::-1] else 'F'

print(result1 + ',' + result2 + ',' + result3)
```

```python
# 方法二
def is_palindrome(s):
    left = 0
    right = len(s) - 1
    while left < right:
        if s[left] != s[right]:
            return False
        left = left + 1
        right = right - 1
    return True


def main():
    str1, str2, str3 = input().split(',')
    result1 = 'T' if is_palindrome(str1) else 'F'
    result2 = 'T' if is_palindrome(str2) else 'F'
    result3 = 'T' if is_palindrome(str3) else 'F'
    print(f"{result1},{result2},{result3}")


if __name__ == "__main__":
    main()
```

> **`is_palindrome` 函数**：
>
> - 该函数用于判断一个字符串是否为回文。
> - 使用双指针法，初始化两个指针 `left` 和 `right` 分别指向字符串的开头和结尾。
> - 在 `while` 循环中，只要 `left` 小于 `right`，就比较这两个指针所指向的字符。如果不相等，说明不是回文，返回 `False`。
> - 若循环结束都未发现不相等的情况，说明是回文，返回 `True`。
>
> **`main` 函数**：
>
> - `str1, str2, str3 = input().split(',')`：从标准输入读取一行字符串，使用逗号将其分割为三部分，分别赋值给 `str1`、`str2` 和 `str3`。
> - 分别调用 `is_palindrome` 函数判断这三个字符串是否为回文，根据结果将对应的字符 `'T'` 或 `'F'` 赋值给 `result1`、`result2` 和 `result3`。
> - 最后使用格式化字符串将三个结果以 `{result1},{result2},{result3}` 的形式输出。

### 第三题：New String

题目描述：输入三串字符串(逗号隔开)`str1`,`str2`,`str3`，要求编写程序，提取每串字符串最末端一个字符，合并组成一个新的字符串并输出。

输入格式：三串非空字符串，中间以逗号空格开。

输出格式：组成后的新字符串。

输入样例-1

```bash
APP,222,CS
```

输出样例-1

```bash
P2S
```

输入样例-2

```bash
EFG,MOP,RST
```

输出样例-2

```bash
GPT
```

提示说明：由于难度问题，我们这里给出程序模板，请在**注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头。**

模板：

```python
str1,str2,str3 = input().split(",")
# 现在程序中有三个字符串，str1,str2,str3
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 不使用函数表示
str1,str2,str3 = input().split(",")
result = str1[-1] + str2[-1] + str3[-1]
print(result)
```

```python
# 使用函数表示
def main():
    # 读取输入的字符串并按逗号分割
    str1, str2, str3 = input().split(',')
    # 提取每个字符串的最后一个字符并拼接成新字符串
    new_str = str1[-1] + str2[-1] + str3[-1]
    print(new_str)


if __name__ == "__main__":
    main()
```

> - `new_str = str1[-1] + str2[-1] + str3[-1]`：使用负数索引 `-1` 来提取每个字符串的最后一个字符，然后将它们依次拼接成一个新的字符串 `new_str`。

### 第四题：Upper and Lower

题目描述：输入一串字符串`str1`，要求编写程序，将其中所有的大写字母和小写字母进行转换，输出转换后字符串。

输入格式：一串非空字符串`str1`，只包含大小写字母元素。

输出格式：一串转换后的字符串。

输入样例

```bash
abcdEFG
```

输出样例

```bash
ABCDefg
```

提示说明：

- 注意大写字母指代`A`~`Z`，小写字母指代`a`~`z`。
- 由于难度问题，我们这里给出程序模板，请在**注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头。**

模板：

```python
str1 = input()
# 现在程序中有一个字符串，str1
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
# 方法一
str1 = input()
result = ''.join([char.upper() if char.islower() else char.lower() for char in str1])
print(result)
```

> - `result = ''.join([char.upper() if char.islower() else char.lower() for char in str1])`：这行代码运用了 Python 的列表推导式和字符串的 `join()` 方法，下面进行详细分析。
>
> - **列表推导式部分**：`[char.upper() if char.islower() else char.lower() for char in str1]`
>   - `for char in str1`：这是列表推导式的迭代部分，它会遍历字符串 `str1` 中的每一个字符 `char`。
>   - `char.upper() if char.islower() else char.lower()`：这是一个条件表达式。对于每一个字符 `char` 会先调用 `char.islower()` 方法判断该字符是否为小写字母。如果是小写字母，就调用 `char.upper()` 方法将其转换为大写字母；如果不是小写字母（即为大写字母），则调用 `char.lower()` 方法将其转换为小写字母。最终会生成一个包含转换后字符的列表。
>
> - **`join()` 方法部分**：`''.join(...)`
>
>   - `join()` 是字符串对象的一个方法，用于将可迭代对象（这里是列表）中的元素连接成一个字符串。
>   - `''` 表示连接时使用的分隔符为空字符串，即直接将列表中的元素依次拼接起来。
>   - 最终将转换后的字符列表拼接成一个完整的字符串，并赋值给变量 `result`。



```python
# 方法二
def main():
    str1 = input()
    result = ""
    for char in str1:
        if 'a' <= char <= 'z':
            # 将小写字母转换为大写字母
            result += chr(ord(char) - 32)
        elif 'A' <= char <= 'Z':
            # 将大写字母转换为小写字母
            result += chr(ord(char) + 32)
        else:
            result += char
    print(result)


if __name__ == "__main__":
    main()
```

> - 初始化一个空字符串 `result`，用于存储转换后的字符串。
> - 使用 `for` 循环遍历输入字符串 `str1` 中的每个字符 `char`。
> - 对于每个字符，进行如下判断：
>   - 如果 `char` 是小写字母（即 `'a' <= char <= 'z'`），利用 `ord()` 函数获取该字符的 ASCII 码值，减去 32（因为大写字母和小写字母的 ASCII 码值相差 32），再通过 `chr()` 函数将得到的新 ASCII 码值转换为对应的字符，添加到 `result` 中。
>   - 如果 `char` 是大写字母（即 `'A' <= char <= 'Z'`），将其 ASCII 码值加上 32，转换为对应的小写字母后添加到 `result` 中。
>   - 如果 `char` 既不是大写字母也不是小写字母（虽然题目说明只包含大小写字母，但加上此逻辑更具通用性），则直接将该字符添加到 `result` 中。

### 第五题：Rotate sentence please

题目描述：输入一串句子(不含标点符号)，单词与单词之间以空格隔开，要求编写程序，将单词顺序进行翻转，但不翻转每个单词的内容。

输入格式：一串非空字符串，包含数个单词，中间以空格隔开，不含标点符号。

输出格式：翻转后的句子。

输入样例

```bash
This works but is confusing
```

输出样例

```bash
confusing is but works This
```

提示说明

- 提示：本题可以复用翻转字符串部分代码。
- 由于难度问题，我们这里给出程序模板，请在**注释说明的部分补全你的代码，如果需要导入模块，请放在代码开头。**

模板：

```python
str1 = input()
# 现在程序中有一个变量，名为str1
# 在这行注释下面，编写代码，输出你的答案
```

**本题代码如下：**

```python
str1 = input()

words = str1.split()

reversed_words = list(reversed(words))
# 也可以这样表达 reversed_words = words[::-1]

result = ' '.join(reversed_words)
print(result)
```



### 参考资料

- [Github - 聪明办法学 python（第二版）- 字符串 Strings](https://github.com/datawhalechina/learn-python-the-smart-way-v2/blob/main/slides/chapter_6-Strings.ipynb)
- [聪明办法学 python（第二版）视频资料 - Chap6 字符串](https://www.bilibili.com/video/BV1pH4y1S71T?spm_id_from=333.788.videopod.sections)

