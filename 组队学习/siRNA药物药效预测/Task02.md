<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>Datawhale AI 夏令营——siRNA药物药效预测<br/><span>Task2：baseline分析 & RNN与特征工程入门</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录时间：2024年7月29日；最后更新时间：2024年7月31日
</div>


## 一、学习官方的baseline

本次基于 [Task01_baseline.ipynb](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/siRNA%E8%8D%AF%E7%89%A9%E8%8D%AF%E6%95%88%E9%A2%84%E6%B5%8B/Task01_baseline.ipynb) 分为 `10个模块` 进行学习。

#### 1. 依赖库的导入与随机种子的设定，确保实验可重复

```python
# 依赖库的导入，这些库包括了文件操作、深度学习、数据处理、模型评估等必要的工具。
import os  # 文件操作
import torch  # 深度学习框架
import random  # 随机数生成
import numpy as np  # 数值计算
import pandas as pd  # 数据处理

import torch.nn as nn  # 神经网络模块
import torch.optim as optim  # 优化器模块

from tqdm import tqdm  # 进度条显示
from rich import print  # 美化打印输出
from collections import Counter  # 计数器工具

from torch.utils.data import Dataset, DataLoader  # 数据集和数据加载器
from sklearn.model_selection import train_test_split  # 数据集划分
from sklearn.metrics import precision_score, recall_score, mean_absolute_error  # 模型评估指标
```

```python
# 随机种子的设定
# 该函数确保了在使用NumPy、Python内置随机数生成器和PyTorch时，所有的随机数生成都是可控的和可复现的，有助于实验结果的一致性，保持结果的一致性对于实验的可重复性和结果分析至关重要。
def set_random_seed(seed):
    # 设置NumPy的随机种子。NumPy是Python中广泛使用的一个库，提供了大量的数学函数工具，包括随机数生成。通过设置相同的种子，可以确保每次调用NumPy的随机函数时，生成的随机数序列都是相同的。
    np.random.seed(seed)
    # 设置Python内置的随机数生成器的种子
    random.seed(seed)
    # 设置PyTorch在CPU上生成的随机种子
    torch.manual_seed(seed)
    # 设置CUDA的随机种子
    torch.cuda.manual_seed(seed)
    # 设置所有CUDA设备的随机种子。如果你的系统中有多个CUDA设备（例如多个GPU），这行代码将确保所有设备上的随机数生成器都使用相同的种子。这对于在多GPU环境中进行分布式训练时保持结果一致性非常关键。
    torch.cuda.manual_seed_all(seed)
    # 确保每次卷积算法选择都是确定的。但这通常会导致性能下降，因为为了确定性，CuDNN可能会选择不是最优的算法。
    torch.backends.cudnn.deterministic = True
    # 关闭CuDNN自动优化功能，确保结果可复现。默认情况下，CuDNN会尝试为其操作选择最快的算法。然而，这种优化可能会导致每次运行时选择的算法不同，从而影响结果的可重复性。
    torch.backends.cudnn.benchmark = False
```

#### 2. 基因组分词器类

```python
class GenomicTokenizer:
    def __init__(self, ngram=5, stride=2):
        # 初始化分词器，设置n-gram长度和stride步幅
        self.ngram = ngram
        self.stride = stride
        
    def tokenize(self, t):
        # 将输入序列转换为大写
        t = t.upper()
        
        if self.ngram == 1:
            # 如果n-gram长度为1，直接将序列转换为字符列表
            toks = list(t)
        else:
            # 否则，按照步幅对序列进行n-gram分词
            toks = [t[i:i+self.ngram] for i in range(0, len(t), self.stride) if len(t[i:i+self.ngram]) == self.ngram]
        
        # 如果最后一个分词长度小于n-gram，移除最后一个分词
        if len(toks[-1]) < self.ngram:
            toks = toks[:-1]
        
        # 返回分词结果
        return toks
```

