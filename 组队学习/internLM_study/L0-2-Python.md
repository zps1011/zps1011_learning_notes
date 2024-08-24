<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>入门岛 - Python 基础</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月20日&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后更新时间：2024年8月24日  
</div>


## 前言

### 1. Python基础概念

Python是一种广受欢迎的高级编程语言，以其简洁、优雅、易读的语法和强大的功能而闻名。由荷兰人Guido van Rossum（吉多·范罗苏姆）于1989年发明，并于1991年发布了第一个版本。Python 的设计哲学强调“优美胜于丑陋，简单胜于复杂，可读性很重要”。这使得 Python 成为一种易于学习、易于阅读和维护的编程语言，且支持多种编程范式，包括面向对象、命令式、函数式和过程式编程。

### 2. Python发展历程

（1）Python 1.x（1991年至2000年）

- 1991年：发布了第一个版本（0.9.0），奠定了Python语言的基础。
- 1994年：Python 1.0发布，引入了lambda、map、filter等函数式编程特性，以及对模块和包的支持。
- 这一阶段主要实现了基本的语法结构、数据类型、异常处理、模块系统等特性。

（2）Python 2.x（2000年至2020年）

- 2000年：Python 2.0发布，引入了列表推导式、垃圾回收机制、Unicode支持等新特性。

- 随后几年，Python 2.x系列不断引入新的特性和改进，如装饰器、生成器表达式、with语句等。

- Python 2.x系列在Web开发、科学计算、数据分析、人工智能等领域得到了广泛应用。

- 2010年：Python 2.7发布，此为Python 2.x系列的最后一个版本，于2020年结束支持。

（3）Python 3.x（2008年至今）

  - 2008年：Python 3.0发布，标志着Python进入了一个新的时代。Python 3.x系列对Python 2.x进行了重大改进和重构，引入了许多新特性和语法改进，改进包括统一文本和二进制数据模型、增加类型注解、异步编程支持等。
  - 截至2024年，Python 3.x系列已经发布了多个版本，如Python 3.12、Python 3.13等，不断引入新的特性和优化。

### 3. Python应用前景

Python在Web应用后端开发、云基础设施建设、DevOps、网络数据采集（爬虫）、自动化测试、数据分析、机器学习等领域都有着广泛的应用。Python以其简洁易学的语法、丰富的生态库支持、高效的开发效率以及强大的社区基础，在多个领域展现出广泛应用前景。与大型语言模型（LLM）的结合，更是推动了 Python 在智能问答、文本生成、数据分析及自动化运维等领域的深度应用，展现了其作为人工智能时代重要编程语言的独特魅力。

## 闯关任务及过程

### 1. Python 实现 wordcount

> - 任务描述：实现一个 wordcount 函数，统计英文字符串中每个单词出现的次数。返回一个字典，key 为单词，value 为对应单词出现的次数。
> - 示例输入：
>
> ```python
> """Hello world!  
> This is an example.  
> Word count is fun.  
> Is it fun to count words?  
> Yes, it is fun!"""
> ```
>
> - 示例输出：
>
> ```python
> {'hello': 1,'world!': 1,'this': 1,'is': 3,'an': 1,'example': 1,'word': 1, 
> 'count': 2,'fun': 1,'Is': 1,'it': 2,'to': 1,'words': 1,'Yes': 1,'fun': 1  }
> ```
>
> - 实现思路：
>
>   1.先去掉标点符号。
>
>   2.把每个单词转换成小写。
>
>   3.不需要考虑特别多的标点符号，只需要考虑实例输入中存在的就可以。

闯关提供文本：

```python
text = """
Got this panda plush toy for my daughter's birthday,
who loves it and takes it everywhere. It's soft and
super cute, and its face has a friendly look. It's
a bit small for what I paid though. I think there
might be other options that are bigger for the
same price. It arrived a day earlier than expected,
so I got to play with it myself before I gave it
to her.
"""

def wordcount(text):
    pass
```

通过闯关要求的实现效果，程序主要按顺序实现以下几点：

1. 替换字符串中的标点符号
2. 将字符串中的每个字符转换成小写
3. 根据空格或空白字符分隔字符串
4. 统计每个单词出现频次

