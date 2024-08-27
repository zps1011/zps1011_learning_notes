<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>基础岛 - 浦语提示词工程实践</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月26日
</div>




## 相关概念

### 1. 提示词

Prompt 即提示词，Prompt 最初是 NLP（自然语言处理）研究者为下游任务设计出来的一种任务专属的输入模板，类似于一种任务（例如：分类，聚类等）会对应一种 Prompt。在 ChatGPT 推出并获得大量应用之后，Prompt 开始被推广为给大模型的所有输入，即用户向 LLM 输入 Prompt ，LLM 返回 Completion 内容。

在使用 ChatGPT API 时，可以设置两种 Prompt：

- System Prompt：该种 Prompt 内容会在整个会话过程中**持久地**影响模型的回复，且相比于普通 Prompt 具有更高的重要性。
- User Prompt：该种 Prompt 会在**每次对话**中被模型使用，用于指导模型生成回复。

我们一般设置 System Prompt 来对模型进行一些**初始化设定**。在通过 System Prompt 设定好模型的人设或是初始设置后，我们通过 User Prompt 给出模型需要遵循的指令或提出问题。

### 2. 提示词工程

Prompt Engineering 即提示词工程，是指**设计和使用 Prompt 的过程**。Prompt Engineering 是一个**复杂的过程** ，需要我们根据具体的任务和模型特性来设计和使用 Prompt。

一般来说，Prompt 的设计需要遵循以下几个原则：

- 编写清晰、具体的指令
- 按一定的逻辑顺序组织 Prompt
- 保持简洁，避免Prompt过长

Prompt Engineering 过程中，大致包括以下几个步骤：

- 初始化：在小样本中调整 Prompt ，尝试使其在这些样本上起效。
- 解决 Bad Case：进行进一步测试时，可能会遇到一些棘手的例子，这些例子无法通过 Prompt 或者算法解决。在这种情况下，可以将这些额外的几个例子添加到正在测试的集合（开发集迭代）中，有机地添加其它难以处理的例子。
- 自动评估：开发集不断扩大至不便测试的规模时，可开始开发一些用于衡量这些小样本集性能的指标，例如平均准确度。

不断地重复进行上述过程，直至我们构建出一个高效的 Prompt，使得模型能够更好地完成我们的需求。

### 3. LangGPT

LangGPT 是一个帮助用户编写高质量提示词的工具，并具有一套模块化、标准化的提示词编写方法论——结构化提示词。其开发团队的愿景是每一个人都能够写出高质量的提示词，更好地利用大模型的能力。

LangGPT 团队分别在 OpenAI 商店和 月之暗面 Kimi 中发布了 LangGPT 辅助提示词编写工具，本次实操任务中，我们将使用 LangGPT 的工具，优化提示词，使 LLM 输出正确结果。

## 基础任务

> - 背景问题：近期相关研究发现，LLM在对比浮点数字时表现不佳，经验证，internlm2-chat-1.8b (internlm2-chat-7b)也存在这一问题，例如认为 13.8<13.11。
>
> - 任务要求：利用 LangGPT 优化提示词，使LLM输出正确结果。
>
> - 实现步骤：
>
>   1.部署 internlm2-chat-1.8b 为 OpenAI 式的通用接口
>
>   2.向 LangGPT 提交提示词请求
>
>   3.向 internlm2-chat-1.8b 提交提示词请求
>
>   4.优化提示词，使LLM输出正确结果
>   
>   5.重复步骤 1-3，直至LLM输出正确结果，记录优化过程并截图

### 1. 部署 internlm2-chat-1.8b 为 OpenAI 式的通用接口

