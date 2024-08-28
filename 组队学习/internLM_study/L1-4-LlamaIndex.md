<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>基础岛 - InternLM + LlamaIndex RAG 实践</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月27日
</div>

## 相关概念

### 1. RAG 技术

RAG（Retrieval-Augmented Generation）是将传统信息检索系统（例如数据库）的优势与生成式大语言模型 (LLM) 的功能结合在一起。通过将这些额外的知识与自己的语言技能相结合，撰写出更准确、更具时效性且更贴合用户的具体需求的问答系统，主要由两部分组成：

- **Retriever**：检索器，用于从大量的文本数据中检索出与问题相关的文本片段。
- **Generator**：生成器，用于根据检索到的文本片段生成答案。

RAG 模型的基本原理是，首先使用检索器从大量的文本数据中检索出与问题相关的文本片段，然后使用生成器根据检索到的文本片段生成答案。这也意味着必须使用一定的模型，将数据整理成检索器可以处理的格式，然后再使用检索器从中检索出相关的文本片段。在这些过程中，都依赖于 Embedding 技术。

Embedding 技术是一种将文本数据转换为数字向量表示的技术，可以将文本数据转换为向量表示，然后使用向量表示进行检索和生成。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-4-LlamaIndex-01.png)

要实现 RAG 应用，首先需要采用 Embedding 技术将文本信息向量化，随后可以采用向量数据库的方式，将向量化的文本信息存储在数据库中，然后使用检索器从数据库中检索出相关的文本片段，最后使用生成器根据检索到的文本片段生成答案。

### 2. [LlamaIndex](https://github.com/run-llama/llama_index)

LlamaIndex 是一个利用大型语言模型（LLM）构建具备情境增强功能的生成式人工智能应用程序的框架。其实际上充当了一种桥梁角色，专为简化大规模语言模型（LLM）在不同场景下的应用而设计。无论是自动文本补全、智能聊天机器人还是知识驱动的智能助手，LlamaIndex 提供了一套完整的工具链，帮助开发者和企业绕开数据处理及模型调用的繁复细节，直接进入应用开发的核心。

## 基础任务

> - 任务描述：基于 LlamaIndex 构建自己的 RAG 知识库，寻找一个问题 A 在使用 LlamaIndex 之前 InternLM2-Chat-1.8B 模型不会回答，借助 LlamaIndex 后 InternLM2-Chat-1.8B 模型具备回答 A 的能力，截图保存。
>
>- 实现步骤：
> 
>    1.准备 LlamaIndex 运行环境与部署代码支撑
>  
>    2.准备 InternLM2-Chat-1.8B 模型
>  
>    3.准备相关代码数据
>  
>    4.部署模型，完成指定任务

### 1. 准备 LlamaIndex 运行环境与部署代码支撑

在任务开始前，我们需要在 InternStudio 中准备好一个开发机。本次闯关创建的开发机信息如下：

- 开发机名称：`zps1011_llama`
- 镜像：`Cuda11.7-conda`
- 资源配置：`30% A100 * 1`
- 运行时长：`8小时0分钟`（系统默认时间）

初始化时间大概需要 10 分钟左右。进入开发机后，在其终端运行以下代码，创建一个新的、名为 zps1011_llamaindex 的 Conda 环境，并安装 LlamaIndex 及相关 python 软件包依赖。

```bash
# 创建虚拟环境
conda create -n zps1011_llamaindex python=3.10 -y
# 激活虚拟环境
conda activate zps1011_llamaindex

# 安装一些必要的库
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.7 -c pytorch -c nvidia

# 更新 pip 工具
python -m pip install --upgrade pip

# 安装 python 依赖包与 Llamaindex
pip install einops==0.7.0 protobuf==5.26.1
pip install llama-index==0.10.38 llama-index-llms-huggingface==0.2.0 "transformers[torch]==4.41.1" "huggingface_hub[inference]==0.23.1" huggingface_hub==0.23.1 sentence-transformers==2.7.0 sentencepiece==0.2.0
```

构建词向量过程中需要使用到嵌入模型，此处我们需要下载 Sentence Transformer 模型，并将他下载至 /root/llamaindex_demo 文件夹中，在终端中输入以下命令：

```bash
cd ~
mkdir llamaindex_demo
mkdir model
cd ~/llamaindex_demo
touch download_hf.py
```

打开`download_hf.py` ，输入以下代码，指定镜像地址下载。

```bash
import os

# 设置环境变量
os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'

# 下载模型
os.system('huggingface-cli download --resume-download sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2 --local-dir /root/model/sentence-transformer')
```

在 zps1011_llamaindex 环境与 /root/llamaindex_demo 目录下执行以下命令，开始下载：

```bash
cd /root/llamaindex_demo
python download_hf.py
```

构建词向量模型时还会有用到 NLTK 模型，且由于 NLTK 依赖包较大，为避免网络问题，运行以下命令将 NLTK 资源下载并解压到服务器上：

```bash
cd /root
git clone https://gitee.com/yzy0612/nltk_data.git  --branch gh-pages
cd nltk_data
mv packages/*  ./
cd tokenizers
unzip punkt.zip
cd ../taggers
unzip averaged_perceptron_tagger.zip
```

