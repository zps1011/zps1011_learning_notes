<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>Datawhale X 魔搭 AI 夏令营<br/><span>从零入门AI生图原理&实践</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录时间：2024年8月9日&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后更新时间：2024年8月17日
</div>

### 一、赛事解读

##### 1. 赛事链接

https://modelscope.cn/brand/view/Kolors?branch=0&tree=0

##### 2. 赛题任务

- 模型训练： 参赛者需要在可图 Kolors 模型的基础上训练 LoRA 模型，这是一种灵活的图像生成模型，可以生成多种风格的图像，如水墨画、水彩画、赛博朋克风格、日漫风格等。
- 故事创作与图像生成： 基于训练好的 LoRA 模型，生成 8 张风格一致的图像来讲述一个连贯的故事。故事内容可以自定，需要通过这些图像清晰地表达。

##### 3. 提交与评分

- 模型与作品提交： 参赛者需要上传训练好的 LoRA 模型和至少 8 张图像及其对应的 prompt（生成图像时所用的文本提示）到指定平台。

- 评分标准：
  - 主观评分： 评审将基于技术运用（40%）、组图风格连贯性（30%）、整体视觉效果（30%）来评价。
  
  - 客观评分： 用于评价提交是否有效，美学分数低于 6 视为无效提交。

  - 美学评分代码

    ```python
    !pip install simple-aesthetics-predictor
    
    import torch, os
    from PIL import Image
    from transformers import CLIPProcessor
    from aesthetics_predictor import AestheticsPredictorV2Linear
    from modelscope import snapshot_download
    
    
    model_id = snapshot_download('AI-ModelScope/aesthetics-predictor-v2-sac-logos-ava1-l14-linearMSE', cache_dir="models/")
    predictor = AestheticsPredictorV2Linear.from_pretrained(model_id)
    processor = CLIPProcessor.from_pretrained(model_id)
    device = "cuda"
    predictor = predictor.to(device)
    
    
    def get_aesthetics_score(image):
        inputs = processor(images=image, return_tensors="pt")
        inputs = {k: v.to(device) for k, v in inputs.items()}
        with torch.no_grad():
            outputs = predictor(**inputs)
        prediction = outputs.logits
        return prediction.tolist()[0][0]
    
    
    def evaluate(folder):
        scores = []
        for file_name in os.listdir(folder):
            if os.path.isfile(os.path.join(folder, file_name)):
                image = Image.open(os.path.join(folder, file_name))
                scores.append(get_aesthetics_score(image))
        if len(scores) == 0:
            return 0
        else:
            return sum(scores) / len(scores)
    
    
    score = evaluate("./images")
    print(score)
    ```
  
    

### 二、文生图知识

**文生图**（Text-to-Image，简称“文生图”）是一种 AI 技术，它可以根据文本描述自动生成图像。这种技术使用了深度学习模型，尤其是变换器（Transformer）架构，来理解文本中的内容和上下文，并将这些信息转化为视觉表示。

**DiffSynth-Studio** 是一个基于扩散模型的文生图技术。扩散模型是一种深度学习方法，可以生成高质量的图像。它的工作原理是首先向数据中添加噪声，然后通过一个训练有素的模型逐步去除这些噪声，最终生成清晰的图像。

**Data-Juicer**：数据处理和转换工具，旨在简化数据的提取、转换和加载过程。

### 三、baseline

##### 1. baseline及注释

（1）终端运行克隆 Git 仓库、下载 baseline 文件

```python
git lfs install  # 安装 Git LFS
git clone https://www.modelscope.cn/datasets/maochase/kolors.git  # 克隆包含 LFS 的仓库
```

（2）进入文件夹，打开 baseline 文件

- 安装 Data-Juicer 和 DiffSynth-Studio

  ```python
  # 安装simple-aesthetics-predictor包，用于评估图像的美学和视觉质量
  !pip install simple-aesthetics-predictor
  
  # 以开发模式安装data-juicer包，允许直接修改源代码并使用，适用于数据处理项目
  !pip install -v -e data-juicer
  
  # 卸载已安装的pytorch-lightning包，通常是为了解决版本冲突或重新安装不同版本
  !pip uninstall pytorch-lightning -y
  
  # 安装peft、lightning、pandas和torchvision这四个库，分别用于项目特定功能、深度学习模型训练、数据分析和图像处理
  !pip install peft lightning pandas torchvision
  
  # 以开发模式安装DiffSynth-Studio，这是一个可能与音频或图像合成相关的项目，允许在不重新安装的情况下修改项目代码
  !pip install -e DiffSynth-Studio
  ```
  
