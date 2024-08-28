<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>基础岛 - XTuner 微调个人小助手认知</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月29日
</div>

## 相关概念

### 1. 微调

大语言模型是在海量文本内容上，以无监督或半监督的方式进行训练的模型，它的训练数据集通常包含了大量的文本数据，如维基百科、新闻、书籍、网页等。这些模型在训练过程中，通过学习文本数据中的统计规律，从而学习到了丰富的语言知识。但是，由于训练数据的局限性，大语言模型在实际应用中还存在一些局限性，如知识时效性受限、专业能力有限、定制化成本高等问题。为了解决这些问题，使大模型能够有更好的应用价值，当下也有多种基本思路解决，如 Prompt Engineering， Finetune（微调）和 RAG（检索增强生成）。

在前面的学习中，我们已经清楚如何编写 Prompt 让 LLM 正确执行任务，了解如何利用 LlamaIndex 框架以及使用 RAG 解决这一问题，本次实践，我们将了解如何通过 Finetune（微调）的方式，来解决大语言模型的局限性。

大语言模型的微调（Fine-tuning）是一种常用的**深度学习技术**，它是在预训练模型的基础上，将模型中一些层的权重参数进行微调，以适应新的数据集或任务。微调是一种基于预训练模型，通过少量的调整（fine-tune）来适应新的任务或数据的方法。

由于大型语言模型的参数规模巨大，且规模日益增大，导致模型的训练和微调成本高昂，直接训练需要耗费大量计算资源和费用。近年来，如何高效地对大模型进行微调成为了研究热点，而 LoRA 和 QLoRA 两种微调技术因其高效性和实用性受到了广泛关注。它们的基本原理是在不改变原有模型权重的情况下，通过外部添加少量新参数来进行微调，这降低了模型的存储需求，也降低了计算成本，实现了对大模型的快速适应，同时保持了模型性能。

### 2. XTuner

XTuner 是一个大语言模型&多模态模型微调工具，由 MMRazor 和 MMDeploy 联合开发。XTuner 以配置文件的形式封装了大部分微调场景，能让零基础的非专业人员也能一键开始微调。对于 7B 参数量的 LLM，微调所需的最小显存仅为 8GB，支持在消费级显卡和 colab 等设备上进行微调。本次实践也将使用该微调工具对 LLM 进行微调。

## 基础任务

> - 任务描述：使用 XTuner 微调 InternLM2-Chat-1.8B 实现自己的小助手认知，记录复现过程并截图。
>
> - 实现步骤：
>
>   - 1.准备 XTuner 运行环境
>
>   - 2.准备微调模型
>
>   - 3.准备微调配置
>
>   - 4.启动微调
>
>   - 5.观察结果

### 1. 准备 XTuner 运行环境

在任务开始前，我们需要在 InternStudio 中准备好一个开发机。本次闯关创建的开发机信息如下：

- 开发机名称：`zps1011_XTuner`
- 镜像：`Cuda12.2-conda`
- 资源配置：`30% A100 * 1`
- 运行时长：`8小时0分钟`（系统默认时间）

进入开发机，执行环境配置代码前，检查并确认是否将 Tutorial 仓库的资料克隆到本地。

```bash
mkdir -p /root/InternLM/Tutorial
git clone -b camp3  https://github.com/InternLM/Tutorial /root/InternLM/Tutorial
```

#### 1.1 创建虚拟环境

在 InterStudio 开发环境中，创建并激活一个新的并命名为 zps1011_xtuner 的 Conda 环境用于执行微调任务，随后在该环境中安装 XTuner ，即运行以下代码：

```bash
# 创建虚拟环境
conda create -n zps1011_xtuner python=3.10 -y

# 激活虚拟环境（注意：后续的所有操作都需要在这个虚拟环境中进行）
conda activate zps1011_xtuner

# 安装一些必要的库
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia -y
# 安装其他依赖
pip install transformers==4.39.3
pip install streamlit==1.36.0
```

#### 1.2 配置 XTuner 运行环境

```bash
# 创建一个目录，用来存放源代码
mkdir -p /root/InternLM/code
cd /root/InternLM/code
git clone -b v0.1.21  https://github.com/InternLM/XTuner /root/InternLM/code/XTuner

# 进入到源码目录
cd /root/InternLM/code/XTuner

# 执行安装
pip install -e '.[deepspeed]'
# 安装速度太慢可以换成以下命令
# pip install -e '.[deepspeed]' -i https://mirrors.aliyun.com/pypi/simple/
```


>-  本次安装的是带有 DeepSpeed 的 XTuner 版本，DeepSpeed 是一个用于训练大型模型的深度学习库，它提供了一种高效的分布式训练方法，能够在大规模 GPU 集群上训练大型模型。即在微调过程中使用 DeepSpeed 可以提高训练速度。
> - 验证 XTuner 安装效果：xtuner version
> - 查看 XTuner 相关帮助：xtuner help

