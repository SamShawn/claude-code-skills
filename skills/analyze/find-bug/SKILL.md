---
name: Find Bug
description: This skill should be used when the user asks to "find bug", "debug", "fix error", "why is this broken", "this doesn't work", "bug analysis", "error analysis", or needs help identifying and fixing code issues.
version: 1.0.0
---

# Find Bug Skill

分析代码中的潜在 bug，解释问题原因，并提供修复方案。能够识别空指针、越界访问、竞态条件、逻辑错误等常见问题。

## 适用场景

- 调试未知代码
- 分析报错信息
- 预防性检查
- 代码审查

## 常见 Bug 类型

### 数组/索引问题
- 数组越界（`i <= length` 应为 `i < length`）
- 空数组访问
- 负数索引

### 空值问题
- 空指针引用（NullPointerException）
- 未初始化的变量
- 可选类型未检查

### 逻辑错误
- 运算符错误（`==` vs `=`）
- 条件判断错误
- 循环边界错误

### 并发问题
- 竞态条件
- 死锁
- 线程不安全

### 资源泄漏
- 未关闭的文件/连接
- 内存泄漏
- 资源未释放

## 输出格式

```
Bug 分析报告
────────────────────────────────────────
🐛 问题 1：数组越界访问
位置：第 2 行 for 循环条件
问题：i <= users.length 会导致最后访问 users[users.length]
      （数组最大索引为 length-1）

原因：循环条件使用 <= 而非 <

修复方案：
将 i <= users.length 改为 i < users.length

修复后代码：
────────────────────────────────────────
function findUser(users, id) {
    for (let i = 0; i < users.length; i++) {
        if (users[i].id === id) {
            return users[i];
        }
    }
    return null;
}
```

## 分析流程

1. 阅读代码并理解逻辑
2. 识别可能导致错误的代码模式
3. 分析问题原因和影响范围
4. 提供修复方案
5. 展示修复后的代码

## 相关技能

- **explain-code** - 代码解释
- **analyze-complexity** - 复杂度分析
- **refactor** - 代码重构