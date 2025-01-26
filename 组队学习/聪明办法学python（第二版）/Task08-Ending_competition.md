<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>聪明办法学 python（第二版）<br/><span>Task 08：结课竞赛</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年1月27日
</div>
<div>
 <!--  
    Author: zps1011 （欢乐一天）  
    Date: 2025-1-26 & 27 
    -->  
&nbsp;&nbsp;&nbsp;
</div>


**作业系统个人主页链接：https://hydro.ac/d/datawhale_p2s/user/48019**

## 一、结课竞赛

### 第一题：成绩

**题目描述：**

小鲸鱼最近学习了 Datawhale 的聪明办法学 Python 线下课程，这门课程的总成绩计算方法是：

总成绩 = 作业成绩 ×20%+ 小测成绩 ×30%+ 期末考试成绩 ×50%

小鲸鱼想知道，这门课程自己最终能得到多少分。

**输入格式**：三个非负整数 A,B,C，分别表示小鲸鱼的作业成绩、小测成绩和期末考试成绩。相邻两个数之间用一个空格隔开，三项成绩满分都是 100 分。

**输出格式**：一个整数，即小鲸鱼这门课程的总成绩，满分也是 100 分。

输入数据 1

```bash
100 100 80
```

输出数据 1

```bash
90
```

输入数据 2

```bash
60 90 80
```

输出数据 2

```bash
79
```

**提示**：

- 输入输出样例 1 说明

   - 小鲸鱼的作业成绩是 100 分，小测成绩是 100 分，期末考试成绩是 80 分，总成绩是 100×20%+100×30%+80×50%=20+30+40=90。

- 输入输出样例 2 说明

   - 小鲸鱼的作业成绩是 60 分，小测成绩是 90 分，期末考试成绩是 80 分，总成绩是 60×20%+90×30%+80×50%=12+27+40=79。

**数据说明**：

对于 30% 的数据，A=B=0。

对于另外 30% 的数据，A=B=100。

对于 100% 的数据，0 ≤ A,B,C ≤ 100 且 A, B, C 都是 10 的整数倍。

**本题代码如下：**

```python
# 方法一
# 用户一次性输入三门成绩，成绩之间以空格分隔  
scores_input = input("")  
  
# 使用split方法将输入的字符串按空格分割成列表。split()以列表形式返回
scores = scores_input.split()  
  
# 将分割后的字符串列表转换成数字列表  
scores = [eval(score) for score in scores]  
  
A, B, C = scores  
# 根据权重计算总成绩  
total_scores = A * 0.2 + B * 0.3 + C * 0.5  
# 根据题目要求，输出为整数  
print(int(total_scores))
```

```python
# 方法二
# 读取输入的作业成绩、小测成绩和期末考试成绩
A, B, C = map(int, input().split())

# 按照总成绩计算公式计算总成绩
total_score = A * 0.2 + B * 0.3 + C * 0.5

# 输出总成绩，转换为整数类型
print(int(total_score))
```

### 第二题：小鲸鱼的游泳时间

**题目描述**：

亚运会开始了，小鲸鱼在拼命练习游泳准备参加游泳比赛，可怜的小鲸鱼并不知道鱼类是不能参加人类的奥运会的。

这一天，小鲸鱼给自己的游泳时间做了精确的计时（本题中的计时都按 24 小时制计算），它发现自己从 a 时 b 分一直游泳到当天的 c 时 d 分，请你帮小鲸鱼计算一下，它这天一共游了多少时间呢？

小鲸鱼游的好辛苦呀，你可不要算错了哦。

**输入格式**：一行内输入四个整数，以空格隔开，分别表示题目中的 a,b,c,d。

**输出格式**：一行内输出两个整数 e 和 f，用空格间隔，依次表示小鲸鱼这天一共游了多少小时多少分钟。其中表示分钟的整数 f 应该小于 60。

输入数据 1

```bash
12 50 19 10
```

输出数据 1

```bash
6 20
```

**提示**：对于全部测试数据，0 ≤ a, c ≤ 24，0 ≤ b, d ≤ 60，且结束时间一定晚于开始时间。

**本题代码如下：**

```python
# 方法一
start_hour, start_min, end_hour, end_min = map(int, input("").split())
# 转换为分钟进行下一步计算
start_time = start_hour * 60 + start_min
end_time = end_hour * 60 + end_min
# 总时间（分钟） = 结束时间 - 开始时间
total_time = end_time - start_time
# 计算小时数和分钟数
hours = total_time // 60
minutes = total_time % 60

print(hours, minutes)
```