- 加载数据集

  ```python
  # 从modelscope.msdatasets库导入MsDataset类，用于加载和操作ModelScope的数据集
  from modelscope.msdatasets import MsDataset
  
  # 加载特定的数据集，设置子集名称、分割类型和缓存目录
  ds = MsDataset.load(
      'AI-ModelScope/lowres_anime',  # 数据集名称
      subset_name='default',         # 子集名称
      split='train',                 # 数据分割类型
      cache_dir="/mnt/workspace/kolors/data"  # 数据缓存目录
  )
  
  # 导入所需的库
  import json, os
  from data_juicer.utils.mm_utils import SpecialTokens  # 导入数据处理工具
  from tqdm import tqdm  # 导入tqdm库，用于显示进度条
  
  # 创建存储训练数据和metadata的文件夹，如果已存在则不会重复创建
  os.makedirs("./data/lora_dataset/train", exist_ok=True)
  os.makedirs("./data/data-juicer/input", exist_ok=True)
  
  # 打开一个文件用于写入元数据，使用jsonl格式存储（每行一个JSON对象）
  with open("./data/data-juicer/input/metadata.jsonl", "w") as f:
      for data_id, data in enumerate(tqdm(ds)):  # 遍历数据集中的每个数据项，使用tqdm显示进度
          image = data["image"].convert("RGB")  # 获取图像数据并转换为RGB格式
          image.save(f"/mnt/workspace/kolors/data/lora_dataset/train/{data_id}.jpg")  # 将图像保存到指定路径
          
          # 构造元数据字典，包含文本描述和图像路径
          metadata = {
              "text": "二次元",  # 文本描述
              "image": [f"/mnt/workspace/kolors/data/lora_dataset/train/{data_id}.jpg"]  # 图像文件路径
          }
          f.write(json.dumps(metadata))  # 将元数据字典转换为JSON字符串并写入文件
          f.write("\n")  # 每个JSON对象后添加换行符，符合jsonl格式要求
  ```
  
- 数据处理

  ```python
  # 定义 data-juicer 的配置，指定处理参数和流程
  data_juicer_config = """
  # global parameters
  project_name: 'data-process'  # 项目名称
  dataset_path: './data/data-juicer/input/metadata.jsonl'  # 数据集的路径
  np: 4  # 使用的子进程数目，用于并行处理数据
  
  text_keys: 'text'  # 指定文本数据的键
  image_key: 'image'  # 指定图像数据的键
  image_special_token: '<__dj__image>'  # 特殊标记，用于图像处理
  
  export_path: './data/data-juicer/output/result.jsonl'  # 处理后数据的导出路径
  
  # process schedule
  # 定义数据处理操作列表
  process:
      - image_shape_filter:
          min_width: 1024
          min_height: 1024
          any_or_all: any  # 任意一个条件满足即可
      - image_aspect_ratio_filter:
          min_ratio: 0.5
          max_ratio: 2.0
          any_or_all: any  # 任意一个条件满足即可
  """
  
  # 将配置写入 YAML 文件
  with open("data/data-juicer/data_juicer_config.yaml", "w") as file:
      file.write(data_juicer_config.strip())
      
  # 使用 data-juicer 进行数据处理
  !dj-process --config data/data-juicer/data_juicer_config.yaml
  ```

  ```python
  # 读取处理后的数据并保存为 CSV 文件
  import pandas as pd
  import os, json
  from PIL import Image
  from tqdm import tqdm
  
  # 创建处理后数据存储目录
  texts, file_names = [], []
  os.makedirs("./data/lora_dataset_processed/train", exist_ok=True)
  
  # 读取处理后的数据
  with open("./data/data-juicer/output/result.jsonl", "r") as file:
      for data_id, data in enumerate(tqdm(file.readlines())):
          data = json.loads(data)  # 解析每行的 JSON 数据
          text = data["text"]  # 提取文本信息
          texts.append(text)
          image = Image.open(data["image"][0])  # 加载图像文件
          image_path = f"./data/lora_dataset_processed/train/{data_id}.jpg"
          image.save(image_path)  # 保存图像到新路径
          file_names.append(f"{data_id}.jpg")
  
  # 创建 DataFrame 来组织文本和文件名
  data_frame = pd.DataFrame()
  data_frame["file_name"] = file_names
  data_frame["text"] = texts
  # 保存 DataFrame 到 CSV 文件
  data_frame.to_csv("./data/lora_dataset_processed/train/metadata.csv", index=False, encoding="utf-8-sig")
  data_frame
  ```