> 1. 类初始化：`__init__` 方法接受两个参数 `ngram` 和 `stride`，用于设置分词器的 n-gram 长度和步幅。
> 2. 分词方法：`tokenize` 方法将输入的序列转换为大写，换为大写有助于处理一致性和减少潜在的错误。然后根据设置的 `ngram` 和 `stride` 对序列进行分词。
> 3. `n-gram` 长度为 1 的处理：如果 `ngram` 为 1，意味着每个字符都是一个独立的分词，就直接将序列转换为字符列表。
> 4. `n-gram` 长度大于 1 的处理：按 `stride`步幅进行分词，并确保每个分词的长度等于 `ngram`，避免因序列长度不足而生成不完整的n-gram。
> 5. 最后一个分词的处理：如果最后一个分词长度小于 `ngram`，将其移除。这通常发生在序列长度不是`stride`的整数倍且最后一个n-gram不完整的情况。
> 6. 返回分词结果：返回一个包含所有有效n-gram分词的列表。

> ```python
> # 列表推导式
> toks = [t[i:i+self.ngram] for i in range(0, len(t), self.stride) if len(t[i:i+self.ngram]) == self.ngram]
> ```
>
> 此代码以初学者的角度可以改写为：
>
> ```python
>     toks = []  # 初始化分词列表
> 	    
>     for i in range(0, len(t), self.stride):  
>         # 检查是否可以从索引i开始截取一个完整的ngram  
>         if i + self.ngram <= len(t):  
>             tok = t[i:i+self.ngram]  # 截取ngram  
>             toks.append(tok)  # 添加到分词列表中
> 
> ```

#### 3. 基因组词汇类

```python
class GenomicVocab:
    def __init__(self, itos):
        # 初始化词汇表，itos是一个词汇表列表
        self.itos = itos
        # 创建从词汇到索引的映射
        self.stoi = {v: k for k, v in enumerate(self.itos)}
        
    @classmethod
    def create(cls, tokens, max_vocab, min_freq):
        # 创建词汇表类方法
        # 统计每个token出现的频率
        freq = Counter(tokens)
        # 选择出现频率大于等于min_freq的token，并且最多保留max_vocab个token
        itos = ['<pad>'] + [o for o, c in freq.most_common(max_vocab - 1) if c >= min_freq]
        # 返回包含词汇表的类实例
        return cls(itos)
```

> 1.  `__init__`方法是类的初始化方法，当创建类的新实例时自动调用。它接受一个参数`itos`，这是一个包含词汇的列表（从词汇到字符串的映射）。然后，它创建了一个名为`stoi`的字典，该字典通过枚举`itos`列表中的元素来建立从词汇（字符串）到索引（整数）的映射。
> 2.  `create`是一个类方法，它通过`@classmethod`装饰器定义。这意味着该方法在调用时接收类本身（`cls`）作为第一个参数，而不是类的实例。它用于根据给定的参数（`tokens`、`max_vocab`、`min_freq`）动态创建一个`GenomicVocab`类的实例。
>     - `tokens`：所有token的列表。
>     - `max_vocab`：词汇表中允许的最大词汇数（包括特殊词汇如`<pad>`）。
>     - `min_freq`：词汇表中词汇的最小出现频率。
> 3.  使用`collections.Counter`（已经导入`from collections import Counter`）来统计`tokens`中每个词汇的出现频率。
> 4.  创建一个包含特殊词汇`<pad>`的列表`itos`。然后，使用列表推导式从`freq.most_common(max_vocab - 1)`（返回按频率降序排列的前`max_vocab - 1`个词汇及其计数）中选择出现频率大于等于`min_freq`的词汇。这里使用`max_vocab - 1`是因为手动添加了`<pad>`词汇，所以只能从词汇中选取`max_vocab - 1`个。
> 5.  使用选定的词汇列表`itos`调用类的构造函数（`__init__`方法），并返回新创建的`GenomicVocab`实例。这样，就根据给定的`tokens`、`max_vocab`和`min_freq`参数动态构建了一个词汇表。
> 6.  `<pad>`作用：在深度学习模型中通常被表示为一个特殊的标记，这个标记的向量表示通常是全零向量，表示它不携带任何有用的信息。在训练和推理时，模型会忽略`<pad>`，只关注包含有用信息的部分。在输入序列的末尾或特定位置添加`<pad>`，模型可以更清晰地理解序列的边界和结构，从而在处理时更准确地捕捉上下文信息。在上面的代码中，`<pad>`被直接添加到了`itos`列表的开头。这意味着在词汇到索引的映射（`stoi`字典）中，`<pad>`将被赋予索引0（或第一个索引，具体取决于枚举的起始值，但Python的`enumerate`默认从0开始）。这种安排是有意为之的，因为它使得在后续处理中，当需要填充序列时，可以直接使用索引0来引用`<pad>`词汇。
>

