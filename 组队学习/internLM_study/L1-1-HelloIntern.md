<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>基础岛 - 书生·浦语大模型全链路开源开放体系</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月21日
</div>

## 关于大语言模型

### 1. 大模型介绍

大语言模型（LLM，Large Language Model），也称大型语言模型，是一种旨在理解和生成人类语言的人工智能模型。

### 2. 常见的 LLM 模型

国外知名的 LLM 有：GPT-3.5、GPT-4、PaLM、Claude 和 LLaMA 等。

国内知名的 LLM 有：文心一言、讯飞星火、通义千问、ChatGLM、百川等。

### 3. LLM 的能力

- **上下文学习**：根据提供自然语言指令或多个任务示例中，通过理解上下文并生成相应输出的方式来执行任务，而无需额外的训练或参数更新。

- **指令遵循**：LLM 能够根据任务指令执行任务，而无需事先见过具体示例，展示了其强大的泛化能力。

- **逐步推理**：LLM 通过采用思维链（CoT, Chain of Thought）推理策略，利用包含中间推理步骤的提示机制来解决这些任务，从而得出最终答案。

### 4. LLM 的特点

- **巨大的规模**：可达数十亿甚至数千亿个参数。能捕捉更多的语言知识和复杂的语法结构。

- **预训练和微调**：在大规模文本数据上进行预训练，学习通用的语言表示和知识。然后通过微调适应特定任务。

- **上下文感知**：能够理解和生成依赖于前文的文本内容。

- **多语言与模态支持**：可理解多种语言和生成不同媒体类型的内容。

- **伦理和风险问题**：引发了包括生成有害内容、隐私问题、认知偏差等。

- **高计算资源需求**：参数规模庞大，需要大量的计算资源进行训练和推理。通常需要使用高性能的 GPU 或 TPU 集群来实现。

## 书生·浦语开源大模型

### 1. 书生·浦语大模型开源历程

2023 年 6 月 7 日，上海人工智能实验室发布浦语系列首个千亿级参数的大语言模型，同年 7 月 6 日发布了支持 8K 语境、26 种语言、全面开源、免费商用的 InternLM-7B 模型、全链路开源工具体系。同时开源有关 InternLM 训练、微调、部署、评测、应用等一系列数据集及工具与框架，目前已经推出了 InternLM 2.5 版本。

书生·浦语提供了一整套完整的开源体系，包括：