- 模型训练

   - 下载预训练模型
   ```python
       from diffsynth import download_models
   
   # 下载指定的预训练模型，准备用于训练
   download_models(["Kolors", "SDXL-vae-fp16-fix"])
   ```
   - 查看训练脚本的输入参数
   
     ```python
     !python DiffSynth-Studio/examples/train/kolors/train_kolors_lora.py -h
     ```
   
     > **主要参数**
     >
     > 1. **--pretrained_unet_path**
     >    - 指定预训练的 UNet 模型的路径。这是一个用于生成图像的关键网络组件。
     > 2. **--pretrained_text_encoder_path**
     >    - 指定预训练的文本编码器模型的路径。这用于将文本输入转换为对应的编码，供模型理解和处理。
     > 3. **--pretrained_fp16_vae_path**
     >    - 指定预训练的变分自编码器（VAE）模型的路径。VAE 通常用于生成和优化图像质量。
     > 4. **--lora_target_modules**
     >    - 指定带有 LoRA 模块的网络层。LoRA 模块用于在保持大部分预训练权重不变的同时，通过低秩矩阵调整网络的特定部分。
     >
     > 数据处理和训练控制
     >
     > 1. **--dataset_path**
     >    - 数据集的路径，指明训练数据的存放位置。
     > 2. **--output_path**
     >    - 模型输出和保存的路径。
     > 3. **--steps_per_epoch**
     >    - 每个训练周期的步数。
     > 4. **--height** 和 **--width**
     >    - 输入图像的高度和宽度。
     > 5. **--center_crop**
     >    - 是否对输入图像进行中心裁剪。
     > 6. **--random_flip**
     >    - 是否随机水平翻转图像。
     > 7. **--batch_size**
     >    - 训练过程中每个批次的图像数量。
     > 8. **--dataloader_num_workers**
     >    - 用于数据加载的子进程数。
     >
     > 性能和优化
     >
     > 1. **--precision**
     >    - 训练的精度设置，如 32 位、16 位或混合精度。
     > 2. **--learning_rate**
     >    - 训练的学习率。
     > 3. **--lora_rank**
     >    - LoRA 更新矩阵的维度，控制 LoRA 模块的参数数量。
     > 4. **--lora_alpha**
     >    - LoRA 更新矩阵的权重，影响模型的调整幅度。
     > 5. **--use_gradient_checkpointing**
     >    - 是否使用梯度检查点，以优化内存使用。
     > 6. **--accumulate_grad_batches**
     >    - 梯度累积的批次数，有助于在内存受限的设备上训练更大的模型。
     > 7. **--training_strategy**
     >    - 训练策略，如使用 DeepSpeed 的不同阶段。
     > 8. **--max_epochs**
     >    - 训练的最大周期数。
     >
     > ModelScope 集成
     >
     > 1. **--modelscope_model_id** 和 **--modelscope_access_token**
     >    - 提供这些参数将使模型在训练完成后自动上传到 ModelScope 平台，其中 `model_id` 是模型在 ModelScope 的标识，`access_token` 是访问 ModelScope 所需的密钥。
     >
     
   - 配置并启动训练
   
     ```python
     import os
     
     # 构建训练命令，并设置相关参数
     cmd = """
     python DiffSynth-Studio/examples/train/kolors/train_kolors_lora.py \
       --pretrained_unet_path models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors \
       --pretrained_text_encoder_path models/kolors/Kolors/text_encoder \
       --pretrained_fp16_vae_path models/sdxl-vae-fp16-fix/diffusion_pytorch_model.safetensors \
       --lora_rank 16 \
       --lora_alpha 4.0 \
       --dataset_path data/lora_dataset_processed \
       --output_path ./models \
       --max_epochs 1 \
       --center_crop \
       --use_gradient_checkpointing \
       --precision "16-mixed"
     """.strip()
     
     # 使用 os.system 来执行训练命令
     os.system(cmd)
     ```
   
     >   **命令参数解释**：
     >
     > - `--pretrained_unet_path`：指定预训练的 UNet 模型文件路径。
     > - `--pretrained_text_encoder_path`：指定预训练的文本编码器模型文件路径。
     > - `--pretrained_fp16_vae_path`：指定预训练的 VAE 模型文件路径，使用 FP16 精度。
     > - `--lora_rank`：设置 LoRA 模块的秩，影响模型参数和复杂性。
     > - `--lora_alpha`：设置 LoRA 模块的 alpha 值，调节更新权重的影响程度。
     > - `--dataset_path`：指定用于训练的数据集的路径。
     > - `--output_path`：定义模型训练完成后保存的路径。
     > - `--max_epochs`：设置训练的最大周期数，这里设置为 1，适用于快速测试。
     > - `--center_crop`：是否对训练图像进行中心裁剪。
     > - `--use_gradient_checkpointing`：是否使用梯度检查点来节省内存。
     > - `--precision`：设置训练的数值精度，这里选择 "16-mixed" 表示使用混合精度训练，有助于节省内存和加速训练。
     >
   
   - 加载和应用 LoRA 适配器
   
     ```python
     from diffsynth import ModelManager, SDXLImagePipeline
     from peft import LoraConfig, inject_adapter_in_model
     import torch
     
     # 定义加载 LoRA 适配器的函数
     def load_lora(model, lora_rank, lora_alpha, lora_path):
         lora_config = LoraConfig(
             r=lora_rank,
             lora_alpha=lora_alpha,
             init_lora_weights="gaussian",
             target_modules=["to_q", "to_k", "to_v", "to_out"],
         )
         model = inject_adapter_in_model(lora_config, model)
         state_dict = torch.load(lora_path, map_location="cpu")
         model.load_state_dict(state_dict, strict=False)
         return model
     
     # 加载模型管理器和图像处理管道
     model_manager = ModelManager(torch_dtype=torch.float16, device="cuda",
                                  file_path_list=[
                                      "models/kolors/Kolors/text_encoder",
                                      "models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors",
                                      "models/kolors/Kolors/vae/diffusion_pytorch_model.safetensors"
                                  ])
     pipe = SDXLImagePipeline.from_model_manager(model_manager)
     
     # 应用 LoRA 并加载训练的状态
     pipe.unet = load_lora(
         pipe.unet,
         lora_rank=16, # 这个参数应与训练脚本中的一致。
         lora_alpha=2.0, # lora_alpha 控制 LoRA 的权重。
         lora_path="models/lightning_logs/version_0/checkpoints/epoch=0-step=500.ckpt"
     )
     ```
   
   - 生成图像
   
     ```python
     torch.manual_seed(0)
     image = pipe(
         prompt="二次元，一个紫色短发小女孩，在家中沙发上坐着，双手托着腮，很无聊，全身，粉色连衣裙",
         negative_prompt="丑陋、变形、嘈杂、模糊、低对比度",
         cfg_scale=4,
         num_inference_steps=50, height=1024, width=1024,
     )
     image.save("1.jpg")
     
     ..................
     ```
     
     > - **设置随机种子**：通过 `torch.manual_seed(seed)` 设置随机数生成器的种子，以确保结果的可重现性。每次生成图像前都设置一个新的种子，使得每张生成的图像都有一定的随机性，但又保持相对一致的风格。
     > - **生成图像**：使用 `pipe` 函数来生成图像。这里的 `pipe` 函数可能是一个预训练的模型管道，用于根据文本描述生成图像。该函数接受几个参数：
     >   - `prompt`：正向提示，描述想要生成的图像内容。
     >   - `negative_prompt`：负向提示，描述应避免的图像特征。
     >   - `cfg_scale`：控制生成过程中创造性和确定性的比例。
     >   - `num_inference_steps`：生成图像时的步骤数量，步骤越多，图像细节可能越丰富。
     >   - `height` 和 `width`：生成图像的尺寸。
     > - **保存图像**：使用 `image.save("filename.jpg")` 保存生成的图像到本地文件。
     >
     
   - 图像合成与输出
   
     ```python
     import numpy as np
     from PIL import Image
     
     # 读取所有生成的图像，转换为numpy数组
     images = [np.array(Image.open(f"{i}.jpg")) for i in range(1, 9)]
     
     # 将图像按照4x2的布局合并成一张大图
     image = np.concatenate([
         np.concatenate(images[0:2], axis=1),
         np.concatenate(images[2:4], axis=1),
         np.concatenate(images[4:6], axis=1),
         np.concatenate(images[6:8], axis=1),
     ], axis=0)
     
     # 将合并后的图像转换回PIL图像，并调整大小
     image = Image.fromarray(image).resize((1024, 2048))
     image.show()  # 显示图像
     ```
   
     