#### 4. siRNA数据集类

```python
class SiRNADataset(Dataset):
    def __init__(self, df, columns, vocab, tokenizer, max_len, is_test=False):
        # 初始化数据集
        self.df = df  # 数据框
        self.columns = columns  # 包含序列的列名
        self.vocab = vocab  # 词汇表
        self.tokenizer = tokenizer  # 分词器
        self.max_len = max_len  # 最大序列长度
        self.is_test = is_test  # 指示是否是测试集

    def __len__(self):
        # 返回数据集中样本的总数，即df的行数。
        return len(self.df)

    def __getitem__(self, idx):
        # 获取数据集中的第idx个样本
        row = self.df.iloc[idx]  # 获取第idx行数据
        
        # 对每一列进行分词和编码
        seqs = [self.tokenize_and_encode(row[col]) for col in self.columns]
        if self.is_test:
            # 仅返回编码后的序列（非测试集模式）
            return seqs
        else:
            # 获取目标值并转换为张量（仅在非测试集模式下）
            target = torch.tensor(row['mRNA_remaining_pct'], dtype=torch.float)
            # 返回编码后的序列和目标值
            return seqs, target

    def tokenize_and_encode(self, seq):
        if ' ' in seq:  # 修改过的序列
            tokens = seq.split()  # 按空格分词
        else:  # 常规序列
            tokens = self.tokenizer.tokenize(seq)  # 使用分词器分词
        
        # 将token转换为索引，未知token使用0（<pad>）
        encoded = [self.vocab.stoi.get(token, 0) for token in tokens]
        # 将序列填充到最大长度
        padded = encoded + [0] * (self.max_len - len(encoded))
        # 返回张量格式的序列
        return torch.tensor(padded[:self.max_len], dtype=torch.long)
```

> - `self.df = df`：存储数据集的DataFrame。DataFrame是pandas库中的一个数据结构，用于以表格形式存储和操作结构化数据。
> - `self.columns = columns`：包含要处理序列的列名列表，这些列中的每个元素都将被分词和编码。
> - `self.vocab = vocab`：存储词汇表，通常是一个映射，将单词或token映射到唯一的整数ID。
> - `self.tokenizer = tokenizer`：分词器对象，用于将原始文本序列分割为token序列。
> - `self.max_len = max_len`：每个序列的最大长度。较短的序列将被填充以达到这个长度，而较长的序列可能会被截断。
> - `self.is_test = is_test`：一个布尔值，指示这个数据集是否用于测试。如果是测试集，则不需要返回目标值。
> - `row = self.df.iloc[idx]`：通过索引`idx`获取DataFrame中的一行数据。
> - 使用列表推导式，对`self.columns`中的每一列进行分词和编码。`tokenize_and_encode`函数用于处理这一过程。
> - 如果`self.is_test`为True（即这是测试集，默认为False），则仅返回编码后的序列列表`seqs`。
> - 如果不是测试集，则还需要获取目标值（`'mRNA_remaining_pct'`列的值），将其转换为PyTorch张量，并连同编码后的序列一起返回。
>
> - 检查给定的序列`seq`中是否包含空格。如果包含，则按空格分词；如果不包含，则使用分词器`self.tokenizer`进行分词。
> - 将分词后的token列表转换为索引列表。如果某个token不在词汇表`self.vocab`中，则使用索引0（通常代表`<pad>`或未知token）。
> - 将索引列表填充到最大长度`self.max_len`。如果列表长度小于`self.max_len`，则在末尾添加足够数量的0（代表填充）。
> - 返回填充后且长度不超过`self.max_len`的PyTorch张量，数据类型为`torch.long`。