### 2. 准备微调模型

```bash
# 创建一个目录，用来存放微调的所有资料，后续的所有操作都在该路径中进行
mkdir -p /root/InternLM/XTuner
cd /root/InternLM/XTuner
mkdir -p Shanghai_AI_Laboratory
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b Shanghai_AI_Laboratory/internlm2-chat-1_8b
```


> - 执行上述操作后，`Shanghai_AI_Laboratory/internlm2-chat-1_8b` 将直接成为一个符号链接，这个链接指向 `/root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b` 的位置。
>
> - 当我们访问 `Shanghai_AI_Laboratory/internlm2-chat-1_8b` 时，实际上就是在访问 `/root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b` 目录下的内容。通过这种方式，我们无需复制任何数据，就可以直接利用现有的模型文件进行后续的微调操作，这样既节省了空间，也便于管理。
>

#### 2.1 模型微调前对话

```bash
conda activate zps1011_xtuner
streamlit run /root/InternLM/Tutorial/tools/xtuner_streamlit_demo.py
```

> Streamlit 程序的完整代码：[xtuner_streamlit_demo.py](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/Material_submission/xtuner_streamlit_demo.py)。

随后，在本地终端中输入以下命令，进行端口映射。

```bash
ssh -CNg -L 7860:127.0.0.1:8501 root@ssh.intern-ai.org.cn -p {port}
```

其中 `{port}`是我们在 InternStudio 中创建的开发机的端口号，该端口号可在平台的开发机页面中的 `SSH 连接` 窗口中知晓。输入 SSH 连接命令后，可能需要输入密码（进行过公钥配置后，在端口映射时不用输入密码）。之后通过本地浏览器可以访问 `http://localhost:7860`。按`Ctrl + C`退出。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-5-XTuner-01.png)

### 3. 准备微调配置

#### 3.1 准备微调数据

为了让模型能在用户提问的时候按照预期的结果进行回复，我们需要准备微调材料，包括微调数据集、微调配置文件等。我们准备一个数据集文件`datas/assistant.json`，文件内容为对话数据。本次通过脚本生成的方式来准备数据。我们创建一个 Python 文件 `xtuner_generate_assistant.py` 。

```bash
cd /root/InternLM/XTuner
mkdir -p datas
touch datas/assistant.json

touch xtuner_generate_assistant.py
```

打开  `xtuner_generate_assistant.py`  后写入以下代码：

```python
import json

# 设置用户的名字
name = 'zps1011'
# 设置需要重复添加的数据次数
n = 8000

# 初始化数据
data = [
    {"conversation": [{"input": "请介绍一下你自己", "output": "我是{}的小助手，内在是上海AI实验室书生·浦语的1.8B大模型哦".format(name)}]},
    {"conversation": [{"input": "你在实战营做什么", "output": "我在这里帮助{}完成XTuner微调个人小助手的任务".format(name)}]}
]

# 通过循环，将初始化的对话数据重复添加到data列表中
for i in range(n):
    data.append(data[0])
    data.append(data[1])

# 将data列表中的数据写入到'datas/assistant.json'文件中
with open('datas/assistant.json', 'w', encoding='utf-8') as f:
    # 使用json.dump方法将数据以JSON格式写入文件
    # ensure_ascii=False 确保中文字符正常显示
    # indent=4 使得文件内容格式化，便于阅读
    json.dump(data, f, ensure_ascii=False, indent=4)
```

> - 我们需要将脚本中`name`后面的内容修改为`你自己的名字`。
>
> - 如果想要让微调后的模型能够完全认识到你的身份，可以将 `n` 值设置较大些。但是 `n` 值过大会导致过拟合，无法有效回答其他问题。
>
> - `过拟合`就是问什么问题，最后输出都是同一个答案。

在 zps1011_xtner 环境与 /root/InternLM/XTuner 目录下运行以下命令，执行该脚本来生成数据文件：

```bash
python xtuner_generate_assistant.py
```

至此，我们就已经将数据集准备好了，接下来需准备相应的配置文件。

#### 3.2 准备配置文件

