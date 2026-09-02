# Karpathy LLM Wiki — WorkBuddy Skills

这是一份将 `karpathy-llm-wiki-vault` 的 `ingest`、`query`、`lint` 工作流适配到 WorkBuddy/CodeBuddy Skills 格式的版本。

## 安装

将 `.codebuddy/skills/` 目录复制到你的 LLM Wiki Vault 根目录：

```text
你的知识库/
├── raw/
├── wiki/
├── CODEBUDDY.md
└── .codebuddy/
    └── skills/
        ├── ingest/SKILL.md
        ├── query/SKILL.md
        └── lint/SKILL.md
```

WorkBuddy 官方文档支持项目级 `.codebuddy/skills/` 和用户级 `~/.codebuddy/skills/`；建议先使用项目级目录，避免影响其他项目。

安装后重新加载 Skills，或重启 WorkBuddy。可用 `/ingest`、`/query`、`/lint` 测试。

本版本已经将原来的 `CLAUDE.md` 规则完整改写为 `CODEBUDDY.md`，不再依赖 `CLAUDE.md` 或 `WORKBUDDY.md`。

## 注意

- 这些 Skill 只提供工作流，不包含你的个人资料。
- 先创建 `raw/` 和 `wiki/` 目录，再运行 `/ingest`。
- `raw/` 是证据层，Skill 不会改写原始文件。
- 归档前会先确认摘要页、实体/概念页、索引和日志均已完成。
