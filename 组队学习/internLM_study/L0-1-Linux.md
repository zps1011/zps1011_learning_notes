<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】<br/><span>入门岛 - Linux 基础知识</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月19日
</div>


## 前言

### 1. Linux基础概念

Linux全称GNU/Linux，是一种自由和开源的类Unix操作系统，是一个基于POSIX和UNIX的多用户、多任务、支持多线程和多CPU的操作系统。Linux的内核是由芬兰人林纳斯·托瓦兹于1991年10月5日首次发布。Linux是自由软件和开放源代码软件发展中最著名的例子。只要遵循GNU通用公共许可证，任何个人和机构都可以免费使用Linux的所有底层源代码，根据自身需求自由地修改和再发布。

### 2. Linux开发

Linux开发主要指的是在Linux操作系统下进行的软件开发的过程，其中包括编写、调试、测试等环节。Linux作为一个开源的操作系统，具有高度的可定制性和灵活性。Linux开发的过程中需要使用一些开发工具，如编译器、调试器、版本控制工具等。Linux开发的过程中还需要遵循一些开发规范，如代码规范、文档规范等。

### 3. [InternStudio]([‌‍﻿‬‌‍‬‌﻿‍⁠‍﻿‬‌﻿‌﻿‌‌‌﻿‌‌‬InternStudio - 飞书云文档 (feishu.cn)](https://aicarrier.feishu.cn/wiki/GQ1Qwxb3UiQuewk8BVLcuyiEnHe))开发机介绍

InternStudio 是大模型时代下的云端算力平台。基于 InternLM 组织下的诸多算法库支持，为开发者提供开箱即用的大语言模型微调环境、工具、数据集，并完美兼容 HugginFace 开源生态。

控制台直通链接：https://studio.intern-ai.org.cn/



## 闯关任务及过程

### 1. 完成SSH连接、端口映射、运行hello_world.py

> - 任务描述：使用SSH连接到Linux服务器，完成端口映射，运行hello_world.py文件。
> - 任务步骤：
>
>   1. 创建远程开发机并在开发机中新建一个 Python 文件 hello_world.py
>   2. 配置SSH Key
>   3. 使用SSH连接到Linux服务器
>   4. 运行hello_world.py文件并完成端口映射

#### （1）创建远程开发机并创建一个 Python 文件 hello_world.py

在进行本地与远程的连接、执行代码程序前，应当先在 InternStudio 上创建一个远程开发机。

点击进入并注册登录 [InterStudio](https://studio.intern-ai.org.cn/)，在左侧 `开发机` 栏目中点击 `创建开发机`，选择并填写开发机的相关信息。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-01.png)

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-02.png)

选取资源时，应当按照实际需求选择以节省开发资源。本次闯关创建的开发机信息如下：

- 开发机名称：`zps1011_test`
- 镜像：`Cuda12.2-conda`
- 资源配置：`10% A100 * 1`
- 运行时长：`2小时0分钟`

选择完成后，点击立即创建，便会创建了一个开发机。静待一段时间后，开发机完成初始化，操作栏中的选项按键变为可点击状态，则表示开发机创建成功。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-03.png)

点击 `进入开发机`，即可进入 Web IDE，对开发机进行操作。开发机左上方为我们准备了三种不同的开发环境，从1到3依次为 `JupyterLab`、`Terminal`、`VSCode`。根据实际需求选择不同的开发环境。 此处我选择使用`VSCode`进行开发。

<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-04.png" alt="image" style="zoom:75%;" />

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-05.png)

在VSCode中，在左侧的资源管理器中定位到 root（根目录）下，在根目录下单击右键选择 `New File`新建文件，并命名为 `hello_world.py` ，创建成功后即可编写代码。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-06.png)