### 第三题：不高兴的小鲸鱼

**题目描述**：

小鲸鱼上初中了。鲸鱼妈妈认为小鲸鱼应该更加用功学习，所以小鲸鱼除了上学之外，还要参加鲸鱼妈妈为它报名的各科复习班。另外每周鲸鱼妈妈还会送它去学习朗诵、舞蹈和钢琴。但是小鲸鱼如果一天上课超过八个小时就会不高兴，而且上得越久就会越不高兴。假设小鲸鱼不会因为其它事不高兴，并且它的不高兴不会持续到第二天。请你帮忙检查一下小鲸鱼下周的日程安排，看看下周它会不会不高兴；如果会的话，哪天最不高兴。

**输入格式**：

输入包括 7 行数据，分别表示周一到周日的日程安排。每行包括两个小于 10 的非负整数，用空格隔开，分别表示小鲸鱼在学校上课的时间和鲸鱼妈妈安排它上课的时间。

**输出格式：**

一个数字。如果不会不高兴则输出 0，如果会则输出最不高兴的是周几（用 1,2,3,4,5,6,7 分别表示周一，周二，周三，周四，周五，周六，周日）。如果有两天或两天以上不高兴的程度相当，则输出时间最靠前的一天。

输入数据 1

```bash
5 3
6 2
7 2
5 3
5 4
0 4
0 6
```

输出数据 1

```bash
3
```

**本题代码如下：**

```python
# 方法一

# 定义一个函数来计算一天的不高兴程度  
def calculate_unhappiness(school_time, mom_time):  
    # 计算总的学习时间  
    total_time = school_time + mom_time  
    # 如果总上课时间不超过8小时，则不高兴程度为0  
    if total_time <= 8:  
        return 0  # 不会不高兴  
    # 如果总上课时间超过8小时，则不高兴的程度等于超出的小时数  
    else:  
        return total_time - 8  
  
# 读取一周的日程安排  
# 初始化一个空列表用于保存一周的日程  
schedule = []  
# 循环 7 次，分别读取周一到周日的日程安排  
for _ in range(7):  
    # 读取一行输入，根据空格将数据分隔成两部分，分别转换为整数类型  
    school_time, mom_time = map(int, input("").split())  
    # 将转换后的时间添加到日程列表中，每个日程是一个包含两个整数的元组  
    schedule.append((school_time, mom_time))  
  
# 初始化最不高兴的程度和对应的日期  
# 初始时，没有不高兴的情况，所以不高兴程度定义为 0  
max_unhappiness = 0  
# 初始时，没有确定最不高兴的日期，所以定义为 0  
unhappiest_day = 0  
  
# 检查每一天的日程安排  
# 遍历日程列表，enumerate函数返回每个元素的索引（从 1 开始，对应周几）和值  
for day, (school_time, mom_time) in enumerate(schedule, start = 1):  
    # 计算当天的不高兴程度  
    unhappiness = calculate_unhappiness(school_time, mom_time)  
    # 如果当天的不高兴程度大于当前记录的最大不高兴程度  
    if unhappiness > max_unhappiness:  
        # 更新最大不高兴程度和对应的日期  
        max_unhappiness = unhappiness  
        unhappiest_day = day  
    
# 输出最不高兴的日期，如果没有任何一天会不高兴，则输出 0  
print(unhappiest_day)
```

```python
# 方法二

# 初始化最不高兴的天数和最大上课时长
most_unhappy_day = 0
max_hours = 0

# 循环处理周一到周日的日程安排
for day in range(1, 8):
    # 读取当天的学校上课时间和妈妈安排的上课时间
    school_hours, extra_hours = map(int, input().split())
    # 计算当天的总上课时长
    total_hours = school_hours + extra_hours

    # 如果当天上课时长超过 8 小时且大于之前记录的最大时长
    if total_hours > 8 and total_hours > max_hours:
        # 更新最不高兴的天数和最大上课时长
        most_unhappy_day = day
        max_hours = total_hours

print(most_unhappy_day)
```

### 第四题：小鲸鱼的 Lucky Word

**题目描述：**

小鲸鱼的词汇量很小，所以每次做英语选择题的时候都很头疼。但是它找到了一种方法，经试验证明，用这种方法去选择选项的时候选对的几率非常大！

