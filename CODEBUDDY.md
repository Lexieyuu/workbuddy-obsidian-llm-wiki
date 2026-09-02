# CODEBUDDY.md — LLM Wiki 统一规则

## 1. 目标与边界

### 核心目标

维护一个可复用、可检索、可持续更新的 LLM Wiki 知识库。

### 维护目标

- 负责维护 `wiki/` 目录。
- 将原始资料提炼为结构化知识。
- 建立概念、实体、来源和综合报告之间的关联。
- 保留来源信息，确保事实、观点和结论可追溯。
- 让每次导入和查询都能为后续工作积累可复用的知识。

### 边界

- `raw/` 是原始资料和证据层，默认只读，不直接修改内容。
- 允许新增和更新 `wiki/` 内的 Markdown 页面。
- `assets/` 允许新增附件，但不得擅自删除或覆盖已有附件。
- 不擅自修改知识库目录结构。
- 不删除、移动或覆盖原始资料；如需归档，必须先完成处理并获得用户确认。
- 不确定的内容必须标注为“待确认”或“未知”，不得编造。

## 2. 目录结构约定

### 原始资料层

```text
raw/
├── 01-articles/       网页剪藏、技术文章
├── 02-papers/         论文、研究报告、PDF
├── 03-transcripts/    视频、播客、访谈和会议转录
├── 04-meeting_notes/  会议记录、头脑风暴和工作笔记
└── 09-archive/        已处理并经确认归档的原始资料
```

规则：`raw/` 负责保存原始内容和证据，不在原文件中进行知识加工。`raw/09-archive/` 默认不读取，除非用户明确要求。

### Wiki 维护层

```text
wiki/
├── index.md           全局目录和页面导航
├── log.md             追加式操作记录
├── sources/           单个来源的摘要与观点
├── entities/          人物、公司、模型、工具、产品、项目
├── concepts/          概念、方法论、技术与架构
└── syntheses/         跨来源综合研究报告
```

### 附件层

```text
assets/
```

图片、PDF 和其他附件存放在 `assets/`。引用图片时优先使用 Obsidian 语法 `![[文件名.png]]`。

### 特殊文件

- `wiki/index.md` 是导航目录，不代替知识正文。每个正式 Wiki 页面都必须登记。
- `wiki/log.md` 是时间顺序的操作记录，只能追加，不能删除或重写历史记录。
- 本文件是 WorkBuddy 的项目规则，不是知识正文。

## 3. 资料处理流程

```text
新资料进入 raw/
        ↓
Ingest 导入
        ↓
阅读并识别来源
        ↓
创建 Source 摘要
        ↓
提炼 Concepts 与 Entities
        ↓
建立内部链接
        ↓
创建或更新 Wiki 页面
        ↓
更新 Index 和 Log
        ↓
必要时生成 Synthesis
        ↓
经确认后归档原始资料
```

处理每份资料时必须：

1. 保留原始来源，不改写原始文件。
2. 创建对应的 `wiki/sources/` 页面。
3. 提取重要概念和实体。
4. 建立与已有 Wiki 页面的 `[[双链]]`。
5. 创建新页面，或读取已有页面后增量更新。
6. 同步更新 `wiki/index.md`。
7. 在 `wiki/log.md` 中追加记录。
8. 只有全部更新成功并获得用户确认后，才可将原始资料移动到 `raw/09-archive/`。

## 4. Wiki 页面格式

每个 Wiki 页面必须使用 YAML frontmatter：

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

`type` 可使用：

```text
source | concept | entity | synthesis
```

`status` 可使用：

```text
draft | verified | needs-review | archived
```

正文可按页面类型选择以下结构，不要求每个页面机械地包含全部章节：

```markdown
# 页面标题

## 一句话总结

## 定义

## 核心内容

## 关键观点

## 我的理解

## 相关概念

## 相关实体

## 关联连接

## 来源与引用

## 知识冲突

## 待确认问题
```

“我的理解”只在用户提供个人判断或明确要求加入时使用，不要把 AI 推测写成用户观点。

