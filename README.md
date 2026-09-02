# Karpathy LLM Wiki — WorkBuddy + Obsidian

这是一个参考 [`karpathy-llm-wiki-vault`](https://github.com/jason-effi-lab/karpathy-llm-wiki-vault) 的 WorkBuddy + Obsidian 版本。

## 核心理念

传统 RAG 往往在每次提问时重新从原始资料中检索和拼接答案；LLM Wiki 则让 AI 把资料逐步“编译”为一个持久、结构化、相互链接的 Markdown 知识网络。

```text
Raw Sources（原始资料）
        ↓ Ingest
Wiki（结构化、相互链接的知识）
        ↓ Query
答案与新的高价值知识
        ↓
持续更新 Wiki
```

- 人负责收集资料、提出问题和做最终判断。
- WorkBuddy 负责提炼、归类、建立链接、维护索引和记录日志。
- Obsidian 负责查看 Markdown 页面、双链和知识图谱。
- `raw/` 是事实来源，`wiki/` 是 AI 维护的知识层。

## 安装

将 `.codebuddy/skills/` 目录复制到你的 LLM Wiki Vault 根目录：

```text
你的知识库/
├── assets/                         图片、PDF、附件
├── raw/                            原始资料层（只读）
│   ├── 01-articles/                网页剪藏、技术文章
│   ├── 02-papers/                  论文、研究报告、PDF
│   ├── 03-transcripts/             视频、播客、会议转录
│   ├── 04-meeting_notes/           会议记录、头脑风暴
│   └── 09-archive/                 已处理并确认归档的资料
├── wiki/                           结构化知识层
│   ├── index.md                    全局内容索引
│   ├── log.md                      追加式操作日志
│   ├── concepts/                   概念、框架、方法论
│   ├── entities/                   人物、公司、工具、项目
│   ├── sources/                    原始资料摘要
│   └── syntheses/                  跨来源综合报告
├── CODEBUDDY.md
└── .codebuddy/
    └── skills/
        ├── ingest/SKILL.md
        ├── query/SKILL.md
        └── lint/SKILL.md
```

WorkBuddy 官方文档支持项目级 `.codebuddy/skills/` 和用户级 `~/.codebuddy/skills/`；建议先使用项目级目录，避免影响其他项目。

安装后重新加载 Skills，或重启 WorkBuddy。可用 `/ingest`、`/query`、`/lint` 测试。

本版本将原仓库的 `CLAUDE.md` / `.claude/skills/` 适配为 WorkBuddy 的 `CODEBUDDY.md` / `.codebuddy/skills/`，保留 Karpathy LLM Wiki 的核心理念和目录结构。

## 注意

- 这些 Skill 只提供工作流，不包含你的个人资料。
- 先在 Obsidian 中打开这个仓库作为 Vault，再运行 `/ingest`。
- `raw/` 是证据层，Skill 不会改写原始文件。
- 归档前会先确认摘要页、实体/概念页、索引和日志均已完成。