对于字符串中的标点符号，可使用循环语句，遍历字符串中的每一个字符，判断当前字符是否为标点符号，若是则将当前字符替换为空格。

 `.isalnum()` 方法：判断字符是否在`a-z`、`A-Z`或`0-9`中，如果在，则返回 True。

 `.isspace()` 方法：判断字符是否为空白字符（如空格、制表符、换行符等），如果是，则返回 True。空白字符通常用于分隔文本中的单词或行。


实现代码如下：

```python
for char in text:
    if char.isalnum() or char.isspace():
        pass
    else:
        text = text.replace(char, ' ')
```

对于字符串中的字符，可使用字符串的 `.lower()` 方法将每个字符都转换成小写，代码如下：

```python
text = text.lower()
```

对于字符串的分隔，可使用字符串的 `split()` 方法。`split()` 方法默认按照空格或空白字符分隔字符串，所以无需提供额外参数，代码如下：

```python
words = text.split()
```

最后进行词频统计。对于统计单词频次，可使用字典存储单词和频次逐个遍历分隔后的单词列表，若当前字典中含该单词，则对应的计数值再加1；若不含，则将该单词添入到字典中，并设置其值为1，从而实现每个单词出现次数的统计，代码如下：

```python
word_counts = {}
    for word in words:  
        if word not in word_counts:  
            word_counts[word] = 1  
        else:  
            word_counts[word] += 1  
```

最后，将记录了统计结果的字典返回，并输出，就可以得到所求结果。整体代码如下：

```python
def wordcount(text):  
    # 1.替换字符串中的标点符号
    for char in text:
    	if char.isalnum() or char.isspace():
        	pass
    	else:
        	text = text.replace(char, ' ')

    # 2.将字符串中的每个字符转换成小写
    text = text.lower()  

    # 3.根据空格或空白字符分隔字符串 
    words = text.split()
    
	# 4.统计每个单词出现频次
    word_counts = {}  
    
    for word in words:  
        if word not in word_counts:  
            word_counts[word] = 1  
        else:  
            word_counts[word] += 1  
 
    return word_counts  

text = """  
Got this panda plush toy for my daughter's birthday,  
who loves it and takes it everywhere. It's soft and  
super cute, and its face has a friendly look. It's  
a bit small for what I paid though. I think there  
might be other options that are bigger for the  
same price. It arrived a day earlier than expected,  
so I got to play with it myself before I gave it  
to her.  
"""  
  
print(wordcount(text))
```

运行以下命令：

```python
python wordcount.py
```

得到输出结果：

```python
{'got': 2, 'this': 1, 'panda': 1, 'plush': 1, 'toy': 1, 'for': 3, 'my': 1, 'daughter': 1, 's': 3, 'birthday': 1, 'who': 1, 'loves': 1, 'it': 7, 'and': 3, 'takes': 1, 'everywhere': 1, 'soft': 1, 'super': 1, 'cute': 1, 'its': 1, 'face': 1, 'has': 1, 'a': 3, 'friendly': 1, 'look': 1, 'bit': 1, 'small': 1, 'what': 1, 'i': 4, 'paid': 1, 'though': 1, 'think': 1, 'there': 1, 'might': 1, 'be': 1, 'other': 1, 'options': 1, 'that': 1, 'are': 1, 'bigger': 1, 'the': 1, 'same': 1, 'price': 1, 'arrived': 1, 'day': 1, 'earlier': 1, 'than': 1, 'expected': 1, 'so': 1, 'to': 2, 'play': 1, 'with': 1, 'myself': 1, 'before': 1, 'gave': 1, 'her': 1}
```



### 2. Vscode 连接 InternStudio debug 过程记录

> - 任务描述：使用本地VSCode连接远程开发机，将上面你写的wordcount函数在开发机上进行debug，体验debug的全流程，并完成一份debug笔记(需要截图)。
> - 任务步骤：
> 
>   1.在本地VSCode中使用 Remote-SSH 插件，连接远程开发机。
>   
>   2.在远程开发机中debug（调试）带有 wordcount 函数的 python 程序。

- 什么是 debug？

  - debug（调试）是软件开发过程中的一个重要环节，它指的是在程序开发或维护过程中，查找并修正程序中的错误（即“bug”）的过程。这些错误可能导致程序无法正确运行、崩溃、产生错误的结果或者行为不符合预期。

#### 2.1 使用 pdb 进行debug

