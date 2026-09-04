# Karpathy LLM Wiki — WorkBuddy + Obsidian

这是一个参考 [`karpathy-llm-wiki-vault`](https://github.com/jason-effi-lab/karpathy-llm-wiki-vault) 的 WorkBuddy + Obsidian 知识库模板。

## 它解决什么问题

LLM Wiki 将原始资料逐步“编译”为持久、结构化、相互链接的 Markdown 知识网络：

```text
Raw 证据 → Source 来源页 → Concept / Entity
                              ↓
                 Comparison / Synthesis
                              ↓
                    Overview / Index 导航
```

人负责收集资料和最终判断；WorkBuddy 负责提炼、归类、建立链接和维护索引；Obsidian 负责浏览双链与知识图谱。

## 目录结构

```text
你的知识库/
├── assets/                         附件
├── raw/                            原始资料层（默认只读）
│   ├── 01-articles/                网页剪藏、技术文章
│   ├── 02-papers/                  论文、研究报告、PDF
│   ├── 03-transcripts/             视频、播客、访谈、会议转录
│   ├── 04-meeting-notes/           会议记录、头脑风暴
│   └── 09-archive/                 已处理并确认归档的资料
├── wiki/                           结构化知识层
│   ├── concepts/                   概念、框架、方法论
│   ├── entities/                   人物、公司、模型、工具、项目
│   ├── sources/                    单个来源摘要与元数据
│   ├── comparisons/                多对象比较
│   ├── overview/                   主题总览与知识地图
│   ├── syntheses/                  跨来源综合报告
│   ├── index.md                    全局索引
│   └── log.md                      追加式变更记录
├── CODEBUDDY.md                    全局维护规则
└── .codebuddy/skills/              任务型工作流
    ├── ingest/SKILL.md
    ├── query/SKILL.md
    └── lint/SKILL.md
```

完整目录、页面 Schema、来源元数据和写入权限以 [`CODEBUDDY.md`](CODEBUDDY.md) 为准。`sources` 字段只引用 Source 页面；Source 页面保存 `source_url`、`raw_path`、作者和发布日期等元数据。页面中的“我的标注”“我的理解”和“AI 分析”必须分别表示用户标注、用户理解与 AI 推理。

## 工作流

- `/ingest`：读取未归档资料，创建或更新 Source、Concept、Entity 等页面，并同步 Index 与实际变更 Log；归档前请求确认。
- `/query`：先读 `wiki/index.md`，再读取相关页面并用双链引用；默认只读、不写 Log、不自动保存答案。
- `/lint`：只读检查死链、孤儿页、未同步索引、Schema 与知识冲突；无修改不写 Log。

高价值的比较、总结或研究结论，经用户同意后保存到 `wiki/comparisons/` 或 `wiki/syntheses/`。

## 安装

将 `.codebuddy/skills/` 目录复制到 LLM Wiki Vault 根目录，在 WorkBuddy 中重新加载 Skills 或重启，然后用 `/ingest`、`/query`、`/lint` 测试。先在 Obsidian 中打开该目录作为 Vault。

本项目只提供工作流，不包含用户个人资料；原始资料不会被 Skill 改写。
