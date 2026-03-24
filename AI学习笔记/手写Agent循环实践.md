# 手写 Agent 循环实践

## Agent 循环核心

```java
public String run(String userInput) {
    addToMemory(user, userInput);
    
    for (int i = 0; i < maxIterations; i++) {
        // 1. 构建 Prompt（系统提示 + 工具列表 + 对话历史）
        String prompt = buildPrompt();
        
        // 2. 调用 LLM
        String response = llm.chat(prompt);
        
        // 3. 解析意图：直接回复 or 调用工具？
        if (isToolCall(response)) {
            ToolCall call = parseToolCall(response);
            String result = executeTool(call);
            addToMemory(tool, result);
        } else {
            addToMemory(assistant, response);
            return response; // 任务完成
        }
    }
    
    return 达到最大迭代次数，任务未完成;
}
```

## 关键设计点

1. 循环终止条件：最大迭代次数 + 任务完成信号
2. 工具执行容错：try-catch → 错误信息回传 LLM
3. 上下文窗口管理：历史过长时压缩或截断
4. 避免死循环：连续相同工具调用超 3 次 → 中断

## 为什么手写

- 理解 Agent 底层原理（框架替你做了什么）
- 可控性强（每个环节自定义）
- 无框架依赖（轻量、易调试）