在任务开始前，我们需要在 InternStudio 中准备好一个开发机。创建开发机的过程可参考[这篇文档](https://github.com/zps1011/zps1011_learning_notes/blob/main/组队学习/internLM_study/L0-1-Linux.md)。

本次闯关创建的开发机信息如下：

- 开发机名称：`zps1011_langgpt`
- 镜像：`Cuda12.2-conda`
- 资源配置：`10% A100 * 1`
- 运行时长：`3小时0分钟`

进入开发机后，在其终端运行以下代码，创建一个新的、名为 zps1011_langgpt 的 Conda 环境，并利用 pip 安装后续部署模型所需的依赖。

```bash
# 创建虚拟环境
conda create -n zps1011_langgpt python=3.10 -y
# 激活虚拟环境
conda activate zps1011_langgpt

# 安装一些必要的库
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia -y

# 更新 pip 工具
python -m pip install --upgrade pip

# 安装其他依赖
pip install transformers==4.43.3
pip install streamlit==1.37.0
pip install huggingface_hub==0.24.3
pip install openai==1.37.1
pip install lmdeploy==0.5.2
```

为了更加方便管理代码文件，我们可以在本地工作目录下创建一个名为`langgpt`的文件夹，在文件夹内进行相关的部署操作，在控制台中输入以下命令：

```bash
## 创建路径
mkdir langgpt
## 进入项目路径
cd langgpt
```

安装终端复用软件 tmux 

```bash
apt-get install tmux
```

在 InternStudio 的开发环境中，模型已经被放置在开发机的公共环境目录 `/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8B` 中，我们可以直接调用。调用后，我们需要部署模型为 OpenAI 式的通用接口。

为了保持服务的持续运行，我们需要让进程在后台持续执行，所以使用`tmux`软件创建新的命令窗口。运行如下命令创建窗口：

```bash
tmux new -t langgpt
```

创建成功后如下图所示，进入命令窗口后，需要在新窗口中再次激活环境。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-3-Prompt-01.png)

首次创建完成后会自动进入窗口，再次进入需要运行以下命令进入：

```bash
tmux a -t langgpt
```

使用 LMDeploy 进行部署，输入以下命令。执行时会发现，显存占用率高达95%。

```bash
CUDA_VISIBLE_DEVICES=0 lmdeploy serve api_server /share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b --server-port 23333 --api-keys internlm2
```

如下图所示，即为部署成功：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-3-Prompt-02.png)

部署完成后，我们在开发机的 `~/langgpt` 路径中创建一个名为 `demo_api.py` 的文件，并写入以下代码：

```python
from openai import OpenAI

client = OpenAI(
    api_key = "internlm2",
    base_url = "http://0.0.0.0:23333/v1"
)

response = client.chat.completions.create(
    model=client.models.list().data[0].id,
    messages=[
        {"role": "user", "content": "请介绍一下广东海洋大学"},
    ]
)

print(response.choices[0].message.content)
```

在另一个控制台中输入以下命令，运行 `demo_api.py` 文件，与模型进行对话，获得相应输出以测试是否部署成功。

```bash
python ~/langgpt/demo_api.py
```

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-3-Prompt-03.png)

服务启动完成后，按`Ctrl+B`进入tmux的控制模式，再按`D`离开当前会话。

InternLM 部署完成后，可利用提供的[`chat_ui.py`](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/Material_submission/chat_ui.py)创建图形化界面。

我们从Github获取项目，运行如下命令：

```bash
git clone https://github.com/InternLM/Tutorial.git
```

下载完成后，运行如下命令进入项目所在的路径：

```bash
cd Tutorial/tools
```

进入相应路径后，输入以下代码运行项目：

```bash
python -m streamlit run chat_ui.py
```

随后，在本地终端中输入以下命令，进行端口映射。

```bash
ssh -CNg -L 7860:127.0.0.1:8501 root@ssh.intern-ai.org.cn -p {port}
```

或

```bash
ssh -p {port} root@ssh.intern-ai.org.cn -CNg -L 7860:127.0.0.1:8501 -o StrictHostKeyChecking=no
```

其中 `{port}`是我们在 InternStudio 中创建的开发机的端口号，该端口号可在平台的开发机页面中的 `SSH 连接` 窗口中看到。输入 SSH 连接命令后，可能需要输入密码（进行过公钥配置的在端口映射时不用输入密码）。之后通过本地浏览器可以访问 `http://localhost:7860`。

