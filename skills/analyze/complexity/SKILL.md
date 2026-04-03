---
name: Analyze Complexity
description: This skill should be used when the user asks to "analyze complexity", "code complexity", "cyclomatic complexity", "refactor suggestion", "code smell", or needs to evaluate code quality metrics.
version: 1.0.0
---

# Analyze Complexity Skill

分析代码的圈复杂度、嵌套深度、函数长度等指标，识别潜在的代码异味并提供重构建议。

## 适用场景

- 代码审查
- 技术债务评估
- 重构优先级排序
- 代码质量监控

## 分析指标

### 圈复杂度（Cyclomatic Complexity）
衡量代码路径的复杂度，值为决策点数量加一。
- 1-10：低复杂度，良好
- 11-20：中等复杂度，建议优化
- 21+：高复杂度，需要重构

### 嵌套深度（Nesting Depth）
代码块嵌套层数，建议不超过 3-4 层。

### 函数长度
建议函数不超过 30-50 行，过长的函数应该拆分。

### 代码行数统计
统计总行数、代码行数、注释行数、空白行数。

## 输出格式

```
复杂度分析报告
────────────────────────────────────────
文件：src/utils.py

函数复杂度：
┌──────────────────┬────────┬────────┬───────┐
│ 函数名           │ 圈复杂度│ 嵌套深度│ 行数  │
├──────────────────┼────────┼────────┼───────┤
│ process_data     │ 12     │ 5      │ 89    │
│ validate_input   │ 8      │ 3      │ 34    │
└──────────────────┴────────┴────────┴───────┘

代码异味识别：
⚠ process_data 函数圈复杂度 12，建议拆分为多个函数
⚠ validate_input 存在 3 层嵌套，建议使用早期返回

重构建议：
1. process_data：拆分成长度 < 30 行的多个函数
2. validate_input：使用 guard clauses 减少嵌套
```

## 重构建议类型

1. **提取函数**：将长函数拆分为多个小函数
2. **早期返回**：使用 guard clauses 减少嵌套
3. **合并条件**：简化复杂的条件判断
4. **移除重复**：提取重复代码为共享函数
5. **简化类**：将大类拆分为多个专注的小类

## 相关技能

- **refactor** - 代码重构优化
- **cleanup** - 代码清理
- **format** - 代码格式化