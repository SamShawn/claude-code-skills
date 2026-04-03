---
name: Explain Code
description: This skill should be used when the user asks to "explain code", "explain", "what does this do", "line by line", "break down", "understand code", or needs detailed explanation of code functionality.
version: 1.0.0
---

# Explain Code Skill

Provide detailed line-by-line or function-by-function analysis of code, explaining its purpose, execution flow, and key logic.

## When to Use

Use this skill when user needs:
- Understand unfamiliar code
- Code review feedback
- Debug logic errors
- Learn from existing code
- Explain complex algorithms

## Analysis Levels

### Line-by-Line
Break down each line or logical group:
```
Line 1: def function_name(param):
    Define a function with one parameter

Line 2: if len(arr) <= 1:
    Check if array is empty or has one element (base case)
```

### Function-Level
Explain each function's:
- Purpose
- Parameters
- Return value
- Side effects

### Module-Level
Explain:
- Class/structure relationships
- Data flow
- Public APIs
- Dependencies

## Complexity Analysis

For algorithms, provide:
- Time complexity (Big O)
- Space complexity (Big O)
- Best/average/worst cases

## Output Format

```
逐行解析：
────────────────────────────────────────
第 1 行：code
    解释内容

复杂度分析：
- 时间复杂度：O(n log n)
- 空间复杂度：O(n)
```

## Best Practices

1. Match explanation level to code complexity
2. Highlight edge cases and gotchas
3. Explain "why" not just "what"
4. Provide context on language features used
5. Suggest improvements if applicable

## Related Skills

- **analyze-complexity** - For complexity metrics
- **find-bug** - For bug detection