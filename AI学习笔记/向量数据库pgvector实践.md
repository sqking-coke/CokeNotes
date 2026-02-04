# 向量数据库 pgvector 实践

## 为什么选 pgvector

- PostgreSQL 原生扩展，和业务数据库统一
- 支持 HNSW 索引（近似最近邻搜索）
- 支持余弦距离、欧氏距离、内积
- 运维成熟，SQL 生态完整

## HNSW 索引配置

```sql
CREATE INDEX ON kb_chunks
  USING hnsw (embedding vector_cosine_ops)
  WITH (m = 16, ef_construction = 200);
```

- m：每个节点的连接数（越大越精确，内存越多）
- ef_construction：构建时的搜索宽度（越大越精确，构建越慢）

## 查询示例

```sql
SELECT content, 1 - (embedding <=> :query_vec) AS similarity
FROM kb_chunks
WHERE 1 - (embedding <=> :query_vec) >= 0.65
ORDER BY embedding <=> :query_vec
LIMIT 5;
```

## 与专用向量库对比

| | pgvector | Milvus | Chroma |
|---|:--:|:--:|:--:|
| 部署复杂度 | ⭐ 低 | ⭐⭐⭐ 高 | ⭐ 低 |
| 亿级规模 | 🟡 | 🟢 | 🔴 |
| SQL 兼容 | 🟢 | 🔴 | 🔴 |
| 十万级 | 🟢 | 🟢 | 🟢 |

### IVFFlat vs HNSW\n| | IVFFlat | HNSW |\n|------|:--:|:--:|\n| 构建速度 | 快 | 慢 |\n| 查询速度 | 中等 | 快 |\n| 召回率 | 高 | 高(可调) |\n| 内存占用 | 低 | 高 |\nHNSW 适合查询密集型，IVFFlat 适合存储敏感型。