这种方法的具体描述如下：假设 maxn 是单词中出现次数最多的字母的出现次数，minn 是单词中出现次数最少的字母的出现次数，如果 maxn−minn 是一个质数，那么小鲸鱼就认为这是个 Lucky Word，这样的单词很可能就是正确的答案。

**输入格式**：一个单词，其中只可能出现小写字母，并且长度小于 100。

**输出格式**：

共两行，第一行是一个字符串，假设输入的单词被验证是 `Lucky Word`，那么输出字符串 `Lucky Word`，否则输出 `No Answer`；

第二行是一个整数，如果输入单词被验证是 `Lucky Word`，输出 maxn−minn 的值，否则输出 0。

输入数据 1

```bash
error
```

输出数据 1

```bash
Lucky Word
2
```


输入数据 2

```bash
olympic
```

输出数据 2

```bash
No Answer
0
```

**提示**：

- 输入输出样例 1 解释

   - 单词 `error` 中出现最多的字母 r 出现了 3 次，出现次数最少的字母出现了 1 次，3−1=2，2 是质数。

- 输入输出样例 2 解释

   - 单词 `olympic` 中出现最多的字母 i 出现了 1 次，出现次数最少的字母出现了 1 次，1−1=0，0  不是质数。

**本题代码如下：**

```python
# 方法一

'''
maxn 是单词中出现次数最多的字母的出现次数
minn 是单词中出现次数最少的字母的出现次数
如果 maxn−minn 是一个质数，则是 Lucky Word，这样的单词很可能就是正确的答案。
'''
word = input() 

# 判断是否为质数
def is_prime(num):  
    if num < 2:  
        return False
    for i in range(2, int(num ** 0.5) + 1): 
        if num % i == 0:  
            return False
    return True  

# 判断是否为lucky_word
def is_lucky_word(word):  

    # 统计单词中每个字母的出现次数  
    # letter_counts = Counter(word)
    letter_counts = [word.count(letter) for letter in word]
      
    # 找出出现次数最多的字母的次数(maxn)  
    # maxn = max(letter_counts.values())
    maxn = max(letter_counts)
      
    # 找出出现次数最少的字母的次数(minn)
    # minn = min(filter(lambda x: x > 0, letter_counts.values()))
    minn = min(letter_counts)

    # 计算差值
    difference_value = maxn - minn 
      
    # 检查 maxn - minn 是否为质数  
    if is_prime(difference_value):  
        return "Lucky Word", difference_value  
    else:  
        return "No Answer", 0  
  

result, difference_value = is_lucky_word(word)

print(result)  
print(difference_value)
```

```python
# 方法二

# 判断一个数是否为质数的函数
def is_prime(num):
    if num < 2:
        return False
    for i in range(2, int(num**0.5) + 1):
        if num % i == 0:
            return False
    return True


# 读取输入的单词
word = input()

# 初始化一个字典来记录每个字母的出现次数
letter_count = {}
for letter in word:
    if letter in letter_count:
        letter_count[letter] += 1
    else:
        letter_count[letter] = 1

# 找出出现次数最多和最少的字母的出现次数
maxn = max(letter_count.values())
minn = min(letter_count.values())

# 计算差值
diff = maxn - minn

# 判断差值是否为质数
if is_prime(diff):
    print("Lucky Word")
    print(diff)
else:
    print("No Answer")
    print(0)
```

### 第五题：哥德巴赫猜想

**题目描述**

输入一个偶数 N，验证 4∼N 所有偶数是否符合哥德巴赫猜想：任一大于 2 的偶数都可写成两个质数之和。如果一个数不止一种分法，则输出第一个加数相比其他分法最小的方案。例如 10，10 = 3 + 7 = 5 + 5，则 10 = 5 + 5 是要被舍去的答案。

**输入格式**：第一行输入一个正偶数 N

**输出格式**

输出 （N−2）/ 2 行。对于第 i 行：

首先先输出正偶数 2i+2，然后输出等号，再输出加和为 2i+2 且第一个加数最小的两个质数，以加号隔开。

输入数据 1

```bash
10
```

输出数据 1

```bash
4=2+2
6=3+3
8=3+5
10=3+7
```

**提示**：数据保证，4 ≤ N ≤ 10000 。

**本题代码如下：**

```python
# 方法一

N = eval(input())
# 判断是否为质数
def is_prime(num):
    if num < 2:
        return False
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            return False
    return True

def Goldbach_Problem(N):
    results = []

    for i in range(4, N + 1, 2):
        for j in range(2, i // 2 + 1):
            if is_prime(j) and is_prime(i - j):
                results.append((i, j, i-j))
                break

    return results

results = Goldbach_Problem(N)

for number in results:
    # 元组解包
    even_num, prime1, prime2 = number
    # 格式化输出
    print(f"{even_num}={prime1}+{prime2}")
```



