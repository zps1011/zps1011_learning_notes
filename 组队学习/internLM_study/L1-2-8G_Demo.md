<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>基础岛 - 8G 显存玩转书生大模型 Demo</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月22日
</div>


## 前言

### 1. LMDeploy

LMDeploy 是一个用于压缩、部署和服务 LLM 的工具包，由 MMRazor 和 MMDeploy 团队开发。它具有以下核心功能：

- **高效的推理**：LMDeploy 通过引入持久化批处理、块 KV 缓存、动态分割与融合、张量并行、高性能 CUDA 内核等关键技术，提供了比 vLLM 高 1.8 倍的推理性能。
- **有效的量化**：LMDeploy 支持仅权重量化和 k/v 量化，4bit 推理性能是 FP16 的 2.4 倍。量化后模型质量已通过 OpenCompass 评估确认。
- **轻松的分发**：利用请求分发服务，LMDeploy 可以在多台机器和设备上轻松高效地部署多模型服务。
- **交互式推理模式**：通过缓存多轮对话过程中注意力的 k/v，推理引擎记住对话历史，从而避免重复处理历史会话。
- **优秀的兼容性**：LMDeploy支持 KV Cache Quant，AWQ 和自动前缀缓存同时使用。

LMDeploy 目前已经支持了 InternLM-XComposer2 系列的部署，但值得注意的是 LMDeploy 仅支持了 InternLM-XComposer2 系列模型的视觉对话功能。

### 2. Demo

在软件开发中，demo 通常指的是一个软件或应用程序的原型或简化版本，旨在展示其核心功能或用户界面。这种 demo 版本通常具有以下几个特点：

- **展示功能**：软件开发中的 demo 版本通常只包含最核心或最关键的功能，而不是完整的软件产品。这有助于团队或客户在开发早期阶段了解软件的主要功能和使用场景。

- **用户界面展示**：demo 可以用于展示软件的用户界面设计，帮助用户体验导航和交互。通过 demo，开发团队可以获得用户反馈，以便在正式发布前进行改进。

- **用户测试**：开发团队可以使用 demo 版本来进行用户测试，通过真实用户的反馈来验证软件的可用性和用户体验，并在正式发布前进行优化。



## 闯关任务及过程

### 1. 基础任务

#### 1.1 Cli Demo 部署 InternLM2-Chat-1.8B 模型

> - 任务描述：使用 Cli Demo 完成 InternLM2-Chat-1.8B 模型的部署，并生成 300 字小故事，记录复现过程并截图。
> - 实现步骤：
> 
>   1.准备  Cli Demo 运行环境与部署代码支撑
>   
>   2.准备 InternLM2-Chat-1.8B 模型
>   
>   3.部署模型，完成指定任务

在任务开始前，我们需要在 InternStudio 中准备好一个开发机。创建开发机的过程可参考[这篇文档](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/L0-1-Linux.md)。

本次闯关创建的开发机信息如下：

- 开发机名称：`zps1011_demo`
- 镜像：`Cuda12.2-conda`
- 资源配置：`10% A100 * 1`
- 运行时长：`2小时0分钟`

进入开发机后，在其终端运行以下代码，创建一个新的、名为 zps1011_demo 的 Conda 环境，并利用 pip 安装后续部署模型所需的依赖。总的安装部署的时间较长，大概持续 40 分钟。

```bash
# 创建环境
conda create -n zps1011_demo python=3.10 -y
# 激活环境
conda activate zps1011_demo
# 安装 torch
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia -y
# 安装其他依赖
pip install transformers==4.38
pip install sentencepiece==0.1.99
pip install einops==0.8.0
pip install protobuf==5.27.2
pip install accelerate==0.33.0
pip install streamlit==1.37.0
```

> `-y` 或 `--yes`：
>
> 这个参数用于自动回答所有提示为“是”的问题。在创建环境或安装包时，Conda可能会询问你是否确定要执行某些操作。使用 `-y` 参数可以避免这些提示，使命令执行更加自动化。这对于脚本和自动化部署特别有用。

