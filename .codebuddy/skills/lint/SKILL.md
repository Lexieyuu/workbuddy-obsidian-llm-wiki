---
name: lint
description: 只读检查 LLM Wiki 的死链、孤儿页、索引、Schema 和知识冲突；支持 /lint、/scan 和 /health。
allowed-tools: Read, Glob, Grep
user-invocable: true
---

# LLM Wiki 健康检查

把 Wiki 当作需要静态检查的知识图谱，发现结构问题和认知技术债。目录、Schema、权限和日志规则统一遵循 `CODEBUDDY.md`，本 Skill 不重复定义。

## 工作流程

1. 定位 Vault 根目录，只检查 `wiki/`；不读取 `raw/09-archive/`。
2. 读取 `wiki/index.md`，提取其中的 `[[页面名称]]`。
3. 扫描 `wiki/` 下 Markdown 页面，排除 `index.md` 和 `log.md`。
4. 检查索引一致性：文件未登记、索引目标不存在或分类不一致。
5. 扫描 Wiki 页面中的 `[[双链]]`，标记不存在的目标；统计被引用次数并报告孤儿页，排除自引用。
6. 按 `CODEBUDDY.md` 检查 frontmatter、`type`、`status`、`sources` 和 Source 元数据是否合规。
7. 搜索 `## 知识冲突`，报告未解决冲突以及“我的标注/我的理解/AI 分析”是否混用。

## 输出

输出健康报告，按绿灯、黄灯、红灯列出问题、页面路径和建议；报告本身不写入仓库。

## 只读规则

本 Skill 默认只读：禁止修改、删除或重命名任何文件，也不追加 `wiki/log.md`。只有用户随后明确确认修复，执行修复的流程才可按 `CODEBUDDY.md` 记录实际变更；如果没有修改，就绝不写 Log。