```python
# 方法二

# 判断一个数是否为质数的函数
def is_prime(num):
    if num < 2:
        return False
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            return False
    return True


# 读取输入的偶数 N
N = int(input())

# 遍历 4 到 N 之间的所有偶数
for even_num in range(4, N + 1, 2):
    # 寻找第一个加数最小的质数对
    for first_prime in range(2, even_num):
        second_prime = even_num - first_prime
        if is_prime(first_prime) and is_prime(second_prime):
            print(f"{even_num}={first_prime}+{second_prime}")
            break
```

> `if is_prime(first_prime) and is_prime(second_prime)`：判断 `first_prime` 和 `second_prime` 是否都为质数。如果是，则使用格式化字符串输出该偶数等于这两个质数相加的表达式，并使用 `break` 语句跳出内层循环，以保证得到的是第一个加数最小的方案。





## 二、调试 debugging

### 1、什么是 debug？

- debug（调试）是软件开发过程中的一个重要环节，它指的是在程序开发或维护过程中，查找并修正程序中的错误（即“bug”）的过程。这些错误可能导致程序无法正确运行、崩溃、产生错误的结果或者行为不符合预期。

### 2、使用 pdb 进行debug

打开本地电脑的 VSCode，创建一个新的 python 文件，命名为 `wordcount.py` ，将以下代码写入该文件中。

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

写入完成并且成功保存后，在终端中输入以下命令，调试该文件：

```bash
python -m pdb wordcount.py
```

[`pdb` 是一个内置的 Python 调试器](https://docs.python.org/zh-cn/3/library/pdb.html)，它允许你逐行执行代码，查看和修改当前执行环境的状态（如变量的值），以及设置断点等。

在调试过程中，可以：

- 使用 `l` (list)命令查看当前代码的位置；

- 使用 `n` (next)命令执行下一行代码；
- 使用 `c` (continue)命令继续执行代码；
- 使用 `p` (print)命令打印变量的值；
- 使用 `b` (break)命令设置断点；
- 使用 `s` (step)命令逐步执行代码等，以便更好地调试代码。
- 调试完成后，输入 `q` (quit)命令退出调试，或输入 `Ctrl + D` 终止调试。

每一环节的相关变量的调试结果如下所示：

1. 替换字符串中的标点符号

   ![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-01.png)

2. 将字符串中的每个字符转换成小写

   ![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-02.png)

3. 根据空格或空白字符分隔字符串

   ![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-03.png)

4. 统计每个单词出现频次

   ![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-04.png)

### 3、使用断点进行 debug

在 VScode 中新建一个`Python_debug.py`文件，写入以下代码：

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

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-05.png)

此次 debug 总共设置了`5个`断点，每一断点的相关变量的调试结果如下所示：

第 1 个断点，此时执行到 sum = 0 ，还没有执行到 sum +=a。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-06.png) 

第 2 个断点，此时已经执行了 sum +=a， sum 值变为 1。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-07.png)

第 3 个断点，此时已经执行了 sum +=b， sum 为 1 + 2 = 3。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-08.png)

第 4 个断点，此时已经执行了 sum +=c， sum 为 3 + 3 = 6。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-09.png)

第 5 个断点

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-10.png)

下图为 debug 工具栏，从 1-6 依次为：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-11.png)

- Continue：沿着断点继续执行
- Step Over：继续维持当前调用栈向下执行一行Python代码并中断
- Step Into：进入被调用的函数并中断执行
- Step Out：继续执行直到当前调用，待其终止后中断（函数或方法）
- Restart：重新启动当前程序调试
- Stop：终止当前程序调试

### 4、Python Tutor

我们还可以使用 Python Tutor 将代码可视化，此工具可以帮助我们的理解代码的执行过程，辅助我们 debug 。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task08-12.png)




### 参考资料

- [Python 官方文档](https://docs.python.org/zh-cn/3/)
- [pdb - Python 的调试器](https://docs.python.org/zh-cn/3/library/pdb.html)
- [VS Code 中的 Python 调试](https://vscode.js.cn/docs/python/debugging)
- [Python Tutor 工具](https://pythontutor.com/python-compiler.html#mode=edit)
- [菜鸟教程 - Python3 教程](https://www.runoob.com/python3/python3-tutorial.html)

