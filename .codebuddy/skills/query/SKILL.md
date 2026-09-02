---
name: query
description: 从本地 LLM Wiki Vault 检索并回答关于笔记、历史记录、知识库和过往决策的问题。必须先读取 wiki/index.md，再读取相关页面并用 Obsidian 双链引用；知识库没有答案时要明确说明。
allowed-tools: Read, Glob, Grep
user-invocable: true
---

# LLM Wiki 知识检索

回答涉及本地笔记、知识库、历史决定或 Vault 内容的问题时，优先使用本地 Wiki，不凭模型记忆代替检索。

## 工作流程

1. 定位当前 Vault 根目录。
2. 第一时间完整读取 `wiki/index.md`。
3. 从 Sources、Entities、Concepts、Syntheses 中选择与问题最相关的页面。
4. 完整读取相关页面，并沿着必要的 `[[wikilink]]` 继续查阅。
5. 综合页面内容回答，并在引用 Wiki 信息的位置标注 `[[页面名称]]`。
6. 如果本地 Wiki 没有相关内容，明确写出：`本地知识库中未找到相关内容，以下为通用知识回答：`，再回答通用部分。

## 引用规范

- 使用 `[[页面名称]]` 标记 Wiki 页面来源。
- 不要把没有读取过的页面当作依据。
- 引用原始资料中的具体句子时，使用简短 Markdown 引用并注明来源页面。
- 不要为了增加引用而重复同一页面；一个段落通常引用一次即可。

## 高价值回答

如果回答具有明显的总结、比较或研究价值，先询问用户是否保存到 `wiki/syntheses/`。得到同意后，创建带有 `type: synthesis` 的页面，并更新 `wiki/index.md` 与 `wiki/log.md`。

## 日志

查询完成后，在 `wiki/log.md` 末尾追加：

```markdown
## [YYYY-MM-DD] query | 操作简述
- **输出**: [[引用页面]] 或即时回答未保存
```

## 硬约束

- 必须先读 `wiki/index.md`。
- 禁止仅凭模型记忆回答本地知识问题。
- 不要为了查询修改或删除 Wiki 页面。
- 知识库没有答案时必须明确降级说明。