- 书生-万卷：首个精细处理的开源多模态语料库
- [InternEvo](https://github.com/InternLM/InternEvo/)：轻量级大模型预训练框架
- [XTuner](#2-XTuner)：轻量化大模型微调工具
- [LMDeploy](#3-LMDeploy)：LLM 轻量化高效部署工具
- [OpenCompass](#4-OpenCompass)：客观评估大模型性能的开源工具
- [MinerU](#5-MinerU)：高质量数据提取、文档解析工具
- [Lagent](#6-Lagent)：大语言模型智能体框架
- ……

![zps1011](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L1-1-HelloIntern-01.png)

### 2. [XTuner](https://github.com/InternLM/xtuner)

XTuner 是上海人工智能实验室开发的一个大语言模型&多模态模型微调工具箱，由 MMRazor 和 MMDeploy 联合开发，它内部集成了大量的微调场景，能够帮助用户在不同场景下高效地对大型语言模型和多模态图文模型进行微调和优化。XTuner具有以下特点：

- **傻瓜化**：以配置文件的形式封装了大部分微调场景，0 基础的非专业人员也能一键开始微调。

- **轻量级**：对于 7B 参数量的 LLM，微调所需的最小显存仅为 8GB，消费级显卡和 colab 都可以轻松运行。

- **高效性**：XTuner 专为提高大型模型的微调效率而设计，能够快速处理大规模数据和复杂模型，减少训练时间。

- **灵活性**：XTuner 支持多种模型和任务的微调，用户可以根据具体需求进行自定义设置和训练，适用于各种场景。

- **全面性**：XTuner 提供全面的功能，包括支持 LLM（大型语言模型）和 VLM（视觉语言模型）的预训练、模型转换、以及自定义训练过程，满足从模型开发到应用部署的全流程需求

### 3. [LMDeploy](https://github.com/InternLM/lmdeploy)

LMDeploy 由 [MMDeploy](https://github.com/open-mmlab/mmdeploy) 和 [MMRazor](https://github.com/open-mmlab/mmrazor) 团队联合开发，是涵盖了 LLM 任务的全套轻量化、部署和服务解决方案。 这个强大的工具箱提供以下核心功能：
- **高效的推理**：LMDeploy 开发了 Persistent Batch(即 Continuous Batch)，Blocked K/V Cache，动态拆分和融合，张量并行，高效的计算 kernel等重要特性。推理性能是 vLLM 的 1.8 倍

- **可靠的量化**：LMDeploy 支持权重量化和 k/v 量化。4bit 模型推理效率是 FP16 下的 2.4 倍。量化模型的可靠性已通过 OpenCompass 评测得到充分验证。

- **便捷的服务**：通过请求分发服务，LMDeploy 支持多模型在多机、多卡上的推理服务。

- **有状态推理**：通过缓存多轮对话过程中 attention 的 k/v，记住对话历史，从而避免重复处理历史会话。显著提升长文本多轮对话场景中的效率。

- **卓越的兼容性**: LMDeploy 支持 [KV Cache 量化](https://github.com/InternLM/lmdeploy/blob/main/docs/zh_cn/quantization/kv_quant.md), [AWQ](https://github.com/InternLM/lmdeploy/blob/main/docs/zh_cn/quantization/w4a16.md) 和 [Automatic Prefix Caching](https://github.com/InternLM/lmdeploy/blob/main/docs/zh_cn/inference/turbomind_config.md) 同时使用。

### 4. [OpenCompass](https://github.com/open-compass/opencompass)

大模型评测是大模型研究的一个重要环节。通过对大模型能力的量化评估，与其它大模型的参数对比，可以更好地了解大模型的性能，也可以更好地指导大模型的研究与开发。

OpenCompass 便是一款专注于大模型评测的开源工具。OpenCompass 由上海人工智能实验室开发，专注于进行大模型的客观评测，它具有以下特点：

- **全面**：OpenCompass 覆盖了大模型的学科、语言、知识、理解和推理能力等多个方面，可以全面评估大模型的性能。
- **客观**：OpenCompass 采用了客观的评测标准，可以更好地评估大模型的性能，避免了主观因素的干扰。
- **开源**：OpenCompass 是一款开源工具，用户可以自由使用，也可以根据自己的需求进行二次开发。
- **易用**：OpenCompass 的使用非常简单，用户只需要按照文档的指引，即可轻松进行大模型的评测。

### 5. [MinerU](https://github.com/opendatalab/MinerU)

MinerU 是一个开源的高质量数据提取工具，专门用于处理各种文档格式，如 PDF、网页和电子书，旨在提取结构化数据，同时保持语义的连续性。MinerU 提供了以下功能：

- **文本提取**：将 PDF 和其他文档转换为 Markdown 和 JSON 等机器可读格式，同时保留文档的原始结构，包括标题、段落和列表。
- **表格和公式识别**：自动识别并将文档中的表格和公式转换为 LaTeX，确保科学内容的准确呈现。
- **多列支持**：有效处理多列文档，并以逻辑的、人类可读的顺序输出文本。
- **图像和 OCR**：提取图像、图注，并使用 OCR 处理损坏的 PDF，支持 CPU 和 GPU 环境。
- **跨平台兼容性**：适用于 Windows、Linux 和 macOS，且在支持的系统上提供 GPU 加速。

### 6. [Lagent](https://github.com/InternLM/lagent)

Lagent 是上海人工智能实验室开发的一款大语言模型智能体框架，它可以帮助用户更好地将大语言模型转变为多种类型的智能体，并提供了一些典型工具为大语言模型赋能。其可以执行以下功能：

- **对话生成**：根据用户输入生成自然且连贯的对话回应。

- **信息提取**：从文本中提取出关键信息，例如实体、事件或关系。

- **文本生成**：根据输入生成高质量的文本，例如文章、报告或创作内容。

- **语言理解**：解析和理解复杂的自然语言文本，帮助解决各种语言处理任务。

![Agent结构](https://github.com/InternLM/lagent/assets/24351120/cefc4145-2ad8-4f80-b88b-97c05d1b9d3e)


### 7. 其它

上海人工智能实验室也在积极探索及研发大模型在更多领域的应用场景，除了上述的应用或工具外，还有诸多开源工具、应用及框架：

- [InternVL: 多模态对话模型](https://github.com/OpenGVLab/InternVL)
- [茴香豆：智能聊天机器人](https://github.com/InternLM/HuixiangDou)
- [MindSearch: 基于大模型的多模态搜索引擎](https://github.com/InternLM/MindSearch)
- [Agent Lego：多功能工具 API 库](https://github.com/InternLM/agentlego?tab=readme-ov-file)
- [OpenAOE：一款优雅且开箱即用的聊天界面](https://github.com/InternLM/OpenAOE)
- ......

## 总结

通过本次的学习，了解了书生·浦语大模型全链路的开源体系。惊叹于书生·浦语提供的数据、预训练、微调、部署、评测、应用等一系列工具与框架。从这些工具与框架的开源，为我们揭开大模型神秘的面纱，也为大模型的应用与研发提供了更为广阔的创作空间。

## 参考资料

- [书生·浦语大模型全链路开源开放体系介绍](https://www.bilibili.com/video/BV18142187g5)
- [书生·浦语官网 ](https://internlm.intern-ai.org.cn/)
- [InternLM](https://github.com/InternLM)
- [OpenCompass 司南](https://opencompass.org.cn/home)
- [OpenCompass 中文教程](https://opencompass.readthedocs.io/zh-cn/latest/)
- [Open X Lab 浦源](https://openxlab.org.cn/home)