至此，LlamaIndex 的基本运行环境准备就绪。

### 2. 编写、运行代码

运行以下指令，把 `InternLM2 1.8B` 进行软连接。

```bash
cd ~/model
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b/ ./
```

使用以下指令，在 llamaindex_demo 目录下新建一个 python 文件：

```bash
cd ~/llamaindex_demo
touch llamaindex_internlm.py
```

打开 `llamaindex_internlm.py` 文件，并写入以下代码：

```python
from llama_index.llms.huggingface import HuggingFaceLLM
from llama_index.core.llms import ChatMessage
llm = HuggingFaceLLM(
    model_name="/root/model/internlm2-chat-1_8b",
    tokenizer_name="/root/model/internlm2-chat-1_8b",
    model_kwargs={"trust_remote_code":True},
    tokenizer_kwargs={"trust_remote_code":True}
)

rsp = llm.chat(messages=[ChatMessage(content="xtuner是什么？")])
print(rsp)
```

写入完成后在 zps1011_llamaindex 环境与 /root/llamaindex_demo 目录下运行该文件：

```bash
python llamaindex_internlm.py
```

如图所示，由于 InternLM2-Chat-1.8B 模型的限制，它并不能回答有关 XTuner 的相关内容，接下来我们将使用 LlamaIndex 框架，构建一个含 XTuner 相关内容的 RAG 应用，使 InternLM2-Chat-1.8B 模型具备回答 XTuner 的能力。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-4-LlamaIndex-02.png)

### 3. LlamaIndex RAG 应用

在 zps1011_llamaindex 环境下，使用以下命令，安装 `LlamaIndex` 词嵌入向量依赖：

```bash
pip install llama-index-embeddings-huggingface==0.2.0 llama-index-embeddings-instructor==0.1.3
```

运行以下命令，获取知识库：

```bash
cd ~/llamaindex_demo
mkdir data
cd data
git clone https://github.com/InternLM/xtuner.git
mv xtuner/README_zh-CN.md ./
```

输入以下指令，新建一个 python 文件：

```bash
cd ~/llamaindex_demo
touch llamaindex_RAG.py
```

打开`llamaindex_RAG.py`文件，并写入以下代码：

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader, Settings

from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from llama_index.llms.huggingface import HuggingFaceLLM

#初始化一个HuggingFaceEmbedding对象，用于将文本转换为向量表示
embed_model = HuggingFaceEmbedding(
#指定了一个预训练的sentence-transformer模型的路径
    model_name="/root/model/sentence-transformer"
)
#将创建的嵌入模型赋值给全局设置的embed_model属性，
#这样在后续的索引构建过程中就会使用这个模型。
Settings.embed_model = embed_model

llm = HuggingFaceLLM(
    model_name="/root/model/internlm2-chat-1_8b",
    tokenizer_name="/root/model/internlm2-chat-1_8b",
    model_kwargs={"trust_remote_code":True},
    tokenizer_kwargs={"trust_remote_code":True}
)
#设置全局的llm属性，这样在索引查询时会使用这个模型。
Settings.llm = llm

#从指定目录读取所有文档，并加载数据到内存中
documents = SimpleDirectoryReader("/root/llamaindex_demo/data").load_data()
#创建一个VectorStoreIndex，并使用之前加载的文档来构建索引。
# 此索引将文档转换为向量，并存储这些向量以便于快速检索。
index = VectorStoreIndex.from_documents(documents)
# 创建一个查询引擎，这个引擎可以接收查询并返回相关文档的响应。
query_engine = index.as_query_engine()
response = query_engine.query("xtuner是什么?")

print(response)
```

写入完成后在 zps1011_llamaindex 环境与 /root/llamaindex_demo 目录下运行该文件：

```bash
python llamaindex_RAG.py
```

借助 RAG 技术后，在外部知识库的作用下，模型已认识了有关XTuner 的内容，回答更加符合我们预期的答案。

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-4-LlamaIndex-03.png)

### 4. LlamaIndex web 构建

除了通过终端与模型交互，我们还可以通过 web 页面进行对话，具体配置方式可参考 [LlamaIndex_webUI.py](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/Material_submission/LlamaIndex_webUI.py) 。

## 总结

本次实操任务中，我们使用 LlamaIndex 框架构建了一个 RAG 应用，使 InternLM2-Chat-1.8B 模型具备回答 XTuner 相关问题的能力。后续，我们可以深入学习 LlamaIndex 框架，探索更多有趣的应用场景；也可以探索了解 Gradio、Streamlit 、Dash 等框架，制作一个更加成熟的 demo 应用，提供更好的用户体验。

## 参考资料

- [书生大模型实战营【第三期】知识文档 - llamaindex+Internlm2 RAG实践](https://github.com/InternLM/Tutorial/tree/camp3/docs/L1/LlamaIndex)
- [词向量及向量知识库](https://datawhalechina.github.io/llm-universe/#/C3/1.词向量及向量知识库介绍?id=词向量及向量知识库)
- [LlamaIndex - GitHub](https://github.com/run-llama/llama_index/)
- [LlamaIndex 官方文档](https://docs.llamaindex.ai/en/latest/)