参考 [XTuner 的官方文档中对其他数据集的微调配置文件示例](https://github.com/ZK-Jackie/llm_study/blob/master/notes/internlm_study_v3)，我们可以了解到对于自定义的数据集的微调配置的写法。于是，我们可以复制 `internlm2_chat_1_8b_qlora_alpaca_e3`文件，并对其相应部分进行修改，修改后的文件：[internlm2_chat_1_8b_qlora_alpaca_e3](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/Material_submission/internlm2_chat_1_8b_qlora_alpaca_e3_copy.py) 。

```bash
cd /root/InternLM/XTuner
xtuner copy-cfg internlm2_chat_1_8b_qlora_alpaca_e3 .
```

```diff
# 修改模型为本地路径
- 27 pretrained_model_name_or_path = 'internlm/internlm2-chat-1_8b'
+ 27 pretrained_model_name_or_path = '/root/InternLM/XTuner/Shanghai_AI_Laboratory/internlm2-chat-1_8b'

# 修改训练数据为数据集路径和变量名字
- 31 alpaca_en_path = 'tatsu-lab/alpaca'
+ 31 alpaca_en_path = 'datas/assistant.json'

# 修改 evaluation_inputs 评估文本
- 60 '请给我介绍五个上海的景点', 'Please tell me five scenic spots in Shanghai'
+ 60 '请介绍一下你自己', 'Please introduce yourself'

# 修改 alpaca_en 对象中的 dataset 参数
- 102 dataset=dict(type=load_dataset, path=alpaca_en_path),
+ 102 dataset=dict(type=load_dataset, path='json', data_files=dict(train=alpaca_en_path)),

- 105 dataset_map_fn=alpaca_map_fn,
+ 105 dataset_map_fn=None,
```

> 我们还可以对一些重要的参数进行调整，包括学习率（lr）、训练的轮数（max_epochs）等。

### 4. 启动微调

在准备好数据集和配置文件后，使用 XTuner 进行微调，在终端中运行以下代码，微调便会开始：

```bash
xtuner train ./internlm2_chat_1_8b_qlora_alpaca_e3_copy.py
```

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-5-XTuner-02.png)

### 5. 观察结果

待微调就完成后，我们获得了微调后以 pth 结尾的 pytorch 模型文件，整体结构如下所示：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-5-XTuner-03.png)

#### 5.1 模型格式转换

由于我们所使用的模型都是 HuggingFace 模型，还需要对其进行一定处理，以便后续使用。XTuner 中已经集成了有关模型转换、合并、推理、对话等功能，使用 `xtuner convert pth_to_hf` 命令运行以下代码，将 pth 模型转换为 HuggingFace Adapter，并直接借助 Adapter 对话。

```bash
cd /root/InternLM/XTuner
# 先获取最后保存的一个pth文件
pth_file=`ls -t ./work_dirs/internlm2_chat_1_8b_qlora_alpaca_e3_copy/*.pth | head -n 1`
export MKL_SERVICE_FORCE_INTEL=1
export MKL_THREADING_LAYER=GNU
xtuner convert pth_to_hf ./internlm2_chat_1_8b_qlora_alpaca_e3_copy.py ${pth_file} ./hf
```

#### 5.2 模型合并

```bash
export MKL_SERVICE_FORCE_INTEL=1
export MKL_THREADING_LAYER=GNU
xtuner convert merge /root/InternLM/XTuner/Shanghai_AI_Laboratory/internlm2-chat-1_8b ./hf ./merged --max-shard-size 2GB
```

其中也记录了微调的分词器、日志文件、启动配置、检查点等信息，都可以帮助我们保护微调过程恢复，了解微调过程及结果。整体结构如下所示：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-5-XTuner-04.png)

#### 5.3 微调后的模型对话

微调完成后，我们可以再次运行`xtuner_streamlit_demo.py`脚本来观察微调后的对话效果。

在运行之前，我们需要将脚本中的模型路径修改为微调后的模型的路径。

```diff
cd /root/InternLM/Tutorial/tools
# 直接修改脚本文件
- 18 model_name_or_path = "/root/InternLM/XTuner/Shanghai_AI_Laboratory/internlm2-chat-1_8b"
+ 18 model_name_or_path = "/root/InternLM/XTuner/merged"
```

修改完成后运行以下命令，启动应用。

```bash
streamlit run /root/InternLM/Tutorial/tools/xtuner_streamlit_demo.py
```

随后，在本地终端中输入以下命令，进行端口映射。

```bash
ssh -CNg -L 7860:127.0.0.1:8501 root@ssh.intern-ai.org.cn -p {port}
```

最后，通过本地浏览器访问：http://127.0.0.1:7860 进行对话。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-5-XTuner-05.png)

## 总结

在本次实践中，我们了解了微调的基本概念和 XTuner 的使用方式，通过 XTuner 对 InternLM2-Chat-1.8B 模型进行了微调，实现了属于自己的小助手认知。为后续的工作打下了坚实的基础。

## 参考资料

- [XTuner 微调个人小助手认知 - 知识文档](https://github.com/InternLM/Tutorial/blob/camp3/docs/L1/XTuner/readme.md)
- [XTuner - GitHub](https://github.com/InternLM/xtuner)
- [XTuner - 官方文档](https://xtuner.readthedocs.io/)
