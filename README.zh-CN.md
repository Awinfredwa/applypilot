# ApplyPilot

[English](README.md) · **中文**

一个 AI 求职助手：每天帮你在 LinkedIn 上找新发布的实习，筛掉不合适的，再花几分钟陪你把申请投出去。

## 从这里开始（不需要任何技术背景）

**你需要：** 一台 Mac 或 Windows 电脑、Google Chrome、LinkedIn 账号，以及包含 Codex 的 ChatGPT 订阅（Plus 或以上）。

1. 从 [chatgpt.com/download](https://chatgpt.com/download) 下载 **ChatGPT 桌面版**，登录后打开 **Codex**。
2. 在 Chrome 里安装 **[Codex 扩展](https://chromewebstore.google.com/detail/codex/hehggadaopoacecdllhhajmbjkdcmajg)**，并确认已登录 **LinkedIn**。
3. 在 Codex 里选择模型 **GPT-6 Sol**，effort 设为 **High**。
4. 上传你的简历，复制下面这段话，把空白处填好后发送：

   ```text
   Set up ApplyPilot for me: https://github.com/Awinfredwa/applypilot
   I'm looking for: ______________________
   (example: summer 2027 marketing or finance internships in New York or remote)
   My resume is attached. Do all the technical setup yourself and only ask me what you need.
   ```

   （这段话也可以用中文写，比如“我想找：2027 年暑期的市场或金融实习，纽约或远程”。）

5. 回答它的几个问题（大约 15 分钟）。它会先陪你做一次练习搜索，再和你一起投出第一份申请，然后设置成每天自动运行。

设置就这么多。你不需要碰任何文件或代码。

## 每天会发生什么

1. **它自动帮你找岗位。** 每天在你选的时间，它会搜索 LinkedIn 上次搜索之后新发布的实习，筛掉不合适的，然后给你发一份简短清单。
2. **你花几分钟投递。** 回复 **apply**。它会逐个填好 LinkedIn Easy Apply 表格，给你看一个简短摘要，你说可以它才提交（默认每天最多 10 个）。
3. **它会学习。** 表格里出现新问题时，它只问你一次，以后都记得。

清单里还会列出几个特别匹配、但不是 Easy Apply 的岗位，附上链接，你想投的话可以自己去公司官网投。

**在设定时间前后，保持电脑开机、Chrome 打开。** 如果某天错过了，下次搜索会自动补上。大多数岗位在工作日上午发布，所以中午左右搜索能赶上最新的一批。

## 之后怎么跟它说话

打开 Codex，直接说：

- “ApplyPilot: apply”
- “ApplyPilot: 也帮我找 data analyst 实习”
- “ApplyPilot: 波士顿的岗位都跳过”
- “ApplyPilot: 暂停到周一”
- “ApplyPilot: 改成晚上 7 点搜索”
- “ApplyPilot: 这周我投了哪些？”

你的记录在个人主文件夹里的 **ApplyPilot** 文件夹中。`tracker.csv` 可以用 Excel、Numbers 或 Google Sheets 打开。

## 需要知道的

- **它不会编造任何信息。** 工作许可、薪资、GPA、日期都只用你提供的。不知道就问你。
- **没有你的同意不会提交。** 每一份申请都由你确认。
- **它不会替你过安全验证。** 验证码、登录码、人机验证都需要你自己处理。遇到时它会停下来告诉你。
- **对 LinkedIn 温和一点。** LinkedIn 不喜欢自动化工具。ApplyPilot 保持正常节奏（默认每天 10 个），LinkedIn 一出现警告就暂停 2 天。账号终究是你的，你随时可以调低上限。
- **你的信息只存在你的电脑上。** 简历、答案和记录都在你的 ApplyPilot 文件夹里，不会进入这个 repo。

---

## 给 AI Agent

从 [AGENTS.md](AGENTS.md) 和 [SKILL.md](SKILL.md) 开始。快速设置：[references/quickstart-setup.md](references/quickstart-setup.md)。每日搜索与投递：[references/daily-linkedin-run.md](references/daily-linkedin-run.md)。

## 进阶

完整工作流包括多个招聘网站、简历定制、Precision/Volume 模式和完整 dashboard。见 [docs/full-workflow.zh-CN.md](docs/full-workflow.zh-CN.md)。

## 致谢

基于 Yvonne He 的 [ApplyPilot](https://github.com/yvonnehe772/applypilot)。MIT License。
