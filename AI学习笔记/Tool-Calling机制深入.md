# Tool Calling 机制深入

## 什么是 Tool Calling

LLM 本身只能生成文本。Tool Calling 让 LLM 能够：
- 查询数据库
- 调用外部 API
- 执行计算
- 操作文件系统

## 实现原理

1. 定义工具（函数签名 + 描述）
2. 将工具 Schema 注入 System Prompt
3. LLM 决策：是直接回答还是调用工具？
4. 如果调用工具 → 返回函数名+参数 JSON
5. 程序执行工具 → 结果回传给 LLM
6. LLM 基于结果生成最终回答

## LangChain4j @Tool 注解

```java
@Tool(查询指定城市的天气)
public String getWeather(
    @P(城市名称) String city,
    @P(日期) String date) {
    return weatherApi.query(city, date);
}
```

LLM 自动看到工具描述和参数说明，自主决定何时调用。

## 安全注意事项

- 工具执行前校验参数（防注入、防越权）
- 限制工具调用次数（防死循环）
- 危险操作需要人工确认
