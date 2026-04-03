---
name: Add Comments
description: This skill should be used when the user asks to "add comments", "add documentation", "add docstring", "comment code", "document function", or needs to add comments or docstrings to existing code.
version: 1.0.0
---

# Add Comments Skill

为代码添加规范的注释，包括文件头注释、函数说明、参数说明、返回值说明。遵循语言特定的注释规范（JSDoc、Google Style、NumPy Style 等）。

## 适用场景

- 遗留代码补充注释
- 规范代码文档
- API 文档前置工作
- 代码自文档化

## 注释风格

### Python (Google Style)
```python
def process_orders(orders: List[Dict], customer_id: int,
                   status: Optional[str] = None) -> List[Dict]:
    """过滤符合条件的企业订单。

    根据客户 ID 和订单状态筛选订单列表。

    Args:
        orders: 订单列表，每个订单包含 customer_id、status 等字段
        customer_id: 客户 ID，用于过滤指定客户的订单
        status: 订单状态过滤条件，默认为 None（不限制状态）

    Returns:
        符合过滤条件的订单列表

    Raises:
        ValueError: 当 customer_id 小于 0 时

    Example:
        >>> orders = [
        ...     {'customer_id': 1, 'status': 'completed'},
        ...     {'customer_id': 2, 'status': 'pending'}
        ... ]
        >>> process_orders(orders, 1, 'completed')
        [{'customer_id': 1, 'status': 'completed'}]
    """
```

### Python (NumPy Style)
```python
def process_orders(orders, customer_id, status=None):
    """
    Filter enterprise orders by customer ID and status.

    Parameters
    ----------
    orders : list
        Order list containing customer_id and status fields.
    customer_id : int
        Customer ID to filter orders.
    status : str, optional
        Order status filter, None means no restriction.

    Returns
    -------
    list
        Filtered order list.

    Raises
    ------
    ValueError
        When customer_id is less than 0.
    """
```

### JavaScript (JSDoc)
```javascript
/**
 * Filter orders by customer ID and status.
 * @param {Array} orders - Order list
 * @param {number} customerId - Customer ID
 * @param {string|null} status - Order status filter
 * @returns {Array} Filtered orders
 * @throws {Error} When customerId is invalid
 * @example
 * const orders = filterOrders(data, 1, 'completed');
 */
function filterOrders(orders, customerId, status) { ... }
```

### TypeScript
```typescript
/**
 * 用户服务接口
 */
interface UserService {
  /**
   * 根据用户 ID 获取用户信息
   * @param id - 用户 ID
   * @returns 用户对象或 null
   */
  getUser(id: number): Promise<User | null>;
}
```

## 输出格式

```
注释生成（Google Style）
────────────────────────────────────────

def process_orders(...):
    """函数说明。

    更详细的说明。

    Args:
        参数说明

    Returns:
        返回值说明

    Raises:
        异常说明

    Example:
        示例代码
    """
```

## 注释原则

1. **必要性**：注释解释"为什么"而非"是什么"
2. **清晰性**：使用简洁、易懂的语言
3. **一致性**：保持注释风格统一
4. **更新性**：代码修改时同步更新注释
5. **避免过度**：不在显而易见的代码上添加注释

## 相关技能

- **generate-docs** - 文档生成
- **generate-test** - 测试生成
- **format** - 代码格式化