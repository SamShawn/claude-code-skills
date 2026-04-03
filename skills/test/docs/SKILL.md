---
name: Generate Docs
description: This skill should be used when the user asks to "generate docs", "create docs", "API docs", "documentation", "JSDoc", "docstring", or needs to generate documentation for code.
version: 1.0.0
---

# Generate Docs Skill

为代码生成 API 文档，包括函数说明、参数类型、返回值、异常处理、使用示例。支持 Markdown、OpenAPI、Swagger、JSDoc 等格式。

## 适用场景

- 项目文档生成
- API 规范输出
- 代码自文档化
- 生成开发者文档

## 文档格式

### Markdown
```markdown
# UserService 类

用户管理服务，提供用户的增删改查功能。

## 方法

### getUser

根据用户 ID 获取用户信息。

**参数：**
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| id | number | 是 | 用户 ID |

**返回值：** Promise<User | null>

**示例：**
const user = await userService.getUser(123);
```

### JSDoc
```javascript
/**
 * 根据用户 ID 获取用户信息。
 * @param {number} id - 用户 ID
 * @returns {Promise<User|null>} 找到的用户对象或 null
 * @throws {Error} 用户不存在
 * @example
 * const user = await userService.getUser(123);
 */
async getUser(id) { ... }
```

### OpenAPI/Swagger
```yaml
paths:
  /users/{id}:
    get:
      summary: 获取用户信息
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
```

## 输出格式

```
API 文档生成
────────────────────────────────────────
# UserService 类

用户管理服务，提供用户的增删改查功能。

## 方法

### getUser

[详细文档内容]

────────────────────────────────────────
```

## 文档规范

1. 每个公开接口都需要文档
2. 参数说明要包含类型和用途
3. 返回值说明可能的类型
4. 异常情况需要说明抛出的错误
5. 提供使用示例
6. 使用语言特定的注释风格

## 相关技能

- **add-comments** - 代码注释
- **generate-test** - 测试生成
- **generate-code** - 代码生成