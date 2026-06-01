# AI 应用 Docker 部署实践

## 多容器编排

```yaml
services:
  nginx:       # 反向代理 + 静态资源
  frontend:    # Next.js 14
  backend:     # FastAPI
  postgres:    # PostgreSQL 16 + pgvector
  redis:       # Redis 7
  ollama:      # 可选：本地 LLM
```

## SSE 流式传输配置

```nginx
location /api/ {
    proxy_pass http://backend:8000;
    proxy_buffering off;       # 关键：关闭缓冲
    proxy_cache off;
    proxy_set_header X-Accel-Buffering no;
}
```

## 镜像优化

- 前端：多阶段构建（deps → build → runner）
- 后端：Python slim 基础镜像 + pip --no-cache-dir
- 减小编译上下文（.dockerignore）

## 环境变量管理

- .env 文件存敏感信息（不入 git）
- .env.example 提交模板
- Docker Compose --env-file 指定环境
