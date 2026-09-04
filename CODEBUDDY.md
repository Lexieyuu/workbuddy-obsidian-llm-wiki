# CODEBUDDY.md — LLM Wiki 统一规则

## 1. 目标与边界

维护一个可复用、可检索、可持续更新、来源可追溯的 LLM Wiki。

- `raw/` 是原始资料与证据层，默认只读；不得改写已有原始文件。
- `wiki/` 是结构化知识层，允许新增和维护 Markdown 页面。
- `assets/` 只允许新增附件，不得擅自删除或覆盖已有附件。
- `raw/09-archive/` 默认不读取；归档必须由用户明确指定或确认具体文件。
- 不擅自修改目录结构，不删除、移动、重命名或覆盖已有文件，除非用户明确确认目标。
- 不确定的信息标记为“待确认”或“未知”，不得编造事实、来源、引用或用户观点。

## 2. 目录结构

```text
raw/
├── 01-articles/       网页剪藏、技术文章、博客
├── 02-papers/         论文、研究报告、PDF
├── 03-transcripts/    视频、播客、访谈、演讲、会议转录
├── 04-meeting-notes/  会议记录、头脑风暴、工作笔记
└── 09-archive/        已处理并经用户确认归档的原始资料

wiki/
├── concepts/          概念、方法论、技术、机制、架构、理论
├── entities/          人物、公司、组织、模型、工具、产品、项目
├── sources/           单个原始来源的知识化摘要
├── comparisons/       多个对象、方法或方案的比较
├── overview/          主题总览、知识地图、专题导航
├── syntheses/         基于多个来源的综合分析与研究结论
├── index.md           全局入口与一级导航
└── log.md             实际变更记录

assets/                图片、PDF、截图、图表、音频及其他附件
```

`index.md` 负责定位知识，不代替正文；页面较多时用 `overview/` 建立专题导航。`log.md` 只能追加实际发生的变更，不记录普通查询或无修改的检查。

## 3. 知识分层与工作流

```text
Raw 证据 → Source 来源页 → Concept/Entity 结构化页面
                         → Comparison / Synthesis 综合页面
                         → Overview / Index 导航
```

处理资料时：读取未归档的 `raw/` → 创建或增量更新 Source 页面 → 提取并连接 Concepts、Entities → 必要时创建 Comparison、Synthesis 或 Overview → 更新 `wiki/index.md` → 对实际变更追加 `wiki/log.md`。只有全部处理完成且用户确认后，才可移动原始资料到 `raw/09-archive/`。

## 4. Wiki 页面 Schema

每个正式 Wiki 页面使用 YAML frontmatter：

```yaml
---
title: "页面标题"
type: concept
status: draft
sources: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
---
```

`type` 为 `source | concept | entity | comparison | overview | synthesis`；`status` 为 `draft | verified | needs-review | archived`。AI 不得擅自将页面设为 `verified`，除非用户明确确认。

`sources` 字段只保存 Source 页面链接，例如 `[[摘要-example]]`，不得直接混用 URL 或 `raw/` 路径。Source 页面自身负责保存证据元数据：

```yaml
source_url: "https://example.com/article"
raw_path: "raw/01-articles/example.md"
author: "作者或未知"
published: "YYYY-MM-DD 或未知"
```

页面可按类型选择章节，但涉及用户与 AI 内容时必须区分：

```markdown
## 我的标注
用户明确提供的重点、批注或判断；没有用户内容时不创建或留空。

## 我的理解
用户明确表达的个人理解或经确认的解释；不得把 AI 推测写成用户观点。

## AI 分析
AI 基于已读取来源做出的推理、归纳、比较或建议，必须标明依据。
```

正式页面通常还应包含 `## 一句话总结`、`## 核心内容`、`## 关联连接`、`## 来源与引用`、`## 知识冲突` 或 `## 待确认问题` 中适用的章节。页面之间优先使用 `[[双链]]`；互相冲突的观点并列记录，不得静默合并。

## 5. 页面分类与命名

| 类型 | 目录 | 命名 |
|---|---|---|
| source | `wiki/sources/` | kebab-case |
| concept | `wiki/concepts/` | kebab-case |
| entity | `wiki/entities/` | 稳定、清晰的 TitleCase |
| comparison | `wiki/comparisons/` | kebab-case |
| overview | `wiki/overview/` | kebab-case |
| synthesis | `wiki/syntheses/` | kebab-case |

文件名保持稳定，不因措辞变化随意重命名已有页面。

## 6. Index、Log 与 Skills

新增或更新正式页面后，同步登记 `wiki/index.md`，按 Sources、Entities、Concepts、Comparisons、Overview、Syntheses 分类。引用必须能追溯到 Source 页面或其 `raw_path`。

`wiki/log.md` 只追加实际发生的新增、更新、删除/移动、归档、索引、结构变化或冲突处理。格式为：

```markdown
## [YYYY-MM-DD] action | 操作简述
- **变更**: 说明实际变更
- **冲突**: 无，或列出待处理冲突
```

`.codebuddy/skills/` 下的 Skill 只描述任务流程和输出要求，不重复定义本文件的目录、Schema、权限或日志规范；发生冲突时以本文件为准。

- `/ingest`：将 `raw/` 资料知识化，更新相关页面、Index 和 Log；归档前请求确认。
- `/query`：先读 Index，再按需读取相关页面并回答；默认只读，不写 Log，不自动固化答案。
- `/lint`：只读检查死链、孤儿页、索引、Schema 和冲突；默认不修改文件，无修改不写 Log。

## 7. 写入与质量检查

可读取与任务相关的 Vault 内容，但排除 `raw/09-archive/`；只写入 `wiki/`、新建 `assets/` 和用户明确指定的输出目录。发现同名页面先读取并增量合并。

每次导入或更新后检查：断链、孤儿页、未同步索引、无来源的关键观点、重复页面、知识冲突、原始资料是否被修改，以及新增页面是否建立关联连接。

> Raw 保存证据，CODEBUDDY.md 定义规则，Wiki 沉淀知识，Index 负责定位，Log 追踪变更，引用保证可信。