然后，我们在开发机的控制台中创建一个目录，用于存放我们的代码。再创建一个 `cli_demo.py`。

```bash
mkdir -p /root/demo              # 创建 demo 工作目录
touch /root/demo/cli_demo.py     # 创建 cli_demo.py 文件
```

随后，我们将下面的代码复制到 `cli_demo.py` 中，准备 InternLM2-Chat-1.8B 模型。在 InternStudio 的开发环境中，模型已经被放置在开发机的公共环境目录 `/root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8B` 中，我们也可以直接调用。

```python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM


model_name_or_path = "/root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b"

tokenizer = AutoTokenizer.from_pretrained(model_name_or_path, trust_remote_code=True, device_map='cuda:0')
model = AutoModelForCausalLM.from_pretrained(model_name_or_path, trust_remote_code=True, torch_dtype=torch.bfloat16, device_map='cuda:0')
model = model.eval()

system_prompt = """You are an AI assistant whose name is InternLM (书生·浦语).
- InternLM (书生·浦语) is a conversational language model that is developed by Shanghai AI Laboratory (上海人工智能实验室). It is designed to be helpful, honest, and harmless.
- InternLM (书生·浦语) can understand and communicate fluently in the language chosen by the user such as English and 中文.
"""

messages = [(system_prompt, '')]

print("=============Welcome to InternLM chatbot, type 'exit' to exit.=============")

while True:
    input_text = input("\nUser  >>> ")
    input_text = input_text.replace(' ', '')
    if input_text == "exit":
        break

    length = 0
    for response, _ in model.stream_chat(tokenizer, input_text, messages):
        if response is not None:
            print(response[length:], flush=True, end="")
            length = len(response)
```

接下来，我们在开发机的终端中输入以下命令来启动我们的 Demo。

```bash
python /root/demo/cli_demo.py
```

静待一段时间，待模型加载完成后，我们可以在开发机的终端中与模型进行对话，让其生成一个 300 字的小故事：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-2-8G_Demo-01.png)



#### 1.2 Streamlit Web Demo 部署 InternLM2-Chat-1.8B 模型

> - 任务描述：使用 Streamlit Web Demo 完成 InternLM2-Chat-1.8B 模型的部署，并生成 300 字小故事，记录复现过程并截图。
>
> - 实现步骤：
> 
>   1.准备  Streamlit Web Demo 运行环境与部署代码支撑
>
>   2.准备 InternLM2-Chat-1.8B 模型
>
>   3.部署模型，完成指定任务

在此任务中，将复现如何使用 Streamlit 部署 InternLM2-Chat-1.8B 模型。

我们执行如下代码来把本教程仓库 clone 到本地，以执行后续的代码。

```bash
cd /root/demo                                       # 进入 demo 文件夹
git clone https://github.com/InternLM/Tutorial.git  # 克隆教程仓库到本地
```

然后，在 demo 目录下，我们执行以下代码来启动一个 Streamlit 服务。

```
streamlit run /root/demo/Tutorial/tools/streamlit_demo.py --server.address 127.0.0.1 --server.port 6006
```

接下来，我们在**本地**的终端中输入以下命令，将 6006 端口映射到本地。

```bash
ssh -CNg -L 6006:127.0.0.1:6006 root@ssh.intern-ai.org.cn -p {port}
```

或

```bash
ssh -p {port} root@ssh.intern-ai.org.cn -CNg -L 6006:127.0.0.1:6006 -o StrictHostKeyChecking=no
```

其中 `{port}`是我们在 InternStudio 中创建的开发机的端口号，该端口号可在平台的开发机页面中的 `SSH 连接` 窗口中看到。输入 SSH 连接命令后，可能需要输入密码（进行过免密登录配置的在这里不用输入密码）。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-2-8G_Demo-02.png)