```python
# pip install gradio==4.29.0
import socket
import re
import gradio as gr
 
# 获取主机名
def get_hostname():
    hostname = socket.gethostname()
    match = re.search(r'-(\d+)$', hostname)
    name = match.group(1)
    
    return name
 
# 创建 Gradio 界面
with gr.Blocks(gr.themes.Soft()) as demo:
    html_code = f"""
            <p align="center">
            <a href="https://intern-ai.org.cn/home">
                <img src="https://intern-ai.org.cn/assets/headerLogo-4ea34f23.svg" alt="Logo" width="20%" style="border-radius: 5px;">
            </a>
            </p>
            <h1 style="text-align: center;">☁️ Welcome {get_hostname()} user, welcome to the ShuSheng LLM Practical Camp Course!</h1>
            <h2 style="text-align: center;">😀 Let’s go on a journey through ShuSheng Island together.</h2>
            <p align="center">
                <a href="https://github.com/InternLM/Tutorial/blob/camp3">
                    <img src="https://oss.lingkongstudy.com.cn/blog/202406301604074.jpg" alt="Logo" width="20%" style="border-radius: 5px;">
                </a>
            </p>

            """
    gr.Markdown(html_code)

demo.launch()
```

 `gradio` 是一个用于构建交互式机器学习界面的 python 库。代码的最后，使用了 `demo.launch()` 方法启动 Gradio 界面，我们会看到创建的界面的显示效果。

#### （2）配置SSH Key

通常情况下，进入开发机需要使用`输入密码`的方式进行SSH远程连接，但是如果每一步骤都需要密码的方式会非常麻烦。此处会讲解如何配置免密登录。

在进行 SSH 连接之前，需要配置本地 SSH Key。打开本地个人电脑的终端，输入以下命令，生成 SSH Key。

```bash
ssh-keygen -t rsa
```

在生成 SSH Key 的过程中，可以选择是否设置密码，如果设置密码，那么在连接远程服务器时，需要输入密码。如果不需要生成密码，则按回车键。

生成 SSH Key 后，我们可以在 `~/.ssh` 目录下找到生成的 SSH Key，其中 `id_rsa` 是私钥，`id_rsa.pub` 是公钥。

Linux 系统 可使用 `cat ~/.ssh/id_rsa.pub` 命令查看公钥。

Windows 就是`C:\Users\{your_username}\`。在 powerShell 中可以使用`Get-Content`命令查看生成的公钥。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-07.png)

复制该公钥内容。我们需要将公钥添加至 InternStudio 的 SSH Key 中。在 InternStudio 的首页中选择 `配置 SSH Key`，点击 `添加 SSH 公钥`，将刚刚复制的公钥粘贴至 `公钥` 一栏中，稍等一小会儿，名称会被自动识别。点击 `立即添加`，即可添加成功。添加成功后可以在 `SSH 公钥管理` 中看到我们刚刚添加的 SSH Key。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-08.png)

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-09.png)

<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-10.png" alt="image" style="zoom:80%;" />

#### （3）使用SSH连接到Linux服务器

在本地电脑的终端中，输入以下命令，使用 SSH 连接到 Linux 服务器。

```bash
ssh -p {port} root@ssh.intern-ai.org.cn -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null
```

其中，`{port}` 是我们在 InternStudio 中创建的开发机的端口号，该端口号可在平台的开发机页面中的 `SSH 连接` 窗口中看到。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-11.png)

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-12.png)

待输出完成后，就可以免密并成功连接到 Linux 服务器了。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-13.png)

#### （4）运行hello_world.py文件并完成端口映射

在连接到 Linux 服务器后，`ls`在根目录下的 `hello_world.py` 文件。使用以下命令完成相关配置并运行该文件。

```bash
pip install gradio && python hello_world.py
```

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-14.png)

接着，我们需要完成端口映射。打开本地电脑另一个终端，在终端中输入以下命令，使用 SSH 连接到 Linux 服务器。

```bash
ssh -p {port} root@ssh.intern-ai.org.cn -CNg -L 7860:127.0.0.1:7860 -o StrictHostKeyChecking=no
```

其中，`{port}` 是我们在 InternStudio 中创建的开发机的端口号。本次任务需要将 Linux 服务器的 `7860` 端口映射到本地电脑的 `7860` 端口。运行命令后，本地控制台将不产生任何输出，这时我们可以在本地浏览器中输入 `http://127.0.0.1:7860`，即可看到 Gradio 界面。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-15.png)

### 2. 使用VSCode远程连接开发机，进行conda环境配置

> - 任务描述：使用VSCode远程开发功能连接到Linux服务器，配置conda环境。
> - 任务步骤：
>   1. 安装VSCode插件Remote - SSH
>   2. 配置SSH连接
>   3. 创建、配置、运行conda环境

