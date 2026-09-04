---
name: query
description: 从本地 LLM Wiki 检索并回答关于知识库、历史记录和过往决策的问题；支持 /query。
allowed-tools: Read, Glob, Grep
user-invocable: true
---

# LLM Wiki 知识检索

优先使用本地 Wiki 回答问题，不用模型记忆代替检索。目录、Schema、引用和写入权限统一遵循 `CODEBUDDY.md`，本 Skill 不重复定义。

## 工作流程

1. 定位当前 Vault 根目录，先完整读取 `wiki/index.md`。
2. 从索引中选择相关的 Source、Entity、Concept、Comparison、Overview 或 Synthesis 页面。
3. 完整读取相关页面，必要时沿 `[[双链]]` 继续查阅；不得把未读取页面当作依据。
4. 综合回答，并在使用 Wiki 信息的位置标注 `[[页面名称]]`。
5. 如果本地 Wiki 没有答案，明确说明“本地知识库中未找到相关内容，以下为通用知识回答：”。

## 只读规则

`/query` 默认只读：不修改 Wiki，不追加 `wiki/log.md`，不自动把回答保存为页面。若回答具有长期复用价值，可询问用户是否保存；只有得到同意后，才创建合适的 Comparison 或 Synthesis 页面并同步 Index 与实际变更 Log。

## 引用规则

- 使用 `[[页面名称]]` 标记已读取的 Wiki 页面。
- 原始资料中的具体句子只做简短引用，并注明 Source 页面。
- “我的标注”“我的理解”仅在明确属于用户内容时作为用户观点；AI 推理标为“AI 分析”。