在完成端口映射后，我们便可以通过浏览器访问 `http://localhost:6006` 来启动我们的 Demo。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-2-8G_Demo-03.png)

> - 广东海洋大学（GDOU）不在广州市。GDOU 有四个校区，分布在湛江市与阳江市，其中湖光校区（主校区）、海滨校区、霞山校区位于湛江市；阳江校区位于阳江市。
> - 1.1 中 Cli Demo 部署的输出结果又是正确的。不太明白原因是什么。



### 2. 进阶任务

> - 任务描述：使用 LMDeploy 完成 InternLM-XComposer2-VL-1.8B 和 InternVL2-1.8B 的部署，并完成一次图文理解对话，记录复现过程并截图。
> - 实现步骤：
> 
>   1.准备部署环境
>   
>   2.部署 InternLM-XComposer2-VL-1.8B 模型
>   
>   3.部署 InternVL2-1.8B 模型

在完成基础任务后，可以继续完成进阶任务。

首先，我们在终端里激活环境（如果环境已激活，可不执行激活命令）并安装 LMDeploy 以及其它依赖。

```bash
conda activate zps1011_demo
# WebUI 中对话，需要安装 lmdeploy[serve] 依赖
pip install lmdeploy[all]==0.5.1
pip install timm==1.0.7
```

接下来，我们使用 LMDeploy 启动一个与 InternLM-XComposer2-VL-1.8B 模型交互的 Gradio 服务。因受性能限制，控制 IO 缓存在0.1M，在终端中输入以下命令：执行命令后，需要进行本地电脑与开发机的 SSH 连接与端口映射，将本地 6006 端口与开发机的 6006 端口连接。

```bash
lmdeploy serve gradio /share/new_models/Shanghai_AI_Laboratory/internlm-xcomposer2-vl-1_8b --cache-max-entry-count 0.1
```

但执行这条命令后，遇到`两个错误`：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-2-8G_Demo-04.png)

根据上述提示，我关闭了防火墙和安全软件，并在相应网站下载了对应的文件，重命名后，放入了相应的路径中，问题解决。随后又有新的问题：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-2-8G_Demo-05.png)

我再次检查了网络设置，没有使用 VPN ，能 ping通外网，访问 https://status.gradio.app 正常显示。

目前还没有解决第二个问题，估计问题解决后，可以访问。

正常访问后，在使用 Upload Image 上传图片后，我们输入 Instruction 后按下回车，便可以看到模型的输出。



同样，我们可以通过下面的命令来启动 InternVL2-2B 模型的 Gradio 服务。

```bash
lmdeploy serve gradio /share/new_models/OpenGVLab/InternVL2-2B --cache-max-entry-count 0.1
```

随后，我们需要进行本地电脑与开发机的 SSH 连接与端口映射，将本地 6006 端口与开发机的 6006 端口连接。

在完成端口映射后，我们便可以通过浏览器访问 `http://localhost:6006` 来启动我们的 Demo。





## 总结

本次实操任务中，我们使用`2种 Demo` 与 `LMDeploy` 完成了 InternLM2-Chat-1.8B 模型的部署，并生成了一个 300 字的小故事，通过终端、控制台、WebUI 的多种对话方式，实现了与模型的交互，完成了指定的闯关任务。

通过对 LMDeploy 的了解与应用。使我更加深入地了解大模型的训练、部署和应用。为后续更多更有意义、更加高深、更具有挑战性的实操任务奠定了基础。

## 参考资料

- [8G 显存玩转书生大模型 Demo 知识文档](https://github.com/InternLM/Tutorial/blob/camp3/docs/L1/Demo/readme.md)
- [8G 显存玩转书生大模型 Demo_过程讲解](https://www.bilibili.com/video/BV18x4y147SU/)
- [LMDeploy 的中文教程](https://lmdeploy.readthedocs.io/zh-cn/latest/)

