<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>聪明办法学 python 第二版<br/><span>Task 01：安装 Installation</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2025年1月15日
</div>



## 一、Marscode 注册

注册链接：https://www.marscode.cn/

第一步：点击注册链接后可使用手机号进行注册。

<div align=center>
<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task01-01.png" alt="MarsCode 注册"/>
</div>


第二步：注册成功后选择 MarsCode IDE，点击`打开网页立即体验`，进入工作台。

<div align=center>
<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task01-02.png" alt="选择 MarsCode IDE"/>
</div>


第三步：点击导入 git 项目。

<div align=center>
<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task01-03.png" alt="导入 git 项目"/>
</div>


第四步：写入课程 github 地址。

<div align=center>
<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task01-04.png" alt="写入课程地址"/>
</div>


第五步：安装 jupyter，输入以下代码：

```python
pip install jupyter
```

<div align=center>
<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task01-05.png" alt="安装 jupyter"/>
</div>



这样，云端环境配置就完成啦 ~~~

## 二、本地安装清单推荐

> 建议所有安装均建议选择默认配置

- Steam++（Watt Toolkit）：https://steampp.net/ 。此应用可以帮助我们流畅访问 github，huggingface 等。

   - 点击“网络加速”，勾选“Github”
   - 打开“加速设置”，点击“安装证书”
   - 点击“代理设置”，关闭“启用 DoH”
   - (Bug) 在安装好 Git 后，运行：

```
git config --global http.sslBackend schannel 
```

- Visual Studio Code: [https://code.visualstudio.com](https://code.visualstudio.com/)

- Git: https://git-scm.com/download

- Miniconda：https://repo.anaconda.com/miniconda/


> 建议迁移软件： Miniconda => Miniforge
>
> 运行命令：conda => mamb
>
> Miniforge 下载地址：https://github.com/conda-forge/miniforge/releases

- Github 加速：https://github.moeyy.xyz




**本人在学习本课程之前已经在本地安装了相应的软件，且能正常运行。**

3、修改 powershell 执行策略

我们在 **Anaconda Powershell Prompt** 中输入：

```
conda init
```

在 Visual Studio Code Terminal 中出现 出现`“在此系统上禁止运行脚本” `的情况下（如图所示），需要执行以下操作：

<div align=center>
<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task01-06.png" alt="报错样式"/>
</div>



在 win 10 系统中，按下 win + r，输入 powershell，回车。进入终端后输入以下命令：

```
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

出现如下内容后，输入：`A`，回车。

<div align=center>
<img src="https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/%E8%81%AA%E6%98%8E%E5%8A%9E%E6%B3%95%E5%AD%A6python%EF%BC%88%E7%AC%AC%E4%BA%8C%E7%89%88%EF%BC%89/images/task01-07.png" alt="更改执行策略"/>
</div>

4、创建、激活与删除 conda 环境

创建 Conda 环境

```
conda create -n zps1011 python=3.12
```

其中 ***-n*** 代表创建的环境名称，上述代码指创建一个名为 zps1011 的 conda 环境，并指定 Python 版本为 3.12。

激活刚刚创建的 conda 环境：

```
conda activate zps1011 
```

如果需要删除某个 conda 环境：先退出，再删除。不能在拟删除的 conda 环境中删除该 conda 环境。

```
conda deactivate # 退出该环境
conda remove -n zps1011 --all # 删除整个环境
```





## 参考资料

- [github - 聪明办法学 python - Installation](https://github.com/datawhalechina/learn-python-the-smart-way-v2/blob/main/slides/chapter_0-Installation.ipynb)
- [聪明办法学 python 视频资料 - Chap 0 安装（2025）](https://www.bilibili.com/video/BV1SscDeQE8B/?spm_id_from=333.337.search-card.all.click)
