# Prompt Engineering 实践笔记

## System Prompt vs User Prompt

- System Prompt：设定角色、约束、输出格式（持久）
- User Prompt：具体任务描述（每次变化）

## 常用技巧

### Few-shot Prompting
提供 2-3 个示例，让模型理解期望的输出格式。

### Chain of Thought
在 Prompt 中要求模型「一步步思考」，显著提升推理准确率。

### 模板分流注入
不同知识类型注入 Prompt 的不同位置：
- 规范/标准 → Prompt 前段（告知规则）
- 案例/模式 → Prompt 中段（提供参考）
- 最佳实践 → Prompt 后段（优化建议）

### 输出格式控制
- JSON 结构化输出： + 明确 Schema
- Markdown 格式化：标题、列表、代码块

## 避坑
- Prompt 太长 → LLM 注意力分散
- 指令冲突 → System 和 User Prompt 不要矛盾
- 过于抽象 → 给具体示例比抽象描述更有效
