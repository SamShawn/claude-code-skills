---
name: Generate Test
description: This skill should be used when the user asks to "generate test", "write test", "create test", "unit test", "add tests", "test coverage", or needs to generate unit tests for existing code.
version: 1.0.0
---

# Generate Test Skill

为指定代码生成完整的单元测试，覆盖正常路径、边界条件、异常情况。支持 pytest、unittest、Jest、Mocha 等主流测试框架。

## 适用场景

- 新功能测试覆盖
- 回归测试保护
- TDD 实践
- 补充缺失测试

## 支持的测试框架

### Python
- pytest (推荐)
- unittest
- nose2

### JavaScript/TypeScript
- Jest (推荐)
- Mocha
- Vitest

### 其他
- Go: testing, testify
- Java: JUnit, TestNG
- Rust: #[test], mockall

## 测试覆盖策略

### 正常路径测试
```python
def test_normal_division(self):
    """正常除法测试"""
    assert divide(10, 2) == 5.0
    assert divide(9, 3) == 3.0
    assert divide(7, 2) == 3.5
```

### 边界条件测试
```python
def test_boundary_cases(self):
    """边界条件测试"""
    assert divide(0, 5) == 0.0  # 分子为零
    assert divide(-10, 2) == -5.0  # 负数
    assert divide(1, 3)  # 小数结果
```

### 异常情况测试
```python
def test_divide_by_zero_raises(self):
    """除零异常测试"""
    with pytest.raises(ValueError, match="Cannot divide by zero"):
        divide(10, 0)
```

## 输出格式

```
单元测试生成（pytest）
────────────────────────────────────────
测试文件：test_divide.py

import pytest
from my_module import divide

class TestDivide:
    """divide 函数的单元测试"""

    def test_normal_division(self):
        """正常除法测试"""
        assert divide(10, 2) == 5.0
        # ... more tests

────────────────────────────────────────
覆盖率预期：
- 分支覆盖：100%
- 边界条件：全部覆盖
- 执行命令：pytest test_divide.py -v --cov
```

## 测试命名规范

- 使用描述性名称：`test_function_name_scenario`
- 遵循 AAA 模式：Arrange, Act, Assert
- 每个测试函数只测试一个场景

## 相关技能

- **generate-code** - 代码生成
- **explain-code** - 代码解释
- **generate-ci** - CI/CD 配置