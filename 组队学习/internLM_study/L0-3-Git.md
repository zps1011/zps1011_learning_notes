<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>书生大模型实战营【第三期】学习笔记<br/><span>入门岛 - Git 基础</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年8月20日
</div>

## 前言

### 1.Git

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。Git 是由林纳斯·托瓦兹（Linux Torvalds）为了帮助管理 Linux 内核开发而设计开发的，自 2005 年以来 Git 在软件开发社区中迅速普及，并成为了最流行的版本控制系统之一。

### 2.Git特点

- **分布式**：与传统的集中式版本控制系统（如 SVN）不同，Git 将仓库完整地镜像到每一台工作机上，这样任何一台机器上都可以随时访问项目的历史版本，并且可以在本地提交更改。这极大地提高了开发效率和安全性，因为即使中心服务器出现问题，本地仓库依然保留项目的完整历史。

- **高效**：Git 对数据的处理非常高效，因为它仅在必要时才处理数据，并且采用了压缩和优化的数据存储方式。此外，Git 的分支和合并操作也非常快，这对于大型项目和频繁的分支切换至关重要。
- **支持非线性开发**：Git 鼓励开发人员创建和使用分支来隔离开发工作。这使得同时处理多个功能和修复多个问题变得简单且高效。开发者可以在自己的分支上自由工作，而不会影响到主分支（master 或 main 分支），待功能完成并经过测试后，再合并回主分支。
- **数据完整性**：Git 使用了 SHA-1 哈希来确保数据的完整性和唯一性。每次提交都会计算出一个唯一的哈希值，并用于引用提交的内容。这确保了数据在传输和存储过程中不会被篡改。
- **易于学习和使用**：Git 的命令行界面非常直观，而且有许多图形界面工具（如 GitHub Desktop, SourceTree, GitKraken 等）可供选择，对初学者也很友好。
- **开源**：Git 是一个自由开源软件，任何人都可以免费使用和修改。
- **跨平台**：Git 可以在 Windows、Linux 和 Mac OS 等多种操作系统上运行，具有很好的移植性。
- **安全性**：Git 采用 SSH 协议进行通信，数据传输安全可靠。

## 闯关任务及过程

### 1. 破冰活动：自我介绍

> - 任务描述：每位学习者需提交一份自我介绍。
> - 提交地址：https://github.com/InternLM/Tutorial 的 camp3 分支
> - 提交要求：
> 
>   1.命名格式为 `camp3_<id>.md`，其中 `<id>` 是您的报名问卷ID。
>   
>   2.文件路径应为 `./data/Git/task/`。
>   
>   3.【大家可以叫我】内容可以是 GitHub 昵称、微信昵称或其他网名。
>   
>   4.在 GitHub 上创建一个 Pull Request，提供对应的 PR 链接。
> - 任务步骤：
> 
>   1.将该仓库的 camp3 分支 fork 到自己的 GitHub 仓库。
>   
>   2.在 `./data/Git/task/` 目录下创建一个名为 `camp3_<id>.md` 的文件，其中 `<id>` 是您的报名问卷ID。
>   
>   3.在文件中填写自我介绍内容。
>   
>   4.提交 Pull Request，将自我介绍文件请求添加到 camp3 分支。

打开指定的仓库地址，点击右上方的 `fork`，即可将该仓库分叉到自己的 GitHub 仓库中，则可在自己的 GitHub 仓库列表中看到分支的仓库。

> ！！！注意执行 fork 操作时，取消勾选“Copy the camp3 branch only”选项

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-01.png)

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-02.png)

在自己的 GitHub 仓库中，找到 `camp3` 分支，点击进入，然后在 `./data/Git/task/` 目录下创建一个名为 `camp3_<id>.md` 的文件。在文件中填写自我介绍内容并保存。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-03.png)

在当前仓库点击 `Pull Request` 选项，然后点击右侧 `New pull request` 按钮.

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-04.png)

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-05.png)

下图的页面即显示两分支的不同之处与个人修改过的相关信息，将自我介绍文件请求添加到 `camp3` 分支。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-06.png)

在页面中填写好相关信息，提交title为git_报名问卷的id_introduction 格式，如git_2208_introduction。然后保存提交 Pull Request，将自我介绍文件请求添加到 camp3 分支，随后便能够在原仓库的 `Pull requests` 中看到自己的提交请求。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-07.png)



### 2. 实践项目：构建个人项目

> - 任务描述：创建一个个人仓库，用于提交笔记、心得体会或分享项目。
> - 任务步骤：
>
>   1.在 GitHub 中创建一个公开(public)仓库。
>      
>   2.创建仓库介绍文件，并添加超链接跳转至 [书生大模型实战营](https://github.com/InternLM/Tutorial)

在GitHub 中，点击 `+` 按钮，再点击到 `New repository` 仓库创建页面。在创建的页面中填写相关信息后，点击`Create repository`创建仓库。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-08.png)

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-09.png)

在仓库首页的 `README.md` 文件，填写指定仓库介绍内容，并添加超链接。

![image](https://github.com/zps1011/zps1011_learning_notes/blob/main/%E7%BB%84%E9%98%9F%E5%AD%A6%E4%B9%A0/internLM_study/images/L0-3-Git-10.png)

## 总结

本次任务主要是完成了两个 Git 课程任务，分别是将自我介绍按照代码贡献的方式提交到指定仓库，与构建自己的个人仓库。通过这两个任务，我学会了如何通过 GitHub 提交开源代码贡献和创建个人仓库。这项技能在我未来的学习生涯和职业生涯中扮演至关重要的角色，带来极大的便利与帮助。掌握Git不仅能提升的开发效率，还能促进团队协作，确保项目的版本控制和历史管理更加有序和安全。

## 参考资料

- [Git 官方文档](https://git-scm.com/doc)
- [GitHub 官方文档](https://docs.github.com/cn)
- [书生大模型实战营【第3期】学员闯关手册](https://aicarrier.feishu.cn/wiki/XBO6wpQcSibO1okrChhcBkQjnsf)
- [书生大模型实战营【第3期】 Git 基础知识文档](https://github.com/InternLM/Tutorial/tree/camp3/docs/L0/Git)
- [Git 基础知识](https://aicarrier.feishu.cn/wiki/YAXRwLZxPi8Hy6k3tOQcuwAHn5g)