打开本地电脑的 VSCode，利用 Remote - SSH 插件连接远程开发机（详细步骤可参考[书生大模型实战营【第三期】学习笔记 - 入门岛 - Linux 基础](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/L0-1-Linux.md)），在远程开发机中创建一个新的 python 文件，将[1.Python实现wordcount](#1-Python-实现-wordcount)中实现的内容复制到该文件中，并在终端中输入以下命令，调试该文件：

```bash
python -m pdb wordcount.py
```

[`pdb` 是一个内置的Python调试器](https://docs.python.org/zh-cn/3/library/pdb.html)，它允许你逐行执行代码，查看和修改当前执行环境的状态（如变量的值），以及设置断点等。

在调试过程中，可以使用 `l` (list)命令查看当前代码的位置，使用 `n` (next)命令执行下一行代码，使用 `c` (continue)命令继续执行代码，使用 `p` (print)命令打印变量的值，使用 `b` (break)命令设置断点，使用 `s` (step)命令逐步执行代码等，以便更好地调试代码。调试完成后，输入 `q` (quit)命令退出调试，或输入 `Ctrl + D` 终止调试。

每一环节的相关变量的调试结果如下所示：

1. 替换字符串中的标点符号

   ![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-01.png)

2. 将字符串中的每个字符转换成小写

   ![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-02.png)

3. 根据空格或空白字符分隔字符串

   ![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-03.png)

4. 统计每个单词出现频次

   ![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-04.png)

#### 2.2 使用断点进行 debug

在远程开发机中新建一个`Python_debug.py`文件，写入以下代码：

```python
def add_numbers(a,b,c):
    sum = 0
    sum +=a
    sum +=b
    sum +=c
    print("The sum is ",sum) 

if __name__ =='__main__':
    x,y,z = 1,2,3
    result = add_numbers(x,y,z)
    print("The result of sum is ",result)
```

进行 debug 前，检查是否安装 Python Extension 插件。如果没有正确安装此插件，断点便不会出现。

在代码行号旁边点击，可以添加一个红点，这就是断点。当代码运行到相应的断点时，它会停下来，在断点前的代码会全部执行，这样就可以检查变量的值、执行步骤等。点击 VSCode 侧边栏小三角形的`Run and Debug（运行和调试）`，然后点击`Run and Debug（开始调试）`按钮，或按 `F5` 键，开始 debug 。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-05.png)

此次 debug 总共设置了`5个`断点，每一断点的相关变量的调试结果如下所示：

第 1 个断点，此时执行到 sum = 0 ，还没有执行到 sum +=a。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-06.png) 

第 2 个断点，此时已经执行了 sum +=a， sum 值变为 1。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-07.png)

第 3 个断点

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-08.png)

第 4 个断点

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-09.png)

第 5 个断点

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-10.png)

下图为 debug 工具栏，从 1-6 依次为：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-11.png)

- Continue：沿着断点继续执行
- Step Over：继续维持当前调用栈向下执行一行Python代码并中断
- Step Into：进入被调用的函数并中断执行
- Step Out：继续执行直到当前调用，待其终止后中断（函数或方法）
- Restart：重新启动当前程序调试
- Stop：终止当前程序调试

#### 2.3 [Python Tutor](https://pythontutor.com/python-compiler.html#mode=edit)

我们还可以使用 Python Tutor 将代码可视化，此工具可以帮助我们的理解代码的执行过程，帮助我们 debug 。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-2-Python-12.png)

## 总结

本次实战主要通过 python 实现了一个 wordcount 函数，用于统计英文字符串中每个单词出现的次数。通过 VSCode 连接远程开发机，体验并尝试用多种方法进行 debug 的过程，回顾了 pdb 调试过程。

## 参考资料

- [书生大模型实战营【第3期】学员闯关手册](https://aicarrier.feishu.cn/wiki/XBO6wpQcSibO1okrChhcBkQjnsf)
- [书生大模型实战营【第3期】 Python 基础知识任务](https://github.com/InternLM/Tutorial/blob/camp3/docs/L0/Python/task.md)
- [书生大模型实战营【第3期】 Python 基础知识文档](https://github.com/InternLM/Tutorial/tree/camp3/docs/L0/Python)
- [书生大模型实战营【第3期】 Python 基础知识视频](https://www.bilibili.com/video/BV1mS421X7h4)
- [书生大模型实战营【第三期】学习笔记 - 入门岛 - Linux 基础](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/L0-1-Linux.md)
- [Python 官方文档](https://docs.python.org/zh-cn/3/)
- [Python Tutor 工具](https://pythontutor.com/python-compiler.html#mode=edit)
