# LangChain4j ChatModel 实践

## ChatLanguageModel 接口

```java
ChatLanguageModel model = OpenAiChatModel.builder()
    .baseUrl(https://api.deepseek.com/v1)
    .apiKey(System.getenv(DEEPSEEK_API_KEY))
    .modelName(deepseek-v4-flash)
    .temperature(0.1)
    .maxTokens(4096)
    .timeout(Duration.ofSeconds(120))
    .build();

String answer = model.chat(你好，介绍一下 RAG);
```

## 流式对话

```java
StreamingChatLanguageModel streamingModel = ...;
streamingModel.chat(..., new StreamingResponseHandler<>() {
    @Override
    public void onNext(String token) {
        System.out.print(token);  // 逐字输出
    }
    @Override
    public void onComplete(Response<T> response) {
        System.out.println(\n完成);
    }
});
```

## 多厂商切换

LangChain4j 统一了不同厂商的接口：
- OpenAI → OpenAiChatModel
- DeepSeek → 通过 OpenAI 兼容接口
- 通义千问 → QwenChatModel
- Ollama → OllamaChatModel

切换厂商只需改 builder，业务代码不变。