#### 5. siRNA Model

```python
class SiRNAModel(nn.Module):
    # 这是类的初始化函数（构造函数），它接收几个参数：
    # vocab_size：词汇表的大小，即嵌入层需要学习的词向量的数量。
    # embed_dim：嵌入层输出的词向量的维度，设置为200。
    # hidden_dim：GRU层中隐藏状态的维度，设置为256。
    # n_layers：GRU层的层数，设置为3。
    # dropout：Dropout比率，用于防止过拟合，设置为0.5。
    def __init__(self, vocab_size, embed_dim=200, hidden_dim=256, n_layers=3, dropout=0.5):
        # 调用父类nn.Module的构造函数，这是所有PyTorch模型定义时的常规做法。
        super(SiRNAModel, self).__init__()
        
        # 初始化嵌入层，用于将词汇表中的每个单词（或token）映射到一个固定大小的密集向量（即嵌入向量）。vocab_size是词汇表的大小，embed_dim是嵌入向量的维度。padding_idx=0指定了填充索引为0，这意味着词汇表中的第一个单词（通常是<pad>）将被用作填充词，并且其嵌入向量将被忽略或特殊处理。
        self.embedding = nn.Embedding(vocab_size, embed_dim, padding_idx=0)
        
        # 初始化GRU层，用于处理序列数据。embed_dim是输入特征的大小（与嵌入向量的维度相同），hidden_dim是隐藏层的维度，n_layers是GRU层的数量。bidirectional=True表示数据会同时从前往后和从后往前处理，batch_first=True表示输入张量的第一个维度是批量大小，dropout=dropout在每个RNN层后应用dropout（Dropout层的丢弃率），以减少过拟合。
        # self.gru = nn.GRU(embed_dim, hidden_dim, n_layers, bidirectional=True, batch_first=True, dropout=dropout if n_layers > 1 else 0)
        self.gru = nn.GRU(embed_dim, hidden_dim, n_layers, bidirectional=True, batch_first=True, dropout=dropout)
        
        # 初始化全连接层，用于将GRU层的输出映射到最终的预测结果。由于GRU是双向的，且可能有多层，因此最终的隐藏状态大小是 hidden_dim * 2 * n_layers（有n_layers层）。在实际情况中，应根据GRU层的配置来调整这个参数。官方给出的是 hidden_dim * 4。因为x的每个特征为256*2，而x有两层，所以256*2*2。
        # self.fc = nn.Linear(hidden_dim * 2 * n_layers, 1)
        self.fc = nn.Linear(hidden_dim * 4, 1) 
        
        # 初始化Dropout层，用于在全连接层之前减少过拟合。dropout是dropout率，表示在训练过程中随机将输入张量中的一部分元素置零的比例。
        self.dropout = nn.Dropout(dropout)
    
    # 前向传播函数 forward，定义了数据如何通过网络流动。
    def forward(self, x):
        # 将输入序列x（本代码中的x是序列的列表，其中每个序列都是词汇索引的列表）传入嵌入层，通过嵌入层转换为嵌入向量的列表。
        embedded = [self.embedding(seq) for seq in x]
        # 初始化一个空列表outputs，用于存储每个序列经过GRU处理后的输出。
        outputs = []
        
        # 对每个嵌入的序列进行处理
        for embed in embedded:
            x, _ = self.gru(embed)  # 传入GRU层，x 是输出序列，_ 是隐藏状态
            x = self.dropout(x[:, -1, :])  # 对每个序列的最后一个隐藏状态应用dropout处理。
            outputs.append(x)
        
        # 将所有序列的输出拼接起来。将所有序列的最后一个隐藏状态拼接起来，形成一个新的张量x。
        x = torch.cat(outputs, dim=1)
        # 将拼接后的张量x通过全连接层self.fc，得到最终的预测结果。
        x = self.fc(x)
        # 使用squeeze()方法去除张量中维度为1的条目，以简化输出形状。
        return x.squeeze()
```

