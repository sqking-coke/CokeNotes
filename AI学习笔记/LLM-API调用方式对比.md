# LLM API 调用方式对比

## 三种主流方式

### 1. 原生 HTTP 调用
- 直接调用 OpenAI/DeepSeek REST API
- Java: OkHttp / Hutool
- Python: requests / httpx
- 优点：零依赖，完全可控
- 缺点：需手动处理流式、重试、超时

### 2. 官方 SDK
- OpenAI Java SDK / Python openai
- 封装了认证、流式、错误处理
- 适合快速接入单一厂商

### 3. AI 框架
- LangChain4j (Java) / LangChain (Python)
- 统一多厂商接口
- 提供 Agent、RAG、Memory 等高级抽象

## 选型建议

| 场景 | 推荐 |
|------|------|
| 学习原理 | 原生 HTTP |
| 生产单厂商 | 官方 SDK |
| 多厂商/复杂 Agent | LangChain4j / LangChain