## 5. 页面类型与命名

| 类型 | 用途 | 建议目录 |
|---|---|---|
| `source` | 记录一篇文章、论文、视频或会议资料的摘要 | `wiki/sources/` |
| `concept` | 记录概念、方法、理论或架构模式 | `wiki/concepts/` |
| `entity` | 记录人物、公司、模型、工具或项目 | `wiki/entities/` |
| `synthesis` | 综合多个来源形成研究报告 | `wiki/syntheses/` |

命名规则：

- Entity 页面使用稳定、清晰的 TitleCase 名称，例如 `ClaudeCode.md`。
- Source、Concept 和 Synthesis 页面使用 kebab-case，例如 `摘要-claude-code.md`。
- 文件名应保持稳定；不要因为措辞变化随意重命名已有页面。

## 6. 链接与引用规则

- 页面之间优先使用 Obsidian 内部链接，例如 `[[ClaudeCode]]`。
- 每个核心概念至少连接一个来源页面。
- 每个来源页面必须记录原始链接或 raw 文件路径。
- 观点、数据、日期和结论必须能够追溯到 `sources` 或 `raw/`。
- 不确定的信息使用 `待确认` 标记。
- 互相冲突的观点必须并列记录，不得强行合并或静默覆盖。
- 每个正式 Wiki 页面必须包含 `## 关联连接`。

## 7. Index 与 Log 维护规则

新增或更新正式页面后，必须同步更新 `wiki/index.md`：

```markdown
## Sources
- [[摘要-source-slug]] — 一句话说明资料主旨

## Entities
- [[EntityName]] — 一句话说明实体

## Concepts
- [[ConceptName]] — 一句话说明概念

## Syntheses
- [[synthesis-slug]] — 一句话说明综合问题
```

每次 `ingest`、`query`、`lint` 或 `sync` 后，在 `wiki/log.md` 末尾追加：

```markdown
## [YYYY-MM-DD] action | 操作简述
- **变更**: 说明新增、更新或检查内容
- **冲突**: 无，或列出待处理冲突
```

## 8. WorkBuddy 工作流

- `/ingest`：扫描 `raw/` 中未归档资料，提取来源、实体和概念，创建或更新 Wiki，并维护 Index 和 Log。
- `/query`：必须先读取 `wiki/index.md`，再读取相关 Wiki 页面；回答时使用 `[[双链]]` 标注依据。
- `/lint`：只读检查死链、孤儿页面、未同步索引和知识冲突；修复前必须得到用户确认。
- 高价值回答经用户确认后，保存到 `wiki/syntheses/` 或其他合适的 Wiki 目录，再更新 Index 和 Log。

查询不是每次都写入 Wiki。普通即时回答不自动固化；只有具有长期复用价值的总结、比较、判断或研究结论才进入 Wiki。

## 9. 写入权限与安全规则

```text
raw/          只读；raw/09-archive/ 默认不读取
assets/       可新增；已有附件不得擅自删除或覆盖
wiki/         允许新增和维护 Markdown 页面
CODEBUDDY.md  修改前必须获得用户确认
```

默认操作：

- 可读取当前 Vault 中与任务相关的目录，但不读取 `raw/09-archive/`。
- 只写入 `wiki/`、新建的 `assets/` 附件和用户明确指定的输出目录。
- 不删除、移动、重命名或覆盖已有文件，除非用户明确确认具体目标。
- 发现同名页面时，先读取并增量合并，不直接覆盖。

## 10. 质量检查

每次导入或更新后检查：

- 是否存在断链；
- 是否存在没有来源的关键观点；
- 是否有重复或高度重叠页面；
- 是否有知识冲突；
- 是否错误修改了原始资料；
- 是否更新了 `wiki/index.md`；
- 是否记录了 `wiki/log.md`；
- 是否为新增页面建立了关联连接。

**核心原则：**

> Raw 保存原始证据，CODEBUDDY.md 定义维护规则，Wiki 沉淀结构化知识，Index 负责定位，Log 负责追踪，引用保证可信。
