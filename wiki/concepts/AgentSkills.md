---
title: "AgentSkills"
type: concept
status: verified
sources:
  - raw/01-articles/workbuddy-official-skills.md
  - raw/01-articles/codebuddy-official-skills-format.md
created: 2026-09-02
updated: 2026-09-02
tags: [skill, workflow, workbuddy]
---

# AgentSkills

## 定义

Agent Skill 是写给 AI 的可复用能力和工作流说明，可以封装特定领域的知识、操作步骤、工具权限和辅助资源。

## 在本项目中的作用

- `ingest`：将原始资料整理成 Wiki 知识。
- `query`：根据 Index 定位并读取相关 Wiki 页面。
- `lint`：检查死链、孤儿页面、索引遗漏和知识冲突。

## WorkBuddy 格式

```text
.codebuddy/skills/
└── skill-name/
    └── SKILL.md
```

每个 `SKILL.md` 使用 YAML Frontmatter 声明 `name`、`description` 和可选的 `allowed-tools`，正文描述触发条件、工作流程和安全边界。

## 安全边界

第三方 Skill 可能读取本地文件、使用凭证或调用外部服务。安装前应审查来源和权限；本项目的知识库 Skill 默认不修改 `raw/` 原始资料。

## 关联连接

- [[WorkBuddy]]
- [[摘要-workbuddy-official-skills]]
- [[摘要-codebuddy-official-skills-format]]
- [ingest Skill](../../.codebuddy/skills/ingest/SKILL.md)
- [query Skill](../../.codebuddy/skills/query/SKILL.md)
- [lint Skill](../../.codebuddy/skills/lint/SKILL.md)

## 来源与引用

- [[摘要-workbuddy-official-skills]]
- [[摘要-codebuddy-official-skills-format]]
