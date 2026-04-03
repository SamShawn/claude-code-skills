---
name: Security Scan
description: This skill should be used when the user asks to "security scan", "vulnerability scan", "secure code", "security audit", "SQL injection", "XSS", "hardcode password", or needs to check code for security issues.
version: 1.0.0
---

# Security Scan Skill

扫描代码中的安全漏洞，包括 SQL 注入、XSS、命令注入、硬编码密码、敏感信息泄露等。提供修复建议和安全最佳实践。

## 适用场景

- 安全审计
- 代码审查
- 上线前检查
- 漏洞修复

## 常见漏洞类型

### SQL 注入
```python
# 危险
query = f"SELECT * FROM users WHERE id = {user_id}"
cursor.execute(query)

# 安全
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```

### XSS (跨站脚本)
```javascript
// 危险
element.innerHTML = userInput;

// 安全
element.textContent = userInput;
```

### 命令注入
```python
# 危险
os.system(f"ping {user_input}")

# 安全
subprocess.run(["ping", user_input], shell=False)
```

### 硬编码密码
```python
# 危险
PASSWORD = "secret123"
API_KEY = "sk-xxx"

# 安全
import os
API_KEY = os.environ.get("API_KEY")
```

### 不安全的随机数
```python
# 危险
random.random()  # 可预测

# 安全
import secrets
secrets.token_hex(16)
```

## 输出格式

```
安全扫描报告
────────────────────────────────────────
文件：app.py

🔴 严重漏洞：

1. SQL 注入漏洞（第 12 行）
   问题：使用字符串格式化拼接 SQL 查询
   代码：query = f"SELECT * FROM users WHERE id = {user_id}"
   风险：攻击者可注入恶意 SQL 语句
   修复：使用参数化查询
   ─────────────────────────────────
   修复代码：
   query = "SELECT * FROM users WHERE id = ?"
   cursor.execute(query, (user_id,))

🟡 中等风险：

2. 敏感信息泄露（第 3 行）
   问题：数据库连接未加密，路径硬编码
   风险：数据库文件暴露敏感数据
   修复：使用环境变量配置数据库路径

🟢 安全建议：

4. 建议添加速率限制防止暴力破解
5. 建议使用 HTTPS 加密通信

────────────────────────────────────────
扫描统计：
- 严重漏洞：1
- 中等风险：2
- 建议：3
- 整体评分：D（存在严重漏洞）
```

## 修复优先级

1. **🔴 严重**：立即修复（SQL注入、命令注入等）
2. **🟡 中等**：尽快修复（硬编码、弱加密等）
3. **🟢 建议**：考虑改进（添加日志、监控等）

## 相关技能

- **analyze-performance** - 性能分析
- **generate-ci** - CI/CD 配置
- **refactor** - 代码重构