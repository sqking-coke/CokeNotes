# Java AI Agent 开发环境搭建

## 技术栈选型

| 组件 | 选择 | 原因 |
|------|------|------|
| 后端框架 | SpringBoot 3.5 | 生态成熟，企业标准 |
| ORM | MyBatis-Plus | 灵活，复杂查询友好 |
| AI 框架 | LangChain4j | Java 版 LangChain |
| LLM | DeepSeek V4 | 性价比高，中文好 |
| 向量库 | pgvector | 与业务 DB 统一 |
| 数据库 | MySQL 8.0 / PostgreSQL 16 | — |

## 项目初始化

```xml
<!-- pom.xml 核心依赖 -->
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j</artifactId>
    <version>1.0.0-beta2</version>
</dependency>
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-open-ai</artifactId>
    <version>1.0.0-beta2</version>
</dependency>
```

## LLM 配置

```yaml
llm:
  provider: deepseek
  api-url: https://api.deepseek.com/v1/chat/completions
  model: deepseek-v4-flash
  temperature: 0.1
  max-tokens: 4096
```