> - 隐藏状态：
>   - 在循环神经网络（RNN）及其变体（如长短时记忆网络LSTM、门控循环单元GRU等）中是一个核心概念。它指的是在序列处理过程中，网络内部用于存储和传递序列信息的一种状态表示。在RNN中，每个时间步都会接收一个输入，并基于当前的输入和前一个时间步的隐藏状态来计算当前时间步的输出和新的隐藏状态。这个新的隐藏状态随后会被传递到下一个时间步，用于计算下一个时间步的输出和隐藏状态，以此类推，直到处理完整个序列。即RNN的隐藏状态将帮助我们捕获文本序列中的上下文信息。
>
>   - 假设我们的输入序列是“I love to eat”，并且我们想要预测下一个单词。这个序列将被逐个单词地输入到RNN中，每个单词都被转换为一个固定大小的向量（通常是通过词嵌入层实现的）。
>
>   - 在处理序列之前，RNN的隐藏状态会被初始化为一个零向量或预先设定的值。这个初始隐藏状态不包含任何关于序列的信息。当单词“I”被输入时，RNN会结合当前的输入向量（表示“I”的词向量）和初始隐藏状态来计算第一个隐藏状态。这个隐藏状态现在包含了关于“I”的信息。接着，单词“love”被输入。RNN现在会结合“love”的词向量和上一个时间步的隐藏状态（即包含“I”信息的隐藏状态）来计算新的隐藏状态。这个新的隐藏状态现在包含了关于“I love”的信息。这个过程会继续进行，直到处理完整个序列“I love to eat”。在每个时间步，RNN都会更新其隐藏状态，以包含到目前为止序列中的所有信息。最后，RNN的隐藏状态会被用作生成输出的依据。输出层通常会基于当前的隐藏状态来计算一个概率分布，这个分布表示了序列中下一个单词可能是哪些单词的概率。
>

 

#### 6. 评估指标计算函数

```python
# 定义了一个名为calculate_metrics 函数，它接受三个参数：y_true（真实值）、y_pred（预测值），以及一个可选的阈值 threshold，本次阈值设置为30。
def calculate_metrics(y_true, y_pred, threshold=30):
    # 计算平均绝对误差
    mae = np.mean(np.abs(y_true - y_pred))

    # 将实际值和预测值转换为二进制分类（低于阈值为1，高于或等于阈值为0）
    y_true_binary = (y_true < threshold).astype(int)
    y_pred_binary = (y_pred < threshold).astype(int)

    # 创建一个布尔型掩码，用于筛选预测值在0和阈值之间的样本
    mask = (y_pred >= 0) & (y_pred <= threshold)
    # 计算在掩码选定范围内的平均绝对误差，如果没有任何样本在选定范围内，则将range_mae设为一个较大的值，本次设置值为100  
    range_mae = mean_absolute_error(y_true[mask], y_pred[mask]) if mask.sum() > 0 else 100

    # 计算精确度、召回率和F1得分
    precision = precision_score(y_true_binary, y_pred_binary, average='binary')
    recall = recall_score(y_true_binary, y_pred_binary, average='binary')
    f1 = 2 * precision * recall / (precision + recall)

    # 计算综合评分，此评分为官方给出的评分机制
    score = (1 - mae / 100) * 0.5 + (1 - range_mae / 100) * f1 * 0.5

    return score
```



#### 7. 模型评估函数

