# AI Agent 可观测性方案

## 可观测性三支柱

| 支柱 | 工具 | 用途 |
|------|------|------|
| 日志 | 结构化日志 + traceId | 单次请求排查 |
| 指标 | Prometheus + Grafana | 趋势分析 + 告警 |
| 链路追踪 | traceId 全链路传递 | 分布式调用追踪 |

## 统一事件表设计

```sql
CREATE TABLE monitor_events (
    category VARCHAR(20),  -- routing/insight/concept/health/system
    event VARCHAR(50),     -- centroid_hit/llm_route_failed/...
    status VARCHAR(20),    -- success/failed/warning
    value_float FLOAT,     -- 距离/置信度/耗时
    extra_json JSONB,      -- 扩展详情
    resolved_at TIMESTAMPTZ -- 告警解除时间
);
```

## 关键监控指标

- 路由命中率（Layer 1 centroid 80-90%）
- 检索延迟 P50/P95
- LLM 调用成功率
- 知识沉淀转化率
- 健康度评分趋势

## 告警分级

| 级别 | 条件 | 响应 |
|:--:|------|------|
| P0 | LLM 服务不可用 | 立即通知 |
| P1 | 检索全部失败 | 5 分钟内响应 |
| P2 | 路由降级率上升 | 1 小时内排查 |
