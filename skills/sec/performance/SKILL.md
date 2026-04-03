---
name: Analyze Performance
description: This skill should be used when the user asks to "analyze performance", "performance optimization", "slow query", "bottleneck", "N+1", "memory leak", or needs to identify and fix performance issues in code.
version: 1.0.0
---

# Analyze Performance Skill

分析代码的性能瓶颈，识别低效算法、重复计算、内存泄漏、N+1 查询等问题。提供优化方案和预期收益。

## 适用场景

- 性能优化
- 慢查询分析
- 架构优化决策
- 资源使用分析

## 常见性能问题

### N+1 查询问题
```python
# 问题：循环内执行数据库查询
def get_user_with_posts(user_ids):
    users = []
    for user_id in user_ids:
        user = db.query("SELECT * FROM users WHERE id = ?", user_id)
        posts = db.query("SELECT * FROM posts WHERE user_id = ?", user_id)
        user['posts'] = posts
        users.append(user)
    return users

# 优化：使用 IN 子句批量查询
def get_user_with_posts(user_ids):
    users = db.query(
        "SELECT * FROM users WHERE id IN ({})".format(
            ','.join(['?'] * len(user_ids))
        ),
        user_ids
    )
    posts = db.query(
        "SELECT * FROM posts WHERE user_id IN ({})".format(
            ','.join(['?'] * len(user_ids))
        ),
        user_ids
    )
    # 内存中关联
    posts_by_user = {}
    for post in posts:
        posts_by_user.setdefault(post['user_id'], []).append(post)
    for user in users:
        user['posts'] = posts_by_user.get(user['id'], [])
    return users
```

### 重复计算
```python
# 问题：每次调用都重新计算
def get_full_name(user):
    return f"{user['first_name']} {user['last_name']}"

# 调用 1000 次 = 计算 1000 次
for user in users:
    print(get_full_name(user))

# 优化：计算一次或使用缓存
def get_full_name(user):
    if 'full_name' not in user:
        user['full_name'] = f"{user['first_name']} {user['last_name']}"
    return user['full_name']
```

### 内存泄漏
```python
# 问题：全局列表不断增长
cache = []

def add_to_cache(item):
    cache.append(item)  # 无限增长

# 优化：使用固定大小的缓存
from functools import lru_cache

@lru_cache(maxsize=1000)
def expensive_function(x):
    return compute(x)
```

## 输出格式

```
性能分析报告
────────────────────────────────────────
问题代码分析

🔴 N+1 查询问题（第 4-6 行）

问题描述：
循环内执行数据库查询，user_ids 有 n 个就会执行 2n+1 次查询

当前查询次数：2n + 1（n 为用户数量）
假设 user_ids = [1,2,3,4,5]
执行查询数：11 次

预期性能影响：
- 100 个用户：201 次查询 → 约 2000ms
- 1000 个用户：2001 次查询 → 约 20000ms

────────────────────────────────────────
优化方案：

方案 1：使用 IN 子句批量查询
────────────────────────────────────────
[优化代码]

优化后查询次数：2 次（固定）
性能提升：100n 倍（n=100 时）

────────────────────────────────────────
其他建议：

🟡 建议添加缓存层（Redis）
   预期提升：查询性能提升 10-100 倍

🟡 建议使用连接池
   预期提升：数据库连接建立时间减少 80%
```

## 分析维度

1. **时间复杂度**：算法效率
2. **空间复杂度**：内存使用
3. **I/O 优化**：数据库查询、网络请求
4. **缓存策略**：重复计算、热点数据
5. **并发能力**：多线程、异步处理

## 相关技能

- **security-scan** - 安全扫描
- **refactor** - 代码重构
- **generate-code** - 代码生成