```python
# 定义了一个名为evaluate_model的函数，它接受三个参数：model（要评估的模型），test_loader（包含测试数据的加载器），以及device（模型和数据将运行的设备，默认为'cuda'，即GPU）。
def evaluate_model(model, test_loader, device='cuda'):
    # 在PyTorch中，模型有两种主要的工作模式：训练模式（train）和评估模式（eval）。训练模式为默认状态。
    # 设置模型为评估模式，通常会关闭特定于训练的模块，如Dropout，因为在评估时，是希望模型使用所有可用的神经元来做出预测。
    model.eval()
    # 两个列表将分别用于存储模型的预测结果和测试数据集的真实目标值。
    predictions = []
    targets = []
    
    # 禁用梯度计算。在评估模型时，不需要计算梯度，因为不会进行反向传播。使用torch.no_grad()上下文管理器可以节省内存并提高计算效率。
    with torch.no_grad():
        # 遍历测试数据加载器中的每个批次，每个批次包含输入数据inputs和对应的目标值target。
        for inputs, target in test_loader:
            # 将输入数据移动到指定设备上
            inputs = [x.to(device) for x in inputs]
            # 获取模型的输出
            outputs = model(inputs)
            # 将预测结果从GPU移到CPU，并转换为numpy数组，添加到predictions列表中
            predictions.extend(outputs.cpu().numpy())
            # 将目标值转换为numpy数组，添加到targets列表中
            targets.extend(target.numpy())

    # 将预测结果和目标值转换为numpy数组
    y_pred = np.array(predictions)
    y_test = np.array(targets)
    
    # 计算评估指标
    score = calculate_metrics(y_test, y_pred)
    # 打印测试得分,保留4位小数
    print(f"Test Score: {score:.4f}")
```



#### 8. 模型训练函数

```python
def train_model(model, train_loader, val_loader, criterion, optimizer, num_epochs=50, device='cuda', output_dir: str=""):
    # 将模型移动到指定设备
    model.to(device)
    best_score = -float('inf')  # 初始化最佳得分为负无穷大
    best_model = None  # 初始化最佳模型
	# 设置循环，遍历指定的训练轮次。
    for epoch in range(num_epochs):
        model.train()  # 设置模型为训练模式
        train_loss = 0  # 初始化训练损失
        # 遍历训练数据加载器
        # 代码使用tqdm包装了train_loader的迭代过程，为训练过程添加了一个进度条。
        # desc参数用于设置进度条的描述，这里显示为当前是第几个epoch，以及总共有多少个epoch。
        # train_loader是一个数据加载器，它按照批次（batch）的方式提供训练数据。每次迭代，它都会返回一批输入数据（inputs）和对应的目标值（targets）。此循环一次batch有64个数据，也就是bs=64
        for inputs, targets in tqdm(train_loader, desc=f'Epoch {epoch+1}/{num_epochs}'):
            inputs = [x.to(device) for x in inputs]  # 将输入移动到设备
            targets = targets.to(device)  # 将目标值移动到设备
            
            optimizer.zero_grad()  # 清空梯度，在进行反向传播之前，需要清空之前累积的梯度。这是因为PyTorch会默认累加梯度，如果不清空，那么梯度将会是之前所有批次的累加，导致参数更新不正确。
            outputs = model(inputs)  # 前向传播，此代码将输入数据（inputs）传递给模型（model），并获取模型的输出（outputs）。
            loss = criterion(outputs, targets)  # 计算损失
            loss.backward()  # 反向传播
            optimizer.step()  # 更新参数
            
            train_loss += loss.item()  # 累加训练损失
        
        model.eval()  # 设置模型为评估模式
        val_loss = 0  # 初始化验证损失
        val_preds = []
        val_targets = []
		
        # 禁用梯度计算
        with torch.no_grad():
            # 遍历验证集
            for inputs, targets in val_loader:
                inputs = [x.to(device) for x in inputs]  # 将输入移动到设备
                targets = targets.to(device)  # 将目标值移动到设备
                outputs = model(inputs)  # 前向传播
                loss = criterion(outputs, targets)  # 计算损失
                val_loss += loss.item()  # 累加验证损失
                val_preds.extend(outputs.cpu().numpy())  # 收集预测值
                val_targets.extend(targets.cpu().numpy())  # 收集目标值
        
        train_loss /= len(train_loader)  # 计算平均训练损失
        val_loss /= len(val_loader)  # 计算平均验证损失
        
        # 转换预测值和目标值为NumPy数组
        val_preds = np.array(val_preds)
        val_targets = np.array(val_targets)
        score = calculate_metrics(val_targets, val_preds)  # 计算验证集上的得分
        
        print(f'Epoch {epoch+1}/{num_epochs}')
        print(f'Train Loss: {train_loss:.4f}, Val Loss: {val_loss:.4f}')
        print(f'Learning Rate: {optimizer.param_groups[0]["lr"]:.6f}')
        print(f'Validation Score: {score:.4f}')

        if score > best_score:
            best_score = score  # 更新最佳得分
            best_model = model.state_dict().copy()  # 更新最佳模型
            torch.save(model.state_dict(), os.path.join(output_dir, "best.pt".format(epoch)))  # 保存最佳模型
            print(f'New best model found with score: {best_score:.4f}')

    return best_model  # 返回最佳模型
```

