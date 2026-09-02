---
name: ingest
description: 将 LLM Wiki Vault 的 raw/ 原始资料整理成 wiki/ 知识页，并维护索引与日志。当用户要求导入、整理、摄取文章、论文、PDF、转录或笔记时使用；也支持 /ingest 和 /ingest <路径>。
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
user-invocable: true
---

# LLM Wiki 资料导入

将原始资料编译为结构化、可引用、相互链接的 Obsidian 知识页。

## 目录约定

- `raw/01-articles/`：网页剪藏和 Markdown 文章
- `raw/02-papers/`：论文、报告和 PDF
- `raw/03-transcripts/`：视频、播客和会议转录
- `raw/09-archive/`：已处理资料；绝对不要读取
- `wiki/sources/`：每份原始资料的摘要
- `wiki/entities/`：人物、公司、工具、产品
- `wiki/concepts/`：概念、框架、方法论
- `wiki/syntheses/`：综合研究报告

先定位当前 Vault 根目录。所有路径均相对于 Vault 根目录，不要把 Skill 自身目录当作 Vault。

## 工作流程

1. 确认输入路径。没有指定路径时，扫描 `raw/`，但排除 `raw/09-archive/`。
2. 完整读取原始文件。Markdown 读取全文；PDF 尽量提取文本，无法提取时记录文件名和页数，不编造内容。
3. 提取核心主旨、实体和概念；非中文资料翻译成简体中文。
4. 在 `wiki/sources/` 创建或更新来源摘要。
5. 为每个实体和概念创建页面，或读取旧页面后增量合并；不要静默覆盖旧信息。
6. 在每个页面加入 `## 关联连接`，使用 `[[页面名称]]` 建立链接。
7. 更新 `wiki/index.md` 和 `wiki/log.md`。
8. 只有在上述页面、索引和日志都成功写入后，才将源文件移动到 `raw/09-archive/`；移动前向用户说明将归档哪些文件并请求确认。

## 来源摘要模板

```markdown
---
title: "摘要-文件-slug"
type: source
tags: [来源, 原始文件]
sources: [raw/01-articles/example.md]
last_updated: YYYY-MM-DD
---

## 核心摘要
用 3-5 句话总结资料。

## 关联连接
- [[EntityName]] — 关联实体
- [[ConceptName]] — 关联概念
```

文件名使用 kebab-case，例如 `摘要-某篇文章.md`。

## 实体/概念页面模板

```markdown
---
title: "页面名称"
type: entity
tags: [标签]
sources: [raw/01-articles/example.md]
last_updated: YYYY-MM-DD
---

## 定义

## 关键信息

## 关联连接
- [[摘要-文件-slug]] — 来源
```

实体放入 `wiki/entities/`，概念放入 `wiki/concepts/`。实体使用 TitleCase；概念和来源使用 kebab-case。

## 冲突处理

发现新旧知识冲突时暂停当前导入，明确展示冲突双方，询问用户选择：保留并标注冲突、采用新信息、或放弃本次导入。不得静默覆盖。

## 硬约束

- 不读取 `raw/09-archive/`。
- 不修改原始文件内部文字。
- 所有生成页面使用简体中文并包含 YAML frontmatter。
- 新增页面必须同步更新 `wiki/index.md` 和 `wiki/log.md`。
