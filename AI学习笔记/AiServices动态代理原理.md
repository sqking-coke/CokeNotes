# AiServices 动态代理原理

## 什么是 AiServices

LangChain4j 的核心抽象——通过接口定义 Agent 行为，框架自动生成实现。

```java
interface CodeReviewer {
    @SystemMessage(你是代码审查专家，审查以下代码...)
    ReviewResult review(String code);
}

// 框架生成代理对象
CodeReviewer reviewer = AiServices.builder(CodeReviewer.class)
    .chatLanguageModel(model)
    .tools(new CodeAnalysisTool())
    .chatMemory(MessageWindowChatMemory.withMaxMessages(10))
    .build();
```

## 动态代理做了什么

1. 解析接口方法的注解（@SystemMessage, @UserMessage）
2. 构建 Prompt 模板
3. 调用 LLM
4. 解析返回结果为接口定义的返回类型
5. 管理 ChatMemory 自动拼接历史

## 和手写版对比

| | 手写版 | AiServices |
|------|:--:|:--:|
| 代码量 | ~200 行 | ~50 行 |
| Prompt 构建 | 手动拼接 | 注解声明 |
| 对话记忆 | 手动管理 | 自动管理 |
| 工具注册 | 手动注入 | @Tool 自动发现 |

**AiServices 适合标准化 Agent，手写适合复杂自定义逻辑。**

### @Tool 参数校验\n@P 注解只是描述，不做校验。实际校验需要在工具方法内部完成：\n- 必填参数非空检查\n- 字符串长度限制\n- 数值范围校验