> ```python
> 查询train_loader的输入尺寸
> for inputs, target in train_loader:
> 	print(len(inputs))
> 	print(inputs[0].shape)
> 	print(inputs[0] [0])
> 	print(target.shape)
> 	break
> 输出：
> 2
> 
> torch.Size([64, 25]) #inputs的张量维度。64为batch的大小，25为序列的长度
> 
> tensor([39, 28, 54, 61, 31, 46, 40, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]) # 第一次循环的数据
> 
> torch.Size([64])

#### 9. 训练主程序

```python
# 设置参数，这些参数在后期可以寻找其更合适的值
bs = 64    # 批次大小，即每次训练时输入到模型的数据量。
epochs = 50    # 训练的迭代次数，即整个数据集将被遍历的次数。
lr = 0.001    # 学习率，控制模型参数更新的步长。
seed = 42    # 随机种子，用于确保实验的可重复性。
output_dir = "output/models"    # 模型保存路径

# 选择设备，选择使用'cuda'还是'cpu'进行模型训练。
device = 'cuda' if torch.cuda.is_available() else 'cpu'

# 设置随机种子以确保结果可重复
set_random_seed(seed)

# 创建输出目录（os模块处理）
if not os.path.exists(output_dir):
    os.makedirs(output_dir)

# 加载数据（pandas模块读取）
train_data = pd.read_csv('train_data.csv')

# 指定需要处理的列
columns = ['siRNA_antisense_seq', 'modified_siRNA_antisense_seq_list']
# 删除包含空值的行，只考虑指定的列和另一个未列出的列 'mRNA_remaining_pct'
train_data.dropna(subset=columns + ['mRNA_remaining_pct'], inplace=True)
# 使用 sklearn.model_selection 以进行数据分割  
# 将数据分为训练集和验证集，验证集占总数据的10% 
train_data, val_data = train_test_split(train_data, test_size=0.1, random_state=42)

# 创建分词器，ngram=3表示使用3-gram，stride=3表示步长为3。因为1个密码子由3个碱基组成。
tokenizer = GenomicTokenizer(ngram=3, stride=3)

# 创建词汇表
all_tokens = []
for col in columns:
    for seq in train_data[col]:
        if ' ' in seq:  # 修改过的序列，即序列已经被分词
            all_tokens.extend(seq.split())
        else:
            all_tokens.extend(tokenizer.tokenize(seq))
vocab = GenomicVocab.create(all_tokens, max_vocab=10000, min_freq=1)

# 找到最大序列长度
max_len = max(max(len(seq.split()) if ' ' in seq else len(tokenizer.tokenize(seq)) 
                    for seq in train_data[col]) for col in columns)

# 创建数据集
train_dataset = SiRNADataset(train_data, columns, vocab, tokenizer, max_len)
val_dataset = SiRNADataset(val_data, columns, vocab, tokenizer, max_len)

# 创建数据加载器
train_loader = DataLoader(train_dataset, batch_size=bs, shuffle=True)
val_loader = DataLoader(val_dataset, batch_size=bs)

# 初始化模型
model = SiRNAModel(len(vocab.itos))
criterion = nn.MSELoss()

# 初始化优化器
optimizer = optim.Adam(model.parameters(), lr=lr)

# 训练模型
best_model = train_model(model, train_loader, val_loader, criterion, optimizer, epochs, device, output_dir=output_dir)
```



#### 10. 测试程序

```python
# 设置输出目录
output_dir = "result"
if not os.path.exists(output_dir):
    os.makedirs(output_dir)

# 加载测试数据
test_data = pd.read_csv('sample_submission.csv')
columns = ['siRNA_antisense_seq', 'modified_siRNA_antisense_seq_list']
test_data.dropna(subset=columns, inplace=True)  # 删除含有空值的行

