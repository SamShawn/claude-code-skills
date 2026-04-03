---
name: Format Code
description: This skill should be used when the user asks to "format code", "lint", "style", "code style", "prettier", "black", "eslint", or needs to format code according to best practices.
version: 1.0.0
---

# Format Code Skill

根据语言最佳实践格式化代码，包括缩进、空格、命名规范、导入排序等。支持多种格式化工具的配置输出。

## 适用场景

- 代码格式化
- 统一团队风格
- 自动修复简单问题
- 生成配置文件

## 格式化规则

### Python (Black + isort)
```python
# 格式化前
def  test(a,b):
    result=[]
    for i in range(len(a)):
        if a[i]>b:
            result.append(a[i])
    return result

# 格式化后
def test(a: list, b: int) -> list:
    result = []
    for i in range(len(a)):
        if a[i] > b:
            result.append(a[i])
    return result
```

### JavaScript/TypeScript (Prettier + ESLint)
```javascript
// 格式化前
function  test(a,b){
const  result=[];
for(let i=0;i<a.length;i++){
if(a[i]>b){
result.push(a[i]);}}
return result;
}

// 格式化后
const test = (a: number[], b: number): number[] => {
  const result: number[] = [];
  for (let i = 0; i < a.length; i++) {
    if (a[i] > b) {
      result.push(a[i]);
    }
  }
  return result;
};
```

### Go (gofmt)
```go
// 格式化后
func process(data []int) []int {
    result := make([]int, 0, len(data))
    for _, v := range data {
        if v > 0 {
            result = append(result, v)
        }
    }
    return result
}
```

## 配置文件输出

### ESLint (.eslintrc.json)
```json
{
  "rules": {
    "semi": ["error", "always"],
    "quotes": ["error", "single"],
    "indent": ["error", 2],
    "no-unused-vars": "error",
    "no-console": "warn"
  }
}
```

### Prettier (.prettierrc)
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5"
}
```

### Black (pyproject.toml)
```toml
[tool.black]
line-length = 88
target-version = ['py38']
include = '\.pyi?$'
```

## 输出格式

```
格式化建议（JavaScript/ESLint + Prettier）
────────────────────────────────────────
问题识别：
1. 函数名与参数缺少空格
2. const/let 声明缺少空格
3. 循环和条件语句缺少空格
4. 大括号位置不符合规范

────────────────────────────────────────
格式化后代码：
[代码]

配置文件建议：
[配置文件内容]
```

## 相关技能

- **refactor** - 代码重构
- **cleanup** - 代码清理
- **generate-ci** - CI/CD 配置