##### 2. baseline运行结果

   ![output](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/AIGC%E5%9B%BE%E5%83%8F%E7%94%9F%E6%88%90/images/baseline_output.png)

### 四、baseline 优化

##### 1. Lora 微调

```python
import os

cmd = """
python DiffSynth-Studio/examples/train/kolors/train_kolors_lora.py \
  --pretrained_unet_path models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors \
  --pretrained_text_encoder_path models/kolors/Kolors/text_encoder \
  --pretrained_fp16_vae_path models/sdxl-vae-fp16-fix/diffusion_pytorch_model.safetensors \
  --lora_rank 16 \
  --lora_alpha 4.0 \
  --dataset_path data/lora_dataset_processed \
  --output_path ./models \
  --max_epochs 10 \
  --center_crop \
  --learning_rate 0.0001 \
  --dataloader_num_workers 8 \
  --use_gradient_checkpointing \
  --precision "16-mixed"
""".strip()

os.system(cmd)
```

> - 修改**lora_rank**：LoRA（Low-Rank Adaptation）的秩，它是一种微调技术，用于调整模型的特定层，而不改变整个模型的结构。
> - 增加**max_epochs**：训练的最大周期数。
> - 增加**learning_rate**: 学习率，控制模型在每次迭代中更新的幅度。是深度学习中最重要的超参数之一。
> - 修改**dataloader_num_workers**：用于数据加载的子进程数。