# 创建分词器
tokenizer = GenomicTokenizer(ngram=3, stride=3)

# 创建词汇表
all_tokens = []
for col in columns:
    for seq in test_data[col]:
        if ' ' in seq:  # 修改过的序列
            all_tokens.extend(seq.split())
        else:
            all_tokens.extend(tokenizer.tokenize(seq))
            
vocab = GenomicVocab.create(all_tokens, max_vocab=10000, min_freq=1)

# 找到最大序列长度
max_len = max(max(len(seq.split()) if ' ' in seq else len(tokenizer.tokenize(seq)) 
                    for seq in test_data[col]) for col in columns)

# 创建测试数据集
test_dataset = SiRNADataset(test_data, columns, vocab, tokenizer, max_len, is_test=True)

# 创建数据加载器
test_loader = DataLoader(test_dataset, batch_size=64, shuffle=False)

# 初始化模型
model = SiRNAModel(len(vocab.itos))
model.load_state_dict(best_model)  # 加载最佳模型权重
model.to(device=device)
model.eval()  # 切换到评估模式，这对于某些模块如Dropout和BatchNorm是必需的

# 进行预测
preds = []
with torch.no_grad():
    for inputs in tqdm(test_loader):
        # import pdb;pdb.set_trace()
        inputs = [x.to(device) for x in inputs]
        outputs = model(inputs)
        preds.extend(outputs.cpu().numpy())

# 将预测结果添加到测试数据中
test_data["mRNA_remaining_pct"] = preds
df = pd.DataFrame(test_data)

# 保存预测结果
output_csv = os.path.join(output_dir, "submission.csv")
print(f"submission.csv 保存在 {output_csv}")
df.to_csv(output_csv, index=False)
```



## 二、提分思路

#### 1. RNN

RNN，全称为递归神经网络，是一种人工智能模型，特别擅长处理序列数据。它和普通的神经网络不同，因为它能够记住以前的数据，并利用这些记忆来处理当前的数据。但RNN也有其局限性，其难以记住和利用很久以前的信息。这是因为在长序列中，随着时间步的增加，早期的信息会逐渐被后来的信息覆盖或淡化。

#### 2. LSTM

LSTM 通过引入一个复杂的单元结构来解决 RNN 的局限性。LSTM 单元包含三个门（输入门、遗忘门和输出门）和一个记忆单元（细胞状态），这些门和状态共同作用，使 LSTM 能够更好地捕捉长期依赖关系。

#### 3. [GRU](https://pytorch.org/docs/stable/generated/torch.nn.GRU.html)

GRU 是 LSTM 的一种简化版本，它通过合并一些门来简化结构，同时仍然保留了解决 RNN 局限性的能力。GRU 仅有两个门：更新门和重置门。

![siRNA_Model](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/siRNA%E8%8D%AF%E7%89%A9%E8%8D%AF%E6%95%88%E9%A2%84%E6%B5%8B/siRNA_Model.png)

#### 4. 数据的特征工程

在序列特征的问题转化为表格问题的方法，并在表格数据上做特征工程。



#### 经过上述的提分思路后，新的代码可见：[Task02_baseline2.ipynb](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/siRNA%E8%8D%AF%E7%89%A9%E8%8D%AF%E6%95%88%E9%A2%84%E6%B5%8B/Task02_baseline2.ipynb)



## 参考资料：

- [循环神经网络 (RNN) 原理与代码实例讲解](https://blog.csdn.net/universsky2015/article/details/140413931)
- [参数调优指南](https://lightgbm.readthedocs.io/en/stable/Parameters-Tuning.html)
- [Task2：深入理解赛题，入门RNN和特征工程](https://linklearner.com/activity/12/4/11)
- [2024 AI夏令营 第三期｜【从零入门AI for science（AI+药物）】baseline精读](https://www.bilibili.com/video/BV1WW42197Bm/?spm_id_from=333.999.0.0&vd_source=032fd15745909dc6b77923b4beaa4819)
- [GRU参数](https://pytorch.org/docs/stable/generated/torch.nn.GRU.html)

