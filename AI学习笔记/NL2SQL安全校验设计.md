# NL2SQL 安全校验设计

## 为什么 NL2SQL 需要安全校验

LLM 生成的 SQL 不可信——可能包含：
- 恶意注入：DROP TABLE、DELETE FROM
- 危险操作：ALTER、TRUNCATE
- 数据泄露：SELECT * FROM users WHERE ...

## 三道安全防线

### 第一道：语法白名单
只允许 SELECT 语句，禁止 DDL/DML：
```java
String upper = sql.toUpperCase().trim();
if (upper.startsWith(DROP) || upper.startsWith(DELETE)
    || upper.startsWith(ALTER) || upper.startsWith(TRUNCATE)) {
    throw new SecurityException(禁止的操作);
}
```

### 第二道：权限控制
- 使用只读数据库账号
- 限制可查询的表和字段
- 敏感字段自动脱敏

### 第三道：结果校验
- 返回行数超限 → 截断并提示
- 查询耗时超时 → 中断并降级
- 结果为空 → ReAct 修正或告知用户

## ReAct 自纠错
```
SQL 执行失败 → 错误信息回传 LLM → 修正 SQL → 重新执行
```