##### 2. 导入模型

```python
from diffsynth import ModelManager, SDXLImagePipeline
from peft import LoraConfig, inject_adapter_in_model
import torch

def load_lora(model, lora_rank, lora_alpha, lora_path):
    lora_config = LoraConfig(
        r=lora_rank,
        lora_alpha=lora_alpha,
        init_lora_weights="gaussian",
        target_modules=["to_q", "to_k", "to_v", "to_out"],
    )
    model = inject_adapter_in_model(lora_config, model)
    state_dict = torch.load(lora_path, map_location="cpu")
    model.load_state_dict(state_dict, strict=False)
    return model

# Load models
model_manager = ModelManager(torch_dtype=torch.float16, device="cuda",
                             file_path_list=[
                                 "models/kolors/Kolors/text_encoder",
                                 "models/kolors/Kolors/unet/diffusion_pytorch_model.safetensors",
                                 "models/kolors/Kolors/vae/diffusion_pytorch_model.safetensors"
                             ])
pipe = SDXLImagePipeline.from_model_manager(model_manager)

# Load LoRA
pipe.unet = load_lora(
    pipe.unet,
    lora_rank=16, # 这个参数应与训练脚本中的一致。
    lora_alpha=2.0, # lora_alpha 控制 LoRA 的权重。
    lora_path="models/lightning_logs/version_4/checkpoints/epoch=9-step=5000.ckpt"
)
```

> - 修改 ***lora_rank***， *# 这个参数应与训练脚本中的一致。*
> - 修改 ***lora_path***="models/lightning_logs/version_4/checkpoints/epoch=9-step=5000.ckpt"
>

##### 3. 优化后的图片输出

![output](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/AIGC%E5%9B%BE%E5%83%8F%E7%94%9F%E6%88%90/images/baseline1_output.png)
