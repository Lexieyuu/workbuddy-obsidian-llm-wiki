---
name: ingest
description: 将 raw/ 原始资料整理成 wiki/ 知识页，并维护索引与实际变更日志；支持 /ingest 和 /ingest <路径>。
allowed-tools: Read, Write, Edit, Bash, Glob, Grep
user-invocable: true
---

# LLM Wiki 资料导入

将原始资料编译为结构化、可引用、相互链接的 Obsidian 知识页。目录、页面 Schema、来源字段、权限和日志规则统一遵循仓库根目录的 `CODEBUDDY.md`，本 Skill 不重复定义它们。

## 工作流程

1. 定位当前 Vault 根目录。没有指定路径时扫描 `raw/`，排除 `raw/09-archive/`。
2. 完整读取原始文件；无法提取的内容明确标注未知，不编造。
3. 识别来源、核心主旨、实体、概念、比较关系和待确认问题。
4. 创建或增量更新对应 Source 页面，并在页面中保存来源元数据；其他页面的 `sources` 只引用该 Source 页面。
5. 创建或更新合适的 Concept、Entity、Comparison、Overview 或 Synthesis 页面，保留已有内容并建立 `[[双链]]`。
6. 更新 `wiki/index.md`；仅当确有实际变更时按 `CODEBUDDY.md` 规则追加 `wiki/log.md`。
7. 所有页面、Index 和 Log 写入成功后，向用户说明待归档文件并请求确认；未经确认不得移动到 `raw/09-archive/`。

## 冲突处理

发现新旧知识冲突时暂停相关导入，展示冲突双方和来源，询问用户是保留并标注冲突、采用新信息，还是放弃本次导入。不得静默覆盖。

## 约束

- 不读取或修改 `raw/09-archive/` 中的内容。
- 不修改原始文件内部文字。
- 不把 AI 推测写入“我的标注”或“我的理解”。AI 生成内容放入“AI 分析”，并标明依据。
- 不擅自将页面标记为 `verified`。