左侧边栏为对话的部分设置，其中设置的第一项为最大 token 长度，将其设置为 0 时表示不限制生成的最大 token 长度。API Key 和 Base URL 在部署 InternLM 时已经设置。点击`保存设置`之后，就会启动对话界面：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-3-Prompt-04.png)

### 2. 使用 LangGPT 优化提示词，使 LLM 输出正确结果

本次任务需解决的是大语言模型比较浮点数字时表现不佳的问题，因此我们需要向 LangGPT 提交这一类提示词请求。

#### 2.1 向 LangGPT 提交提示词请求

在`没有提交提示词`的请求时，InternLM2-Chat-1.8B 的回答内容如下所示：

| 用户输入内容             | InternLM2-Chat-1.8B 回答 |
| ------------------------ | ------------------------ |
| 13.8 和 13.11 哪个更小？ | 13.8 比 13.11 更小。     |
| 13.8 和 13.11 哪个更大？ | 13.11 比 13.8 更大。     |
| 3.8 和 3.11 哪个更大？   | 3.11 比 3.8 更大。       |

我们需要前往 [LangGPT 提示词助手页面](https://chatgpt.com/g/g-Apzuylaqk-langgpt-ti-shi-ci-zhuan-jia)，向 LangGPT 提交提示词请求，下图为我提交的提示词请求， LangGPT 助手返回结果后，便能向 InternLM2-Chat-1.8B 发送该提示词请求。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-3-Prompt-05.png)

#### 2.2 向 internlm2-chat-1.8b 提交提示词请求

根据 [1. 部署 internlm2-chat-1.8b 为 OpenAI 式的通用接口](#1-部署-internlm2-chat-1-8b-为-OpenAI-式的通用接口) 后，接下来我们向模型询问一些浮点数比较问题，检查它的回答是否正确。

多轮对话后的输出结果如下表所示，后续还需要优化提示词。

| 用户输入内容             | InternLM2-Chat-1.8B 回答                                     |
| ------------------------ | ------------------------------------------------------------ |
| 13.8 和 13.11 哪个更大？ | 根据您提供的浮点数，13.8 和 13.11 的大小关系如下：13.8 大于 13.11 。因此，13.8  更大。 |
| 13.8 和 13.11 哪个更小？ | 根据您提供的浮点数，13.8 和 13.11 的大小关系如下：13.11 大于 13.8 。因此，13.11 更小。 |
| 3.8 和 3.11 哪个更大？   | 根据您提供的浮点数，3.8 和 3.11 的大小关系如下：3.8 大于 3.11 。因此，3.8  更大。 |

## 总结

本次实操任务中，我们部署  InternLM2-Chat-1.8B 模型为 OpenAI 式的通用接口，通过测试并让其生成了相应的内容，在上一节实践中，实现通过控制台、WebUI 的部署与对话；在本节通过 API 接口的方式，实现了模型的部署与对话。同时，我们使用 LangGPT 完成了提示词的初步编写，使得 InternLM2-Chat-1.8B 能够更好地完成浮点数值比较问题。

## 参考资料

- [浦语提示词工程实践 - 知识文档](https://github.com/InternLM/Tutorial/tree/camp3/docs/L1/Prompt)
- [动手学大模型应用开发 - Prompt Engineering](https://datawhalechina.github.io/llm-universe/#/C2/3.%20Prompt%20Engineering)
- [LangGPT - GitHub](https://github.com/langgptai/LangGPT)
- [LangGPT 提示词专家](https://chatgpt.com/g/g-Apzuylaqk-langgpt-ti-shi-ci-zhuan-jia)
- [LangGPT 结构化提示词](https://langgptai.feishu.cn/wiki/RXdbwRyASiShtDky381ciwFEnpe)
- [LMDeploy - GitHub](https://github.com/internlm/lmdeploy/)
- [LMDeploy 官方文档](https://lmdeploy.readthedocs.io/zh-cn/latest)

## 附录

- [基础任务提交材料](https://github.com/zps1011/zps1011_learning_notes/tree/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/Material_submission/L1-3-base_task)
