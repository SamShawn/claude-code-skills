---
name: Generate Code
description: This skill should be used when the user asks to "generate code", "write code", "implement function", "create class", "build [feature]", "write [language] code", or needs code for a specific programming task.
version: 1.0.0
---

# Generate Code Skill

Generate complete, production-ready code based on user requirements. Support multiple programming languages including Python, JavaScript, TypeScript, Go, Rust, Java, and more.

## When to Use

Use this skill when user needs:
- Quick project scaffolding
- Function/class/module implementation
- Specific feature code snippets
- Learning examples for new technologies

## Supported Languages

- Python, JavaScript, TypeScript
- Go, Rust, Java, C/C++
- Ruby, PHP, Swift, Kotlin
- Shell, SQL, and more

## Code Generation Guidelines

### Function Structure
```
def function_name(param: Type) -> ReturnType:
    """Docstring describing purpose."""
    # Implementation
    return result
```

### Class Structure
```python
class ClassName:
    """Class description."""

    def __init__(self, param: Type):
        self.param = param

    def method(self) -> ReturnType:
        """Method description."""
        return result
```

### Best Practices

1. Include type hints where applicable
2. Add docstrings with Args/Returns/Raises
3. Handle edge cases and errors
4. Follow language-specific conventions
5. Include necessary imports

## Output Format

Provide:
1. Complete, runnable code
2. Usage example
3. Dependencies if needed
4. Configuration requirements

## Related Skills

- **generate-project** - For full project structure
- **generate-template** - For design patterns
- **generate-test** - For unit tests