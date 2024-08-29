<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>基础岛 - OpenCompass 评测 InternLM2 1.8B 实践</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月29日
</div>


## 相关概念

### 1. 大模型评测

大模型评测对于模型的性能评估至关重要。通过全面、客观、公正的评测，我们才能够了解一款模型的性能、能力、安全性等各个方面的表现，这不仅有助于我们更好地了解模型的优劣，也有助于我们更好地选择模型、优化模型，帮助、激励开发者开发出更加优秀的模型作品。不仅仅是对于开发者，对于普通用户、管理机构、产业界等，大模型评测都有着重要的意义。对于模型回答的内容和答案的准确性、流畅性等，我们也常用 BLEU、ROUGE、Perplexity 等指标来进行评测。

在模型和评测方法层出不穷的情况下，一个全面、公正、权威的评测体系，来帮助我们更好地评测大模型是十分关键的，OpenCompass 便是这样一个评测体系 、工具和平台。

### 2. OpenCompass

OpenCompass 是一个开源的大模型评测框架，致力于打造全球领先的大模型开源评测体系。OpenCompass 通过提供统一的评测标准、评测方法、评测工具，帮助用户更好地评测大模型的性能、能力等，帮助用户更好地选择、优化模型。

OpenCompass “集百家之长”，在各个方面都有着很高的实力。OpenCompass 的实力也成功令其用户遍及国内外知名企业与科研机构，成为 Meta 官方推荐的唯一由国内开发的大模型评测体系，与 HuggingFace、Stanford 和 Google 推出的评测体系齐名。

在 OpenCompass 中评估一个模型通常包括以下几个阶段：配置 -> 推理 -> 评估 -> 可视化。

- 配置：这是整个工作流的起点。您需要配置整个评估过程，选择要评估的模型和数据集。此外，还可以选择评估策略、计算后端等，并定义显示结果的方式。
- 推理与评估：在这个阶段，OpenCompass 将会开始对模型和数据集进行并行推理和评估。推理阶段主要是让模型从数据集产生输出，而评估阶段则是衡量这些输出与标准答案的匹配程度。这两个过程会被拆分为多个同时运行的“任务”以提高效率。
- 可视化：评估完成后，OpenCompass 将结果整理成易读的表格，并将其保存为 CSV 和 TXT 文件。

### 3. C-Eval 数据集

C-Eval 发布于2023年5月22日，是一个相对较新的评估基准。C-Eval 数据集是一个全面、高质量、多学科的**中文大模型**评估基准，通过一系列多选题来评估模型在知识和推理能力方面的表现，为中文社区的大模型研发者提供了有力的评估工具和改进方向，有助于推动中文大模型技术的不断进步和发展。

## 基础任务

> - 任务描述：使用 OpenCompass 评测 internlm2-chat-1.8b 模型在  C-Eval 数据集上的性能，记录复现过程并截图。
>
> - 实现步骤：
>
>    1.准备 OpenCompass 运行环境
>    
>    2.准备  C-Eval 数据集
>    
>    3.启动评测任务
>    
>    4.总结评测结果

### 1. 准备 OpenCompass 运行环境

在任务开始前，我们需要在 InternStudio 中准备好一个开发机。本次闯关创建的开发机信息如下：

- 开发机名称：`zps_OpenCompass`
- 镜像：`Cuda11.7-conda`
- 资源配置：`10% A100 * 1`
- 运行时长：`12小时0分钟`（OpenCompass 评测时间较长）

在 InterStudio 开发环境中，创建并激活一个新的并命名为 zps1011_OpenCompass 的 Conda 环境用于执行任务，依次运行以下代码：

```bash
# 创建虚拟环境
conda create -n zps1011_OpenCompass python=3.10 -y
# 激活虚拟环境
conda activate zps1011_OpenCompass
# 安装一些必要的库
conda install pytorch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 pytorch-cuda=12.1 -c pytorch -c nvidia -y
# 配置其它依赖
cd /root
git clone -b 0.2.4 https://github.com/open-compass/opencompass
cd opencompass
# 以“可编辑模式”（editable mode）安装当前目录（. 表示当前目录）下的包或应用
pip install -e .
# 更新软件包列表、安装软件以及管理Python项目依赖
apt-get update
apt-get install cmake
pip install -r requirements.txt
pip install protobuf
```

### 2. 准备 C-Eval 数据集

在 git clone opencompass 时一定要将 opencompass clone 到 /root 路径下。解压后将会在 OpenCompass 下看到 data 文件夹。

```bash
# 解压评测数据集到 /root/opencompass/data/ 处。
cp /share/temp/datasets/OpenCompassData-core-20231110.zip /root/opencompass/
unzip OpenCompassData-core-20231110.zip
```

### 3. 启动评测任务

#### 3.1 使用命令行配置参数法进行评测

