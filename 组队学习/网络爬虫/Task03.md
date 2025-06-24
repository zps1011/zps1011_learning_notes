<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>网络爬虫技术入门<br/><span>Task 03 爬虫技术进阶 </span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年6月24日
</div



### 一、Headers 的作用

- **定义**：Headers 是 HTTP 协议中的请求头，通常用于存放与请求内容无关的数据。
- **常见用途**：
  - 存放安全验证信息，如 User-Agent、Token、Cookie 等。
- **在 Requests中 的使用**：
  - 可以将请求头信息存储在`headers`参数中。
  - Requests 会自动将这些信息拼接成完整的 HTTP 请求头。

### 二、处理 Cookie 与模拟登录

#### 1、目标

通过模拟登录[17K小说网](https://www.17k.com/)，抓取个人书架上的小说信息。

#### 2、实现步骤

（1）**登录并获取Cookie**

- 使用`requests`库的`session`对象来模拟登录操作。
- 在登录过程中，服务器会返回 Cookie，这些 Cookie 会被`session`自动保存。
- Cookie是后续请求中验证用户身份的关键。

（2）**携带Cookie请求书架数据**

- 使用同一个`session`对象请求书架页面的 URL。
- 由于`session`会自动携带之前获取的 Cookie，因此无需手动设置 Cookie。

#### 3、代码实现

```python
import requests

# 创建session对象
session = requests.session()

# 准备登录所需的数据
data = {
    "loginName": "***********",  # 替换为实际账号
    "password": "***********"    # 替换为实际密码
}

# 设置请求头，模拟浏览器行为
headers = {
    "user-agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36"
}

# 登录请求
login_url = "https://passport.17k.com/ck/user/login"
resp = session.post(login_url, data=data, headers=headers)

# 打印登录响应内容和获取的Cookie
print(resp.text)
print(resp.cookies)

# 请求书架数据
bookshelf_url = "输入书架的 URL"
resp = session.get(bookshelf_url, headers=headers)

# 打印书架数据
resp_json = resp.json()
print(resp_json)
print(resp_json['data']['recentFavorites']['lists'])
```

#### 4、手动设置 Cookie 的方式

如果不想使用`session`，也可以手动设置 Cookie 来请求书架数据。这种方式需要提前获取并手动填写 Cookie。

```python
import requests

# 手动设置Cookie
cookie = {
    "输入你获得的 Cookie "
}

# 请求书架数据
bookshelf_url = "输入书架的 URL"
resp = requests.get(bookshelf_url, headers=headers, cookies=cookie)

# 打印书架数据
print(resp.text)
```

#### 5、总结

本小节了解了使用 session 的优势：自动管理 Cookie，简化代码逻辑。在某些情况下，可能需要手动调整 Cookie值，或者在无法使用 session 时作为替代方案。Cookie 的有效性可能因`时间`或`用户行为`而变化，需要确保 Cookie 是最新的。

### 三、梨视频数据抓取与防盗链处理

#### 1、步骤概述

抓取梨视频数据需要经过以下关键步骤：

（1）通过开发者工具定位视频资源的`实际下载地址`。

（2）检查视频资源是否在网页源代码中，以确认视频资源的位置。

（3）根据资源位置和网站机制，确定合适的抓取方法。

#### 2、代码实现

```python
import requests

# 1. 获取视频的contId
url = "https://www.pearvideo.com/video_1721605"
contId = url.split("_")[1]  # 从URL中提取视频的唯一标识符contId
# print(contId)

# 2. 构造获取视频状态的URL
videoStatusUrl = f"https://www.pearvideo.com/videoStatus.jsp?contId={contId}&mrd=0.8770894467476524"

# 3. 设置请求头，包含Referer以应对防盗链
headers = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.192 Safari/537.36",
    "Referer": url  # 指定Referer为视频页面URL，以通过防盗链验证
}

# 4. 发送请求，获取视频信息
resp = requests.get(videoStatusUrl, headers=headers)
dic = resp.json()

# 5. 提取视频下载地址并进行处理
srcUrl = dic['videoInfo']['videos']['srcUrl']
systemTime = dic['systemTime']
srcUrl = srcUrl.replace(systemTime, f"cont-{contId}")  # 替换URL中的动态部分，拼接出真正的视频下载地址
# print(srcUrl)

# 6. 下载视频
with open("a.mp4", mode="wb") as f:
    video_content = requests.get(srcUrl).content
    f.write(video_content)
```

#### 3、关键点解析

1. **提取`contId`**
   - 视频页面的URL中通常包含一个`contId`，这是视频的唯一标识符，用于构造获取视频状态的请求。
2. **Referer 防盗链**
   - 网站通过 Referer 字段验证请求来源是否合法。在请求视频状态时，需要将 Referer 设置为视频页面的 URL。
3. **处理动态 URL**
   - 视频的实际下载地址可能包含动态生成的时间戳或其他标识符。通过替换这些动态部分为`contId`，可以构造出正确的下载地址。
4. **下载视频**
   - 使用`requests.get`获取视频内容，并以二进制模式写入本地文件。

#### 4、总结

从上述代码，我们可以学习到，通过设置Referer字段，可以绕过简单的防盗链验证。以及我们需要分析返回的JSON数据，找到正确的替换规则，以构造出有效的视频动态下载地址。

### 四、使用代理服务器

#### 1、需求背景

在进行网络爬虫开发时，频繁地向目标网站发送请求可能会导致服务器将你的 IP 地址封锁，以防止爬虫行为。为了避免这种情况，可以使用代理服务器来伪装请求来源。

#### 2、代理服务器的原理

代理服务器的作用是作为请求的中转站。当你通过代理服务器发送请求时，目标网站看到的请求来源是代理服务器的IP地址，而不是你的真实 IP 地址。这样可以有效避免你的 IP 被封锁。

#### 3、如何在爬虫中使用代理

```python
import requests

# 设置请求头，模拟浏览器行为
headers = {
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36"
}

# 配置代理
proxies = {
    # "http": "http://47.101.44.122:80"
    "https": "https://114.86.178.108:9000"
}

# 发送请求，通过代理访问目标网站
response = requests.get("https://www.baidu.com", headers=headers, proxies=proxies)
response.encoding = 'utf-8'

# 输出响应内容
print(response.text)
```

> 使用代理 IP 时需注意其合法性。某些代理 I P可能属于灰色产业，使用时需谨慎。
>
> 代理 IP 的质量参差不齐，有些可能不稳定或不可用。我们需要从从可靠的代理服务提供商获取代理 IP 。



#### 4、常用代理服务提供商

- **云代理**：http://www.ip3366.net/
  - 特点：提供大量免费代理IP，支持动态HTTP代理，代理IP经过严格筛选和验证，匿名度高，传输速度快。

#### 5、总结

使用代理服务器是应对 IP 封锁的有效手段。通过代理服务器，可以隐藏真实 IP 地址，避免被目标网站检测到爬虫行为。在使用代理时，需要注意代理 IP 的稳定性和合法性，以确保爬虫的顺利及合法。



### 参考资料

- [Datawhale-零基础网络爬虫技术-第 03 讲_爬虫技术进阶](https://www.datawhale.cn/learn/content/178)