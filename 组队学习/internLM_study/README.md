【项目地址】
https://github.com/internLM/tutorial

【闯关手册】
https://aicarrier.feishu.cn/wiki/XBO6wpQcSibO1okrChhcBkQjnsf

【InternStudio开发机】
https://studio.intern-ai.org.cn

【初始化InternStudio开发机】
- 慎重执行，所有数据将会丢失。此操作仅限 InternStudio 平台，自己的机器千万不要这么操作。
   - 第一步，**本地终端** ssh 连接开发机（不能在 web 页面操作，不要用 vscode 的 ssh连接）
   - 第二步，执行 `rm -rf /root` ，静待其执行完成
   - 第三步，重启开发机，系统会重置 `/root` 路径下的配置文件
   - 第四步， `ln -s /share /root/share`