在 /root/opencompass/configs/models/hf_internlm/ 的路径中打开 `hf_internlm2_chat_1_8b.py` ，写入以下代码，代码主要对模型的路径进行了修改：

```python
from opencompass.models import HuggingFaceCausalLM


models = [
    dict(
        type=HuggingFaceCausalLM,
        abbr='internlm2-1.8b-hf',
        path="/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b",
        tokenizer_path='/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b',
        model_kwargs=dict(
            trust_remote_code=True,
            device_map='auto',
        ),
        tokenizer_kwargs=dict(
            padding_side='left',
            truncation_side='left',
            use_fast=False,
            trust_remote_code=True,
        ),
        max_out_len=100,
        min_out_len=1,
        max_seq_len=2048,
        batch_size=8,
        run_cfg=dict(num_gpus=1, num_procs=1),
    )
]
```

通过以下命令评测 InternLM2-Chat-1.8B 模型在 C-Eval 数据集上的性能。由于 OpenCompass 默认并行启动评估过程，我们在运行时以 --debug 模式启动评估，并检查评估过程中是否存在问题。在 --debug 模式下，任务将按顺序执行，并实时打印输出。

```bash
# 环境变量配置
export MKL_SERVICE_FORCE_INTEL=1
# 或
export MKL_THREADING_LAYER=GNU
# 启动评测
python run.py --datasets ceval_gen --models hf_internlm2_chat_1_8b --debug
```

评测启动后如下图所示：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-6-OpenCompass-01.png)



#### 3.2 使用配置文件修改参数法进行评测

除了通过命令行配置实验外，OpenCompass 还允许用户在配置文件中编写实验的完整配置。配置文件是以 Python 格式组织的，并且必须包括 datasets 和 models 字段。本次测试配置在 `configs`文件夹 中，通过 `继承机制` 引入所需的数据集和模型配置，并以所需格式组合 datasets 和 models 字段。 运行以下代码，在 configs 文件夹下创建`eval_tutorial_demo.py` 。

```bash
cd /root/opencompass/configs
touch eval_tutorial_demo.py
```

打开 `eval_tutorial_demo.py` ，写入以下代码：

```python
from mmengine.config import read_base

with read_base():
    from .datasets.ceval.ceval_gen import ceval_datasets
    from .models.hf_internlm.hf_internlm2_chat_1_8b import models as hf_internlm2_chat_1_8b_models

datasets = ceval_datasets
models = hf_internlm2_chat_1_8b_models
```

运行以下代码，将配置文件的路径传递给 run.py。

```bash
cd /root/opencompass
python run.py configs/eval_tutorial_demo.py --debug
```

评测开始后如下图所示：

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-6-OpenCompass-02.png)



### 4. 总结评测结果

总计 8 个小时左右，评测任务结束，我们可以在控制台中查看评测结果。根据评测结果，我们可以得到 internlm2-chat-1.8b 模型在  C-Eval 数据集上的性能表现。OpenCompass 评测工具还会生成评测报告，我们可以根据控制台返回路径的信息，查看报告中的详细信息。

部分测评报告如下图所示：

- 使用**命令行**配置参数法的测评结果

  ![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-6-OpenCompass-03.png)



- 使用**配置文件**修改参数法的测评结果

  ![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-6-OpenCompass-04.png)

> it/s 表示的是每次迭代所花费的时间。例如，如果显示 "1.00s/it"，则表示每次迭代需要花费 1 秒钟。如果显示 "it/s"，则表示迭代速度非常快，每次迭代花费的时间小于 1 秒。



**详细报告结果见[附录](#附录)**



## 总结

本次任务是使用 OpenCompass 评测 internlm2-chat-1.8b 模型在  C-Eval 数据集上的性能，通过本次实操，我们了解了 OpenCompass 的评测方式、评测工具、评测流程，以及如何使用 OpenCompass 对大模型进行评测。希望随着 OpenCompass 的发展，为大模型评测提供更多的帮助，真正地成为开发者、用户心中的权威 “指南针”。

## 参考资料

- [OpenCompass 评测 InternLM2 1.8B - 知识文档](https://github.com/InternLM/Tutorial/blob/camp3/docs/L1/OpenCompass/readme.md)
- [OpenCompass 教程](https://opencompass.readthedocs.io/zh-cn/latest/get_started/quick_start.html)
- [OpenCompass - GitHub](http://github.com/open-compass/opencompass)
- [OpenCompass - 官方文档](https://opencompass.org.cn/doc)

## 附录

- [使用命令行配置参数法的测评报告 - zps1011_CLI_20240829_162917.zip](https://github.com/zps1011/zps1011_learning_notes/tree/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/Material_submission/L1-6-base_task)

- [使用配置文件修改参数法的测评报告 - zps1011\_配置文件_20240829_202005.zip](https://github.com/zps1011/zps1011_learning_notes/tree/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/Material_submission/L1-6-base_task)

