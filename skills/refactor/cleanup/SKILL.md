---
name: Cleanup Code
description: This skill should be used when the user asks to "cleanup", "clean up code", "remove dead code", "remove unused", "code cleanup", or needs to remove unused code, imports, or comments.
version: 1.0.0
---

# Cleanup Code Skill

移除死代码、未使用的导入、重复代码、过时注释。生成清洁的代码版本并说明清理内容。

## 适用场景

- 项目维护
- 代码审查前清理
- 减小代码体积
- 移除遗留代码

## 清理项目

### 未使用的导入
```python
# 移除
import os  # 未使用
from typing import List, Dict, Optional  # Optional 未使用
```

### 未使用的函数/变量
```python
# 移除
def old_function():  # 未使用
    pass

x = 1
y = 2
z = 3  # 未使用的变量
```

### 死代码
```python
# 移除
if False:  # 永远不执行的代码
    debug_log()

# 移除
def unused_helper():
    pass  # 从未被调用
```

### 过时注释
```python
# 移除
# TODO: implement later (已过期)
# This function was used for debugging
# Removed on 2023-01-01
```

### 重复代码
识别并提取重复代码为共享函数：
```python
# 重构前
def process_a():
    # 相似逻辑
    result = validate(input_a)
    save(result)
    return result

def process_b():
    # 相同逻辑
    result = validate(input_b)
    save(result)
    return result

# 重构后
def process_common(input_data):
    result = validate(input_data)
    save(result)
    return result
```

## 输出格式

```
代码清理报告
────────────────────────────────────────
文件：sample.py

已清理项目：

🗑 移除未使用导入：
   - typing.Optional

🗑 移除未使用函数：
   - old_function()

🗑 移除未使用变量：
   - z (第 18 行)

🗑 移除过时注释：
   - 调试相关注释

────────────────────────────────────────
清理后代码：
[代码]

统计：
- 移除代码行数：12 行
- 移除注释行数：5 行
- 代码精简率：约 30%
```

## 注意事项

1. 先备份原始代码
2. 确认代码确实未使用再删除
3. 注意是否有条件编译相关代码
4. 移除前验证测试仍然通过

## 相关技能

- **refactor** - 代码重构
- **format** - 代码格式化
- **cleanup** - 代码清理