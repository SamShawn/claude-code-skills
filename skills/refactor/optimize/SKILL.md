---
name: Refactor Code
description: This skill should be used when the user asks to "refactor", "refactor code", "optimize code", "improve code", "clean up code", or needs to restructure existing code for better quality.
version: 1.0.0
---

# Refactor Code Skill

对代码进行重构优化，包括提取重复代码、简化复杂逻辑、改善命名、增强可读性。提供优化前后的对比说明。

## 适用场景

- 清理技术债务
- 提升代码质量
- 改善可维护性
- 标准化代码风格

## 重构技术

### 提取函数 (Extract Function)
将长函数中的代码块提取为独立函数：
```python
# 重构前
def process_order(order):
    # 验证订单
    if order.status != 'valid':
        return False
    # 计算价格
    total = sum(item.price for item in order.items)
    # ...
    return True

# 重构后
def validate_order(order):
    return order.status == 'valid'

def calculate_total(order):
    return sum(item.price for item in order.items)
```

### 早期返回 (Guard Clauses)
减少嵌套层级：
```python
# 重构前
def process(data):
    if data:
        if data.is_valid:
            result = do_something(data)
            return result
        else:
            return None
    else:
        return None

# 重构后
def process(data):
    if not data:
        return None
    if not data.is_valid:
        return None
    return do_something(data)
```

### 重命名 (Rename)
使用描述性命名：
- `d` → `data` 或 `details`
- `process1` → `validate_input`
- `tmp` → `temporary_file`

### 合并条件 (Consolidate Conditions)
简化复杂的条件判断：
```python
# 重构前
if status == 'active':
    if not archived:
        do_something()

# 重构后
if status == 'active' and not archived:
    do_something()
```

## 输出格式

```
重构分析
────────────────────────────────────────
原始问题：
1. 嵌套层级过深（4 层）
2. 函数名不具描述性
3. 逻辑分散难以理解
4. 缺少类型提示

重构方案：使用早期返回 + 提取函数

────────────────────────────────────────
重构后代码：
[代码]

改进点：
✓ 嵌套深度：4 层 → 2 层
✓ 函数名：描述性强
✓ 可测试性：提取的函数可独立测试
```

## 相关技能

- **analyze-complexity** - 复杂度分析
- **cleanup** - 代码清理
- **format** - 代码格式化