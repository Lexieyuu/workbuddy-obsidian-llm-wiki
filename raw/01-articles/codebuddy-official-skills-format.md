---
title: "CodeBuddy 官方 Skills 格式说明"
type: source
source_url: https://cloud.tencent.com/document/product/1831/137020
captured: 2026-09-02
---

# CodeBuddy 官方 Skills 格式说明

> 本文件是官方文档的案例摘录与索引，不是官方文档全文。完整内容请访问 source_url。

## 核心摘要

官方格式以独立目录中的 `SKILL.md` 为核心，并可使用 YAML Frontmatter 描述名称、用途和工具权限。项目级 Skill 放在 `.codebuddy/skills/`，用户级 Skill 放在 `~/.codebuddy/skills/`。

## 关键格式

```text
.codebuddy/skills/
└── skill-name/
    └── SKILL.md
```

## 与本知识库的关联

- 本项目使用 `.codebuddy/skills/ingest/SKILL.md`。
- 本项目使用 `.codebuddy/skills/query/SKILL.md`。
- 本项目使用 `.codebuddy/skills/lint/SKILL.md`。

## 原始来源

- https://cloud.tencent.com/document/product/1831/137020