VSCode 是一款由微软开发的开源代码编辑器，支持 Windows、macOS 和 Linux 等操作系统。VSCode 具有丰富的插件生态系统，可以满足不同用户的需求。VSCode 还支持远程开发功能，可以通过 SSH 连接到远程服务器进行开发。在本次任务中，将使用 VSCode 远程开发功能连接到 Linux 服务器，并配置 conda 环境。

#### （1）安装VSCode插件Remote - SSH

在本地电脑上打开 VSCode，点击左侧的扩展按钮，搜索 `Remote - SSH` 插件并安装。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-16.png)

#### （2）配置SSH连接

点击左侧的 `Remote Explorer（远程资源管理器）`，点击 `+` 按钮，选择 `Add New SSH Host（新建远程）`，在页面上方弹出的输入框中输入以下命令：

```bash
ssh -p {port} root@ssh.intern-ai.org.cn -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null
```

其中，`{port}` 是我们在 InternStudio 中创建的开发机的端口号。输入完成后按下键盘上的 `Enter` 键，并选择要更新的配置文件（以默认的第一个配置文件为例），随后便添加成功。可在左侧的 `Remote Explorer（远程资源管理器）` 中相应远程连接。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-17.png)

选中需要连接远程服务器，点击 `Connect to Host（在当前窗口中连接）`，VSCode 将会自动连接到远程服务器。初次连接时，会在开发机中下载远程连接所需的缓存应用文件。连接成功后，我们可以在左下角的状态栏中看到当前连接的主机名，也能在左侧的资源管理器中看到开发机的文件目录。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-18.png)

#### （3）配置conda环境

下一步，我们需要配置 conda 环境。在当前连接了远程开发机的 VSCode 中，点击顶部导航栏 `终端（Terminal）`，选择 `新建终端（New Terminal）`，在终端中输入以下命令，创建一个新的、基于 python3.10 的，名为 zps1011_test 的 conda 环境。

```bash
conda create -n zps1011_test python=3.10
```

确认环境创建，静候一段时间，待环境创建完成。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-19.png)

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-20.png)

然后，我们需要输入以下命令，激活该环境。

```bash
conda activate zps1011_test
```

成功激活环境后，可以在终端中看到环境名前的 `(zps1011_test)`。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-21.png)

### 3. 创建并运行test.sh文件

> - 任务描述：在Linux服务器上创建一个名为test.sh的Shell脚本文件，编写内容后运行该文件。
> - 任务步骤：
>   1. 创建test.sh文件
>   2. 编写Shell脚本
>   3. 运行test.sh文件

`.sh`文件是脚本文件，一般是 bash 脚本，包含一系列的命令，可以在Linux系统中运行。

在本次任务中，我创建一个名为 `test.sh` 的 Shell 脚本文件，用于更换 conda 源。并在文件内编写以下内容。

```bash
#设置清华镜像
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
conda config --set show_channel_urls yes
conda config --show
```

上述代码中，使用 `conda config` 命令更换 conda 源为清华镜像。在文件编写完成后，点击顶部导航栏的 `File（文件）`，选择 `Save（保存）`，保存文件，也可以通过键盘快捷键 `Ctrl + S` 保存文件。保存文件后，我们需要在终端中运行该文件。在终端中输入以下命令，运行 `test.sh` 文件。

```bash
bash test.sh
```

静待程序运行完成，届时可以看到，在终端中输出了换源结果。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-1-Linux-22.png)

## 总结

本次闯关初步接触了 Linux 的基本概念以及如何使用开发机。在闯关实战途中，通过实践学习了如何使用 SSH 连接到 Linux 服务器，如何通过本地 VSCode 远程开发功能连接到 Linux 服务器，以及如何在 Linux 服务器上创建并运行 Shell 脚本文件。通过本次学习，我对 Linux 系统有了更深入的了解，也掌握了一些在 Linux 服务器上的基本操作技能。

## 参考资料

- [书生大模型实战营【第3期】学员闯关手册](https://aicarrier.feishu.cn/wiki/XBO6wpQcSibO1okrChhcBkQjnsf)
- [书生大模型实战营【第3期】 Linux 基础知识任务](https://github.com/InternLM/Tutorial/blob/camp3/docs/L0/Linux/task.md)
- [书生大模型实战营【第3期】 Linux 基础知识文档](https://github.com/InternLM/Tutorial/blob/camp3/docs/L0/Linux/readme.md)

- [书生大模型实战营【第3期】 Linux 基础知识视频](https://www.bilibili.com/video/BV1FS421d7yg/)
