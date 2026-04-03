# Claude Code Skills 工具集

> 面向编程、开发、调试、重构、自动化脚本的可复用代码能力工具箱

---

## 一、工具集简介

本工具集提供 15+ 个高质量通用编程技能，覆盖代码生成、调试、重构、性能、安全、自动化、文档等核心开发场景。每个技能均可独立调用，也可组合使用形成完整工作流。

**核心理念**：通过标准化指令快速调用 Claude Code 的特定能力，实现开发效率的量级提升。

---

## 二、分类目录

| 分类 | 描述 | 技能数量 |
|------|------|----------|
| 代码生成 | 根据需求生成各类代码骨架和实现 | 4 |
| 代码分析 | 解释、复杂度分析、bug 定位 | 3 |
| 代码重构 | 优化、清理、格式化、重写 | 3 |
| 测试与文档 | 单元测试生成、API 文档、注释 | 3 |
| 部署与运维 | Dockerfile、CI/CD、脚本生成 | 2 |
| 安全与性能 | 漏洞扫描、瓶颈分析、优化建议 | 2 |

---

## 三、技能详细说明

### 3.1 代码生成类

---

#### 技能 01：快速代码生成

**调用指令**：`/generate-code [语言] [功能描述]`

**功能说明**：根据指定编程语言和功能需求，生成完整的代码实现。涵盖函数、类、模块等基础结构，支持主流语言（Python、JavaScript、TypeScript、Go、Rust、Java 等）。

**适用场景**：

- 快速搭建项目骨架
- 实现特定功能的代码片段
- 学习新技术时的示例代码

**示例输入**：

```
/generate-code python
需求：实现一个异步 HTTP 请求工具类，支持 GET/POST 方法，包含超时控制和重试机制
```

**示例输出**：

```python
import asyncio
import aiohttp
from typing import Optional, Dict, Any

class AsyncHttpClient:
    """异步 HTTP 请求工具类"""

    def __init__(self, timeout: int = 30, max_retries: int = 3):
        self.timeout = aiohttp.ClientTimeout(total=timeout)
        self.max_retries = max_retries

    async def get(self, url: str, headers: Optional[Dict] = None) -> Dict[str, Any]:
        return await self._request("GET", url, headers=headers)

    async def post(self, url: str, data: Optional[Dict] = None,
                   headers: Optional[Dict] = None) -> Dict[str, Any]:
        return await self._request("POST", url, json=data, headers=headers)

    async def _request(self, method: str, url: str, **kwargs) -> Dict[str, Any]:
        for attempt in range(self.max_retries):
            try:
                async with aiohttp.ClientSession() as session:
                    async with session.request(method, url, timeout=self.timeout, **kwargs) as resp:
                        return {"status": resp.status, "data": await resp.json()}
            except Exception as e:
                if attempt == self.max_retries - 1:
                    raise
                await asyncio.sleep(2 ** attempt)
        return {"status": 0, "error": "Max retries exceeded"}
```

---

#### 技能 02：项目结构生成

**调用指令**：`/generate-project [项目类型] [语言]`

**功能说明**：生成完整的项目目录结构，包含配置文件、入口文件、依赖管理、测试目录等。适用于新项目初始化或现有项目的规范化重构。

**适用场景**：

- 初始化新项目
- 规范化项目结构
- 快速了解项目组织方式

**示例输入**：

```
/generate-project web-service python
```

**示例输出**：

```
my_project/
├── src/
│   ├── __init__.py
│   ├── main.py              # 应用入口
│   ├── config.py            # 配置管理
│   ├── models/              # 数据模型
│   ├── services/            # 业务逻辑
│   ├── routers/             # API 路由
│   └── utils/               # 工具函数
├── tests/                   # 测试目录
│   ├── unit/
│   └── integration/
├── scripts/                 # 运维脚本
├── docs/                    # 项目文档
├── .env.example             # 环境变量示例
├── requirements.txt         # Python 依赖
├── docker-compose.yml       # 容器编排
├── Dockerfile               # 容器镜像
└── README.md                # 项目说明
```

---

#### 技能 03：代码模板生成

**调用指令**：`/generate-template [模板类型]`

**功能说明**：生成常用代码模板，涵盖设计模式、算法实现、数据结构等。包括单例模式、工厂模式、观察者模式、LRU 缓存、限流器等。

**适用场景**：

- 学习设计模式
- 快速实现常见模式
- 代码规范化

**示例输入**：

```
/generate-template singleton python
```

**示例输出**：

```python
class Singleton:
    """单例模式实现（线程安全）"""
    _instance = None
    _lock = __import__('threading').Lock()

    def __new__(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        if not hasattr(self, '_initialized'):
            self._initialized = True
            # 初始化逻辑
```

---

#### 技能 04：脚本生成

**调用指令**：`/generate-script [脚本类型] [场景描述]`

**功能说明**：生成自动化脚本，包括数据处理、文件操作、定时任务、系统运维等。支持 Shell、Python、PowerShell 等脚本语言。

**适用场景**：

- 自动化重复任务
- 数据处理流水线
- 系统维护脚本

**示例输入**：

```
/generate-script shell
需求：批量重命名指定目录下所有 .png 文件，格式为 image_001.png, image_002.png
```

**示例输出**：

```bash
#!/bin/bash

# 批量重命名 PNG 文件
# 用法：./rename_png.sh <目录路径>

directory="${1:-.}"

cd "$directory" || exit 1

counter=1
for file in *.png; do
    if [ -f "$file" ]; then
        extension="${file##*.}"
        new_name=$(printf "image_%03d.%s" $counter "$extension")
        mv "$file" "$new_name"
        echo "Renamed: $file -> $new_name"
        ((counter++))
    fi
done

echo "完成！共重命名 $((counter - 1)) 个文件"
```

---

### 3.2 代码分析类

---

#### 技能 05：逐行代码解释

**调用指令**：`/explain-code [代码片段或文件路径]`

**功能说明**：对指定代码进行逐行或逐函数解析，说明每行代码的作用、执行流程、关键逻辑。对于复杂算法，提供时间/空间复杂度分析。

**适用场景**：

- 学习陌生代码
- 代码审查
- 排查逻辑错误

**示例输入**：

```python
# 代码片段
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)
```

**示例输出**：

```
逐行解析：
────────────────────────────────────────
第 1 行：def quicksort(arr):
    定义快速排序函数，接受一个列表参数 arr

第 2 行：if len(arr) <= 1:
    递归终止条件：列表为空或只有一个元素时无需排序

第 3 行：pivot = arr[len(arr) // 2]
    选择枢轴元素：取列表中间位置元素（避免最坏情况）

第 4-6 行：left = [x for x in arr if x < pivot]
           middle = [x for x in arr if x == pivot]
           right = [x for x in arr if x > pivot]
    列表推导式将数组分为三部分：小于、等于、大于枢轴的元素

第 7 行：return quicksort(left) + middle + quicksort(right)
    递归排序左右两部分，然后拼接结果

复杂度分析：
- 时间复杂度：平均 O(n log n)，最坏 O(n²)
- 空间复杂度：O(n) 用于存储分割后的列表
```

---

#### 技能 06：代码复杂度分析

**调用指令**：`/analyze-complexity [文件路径] [或代码片段]`

**功能说明**：分析代码的圈复杂度、嵌套深度、函数长度等指标，识别潜在的代码异味（Code Smell），提供重构建议。

**适用场景**：

- 代码审查
- 技术债务评估
- 重构优先级排序

**示例输入**：

```
/analyze-complexity src/utils.py
```

**示例输出**：

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
│ format_output    │ 3      │ 2      │ 21    │
└──────────────────┴────────┴────────┴───────┘

代码异味识别：
⚠ process_data 函数圈复杂度 12，建议拆分为多个函数
⚠ validate_input 存在 3 层嵌套，建议使用早期返回
⚠ validate_input 函数长度 34 行，超过推荐阈值 30 行

重构建议：
1. process_data：拆分成长度 < 30 行的多个函数
2. validate_input：使用 guard clauses 减少嵌套
3. 整体评分：B（良好，存在改进空间）
```

---

#### 技能 07：Bug 定位与修复

**调用指令**：`/find-bug [代码或错误信息]`

**功能说明**：分析代码中的潜在 bug，解释问题原因，并提供修复方案。能够识别空指针、越界访问、竞态条件、逻辑错误等常见问题。

**适用场景**：

- 调试未知代码
- 分析报错信息
- 预防性检查

**示例输入**：

```javascript
// 代码
function findUser(users, id) {
    for (var i = 0; i <= users.length; i++) {
        if (users[i].id === id) {
            return users[i];
        }
    }
    return null;
}
```

**示例输出**：

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

🐛 问题 2：使用 var 声明
位置：第 2 行
问题：var 存在变量提升，建议使用 let/const

修复方案：
将 var 改为 const 或 let

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

---

### 3.3 代码重构类

---

#### 技能 08：代码重构与优化

**调用指令**：`/refactor [代码或文件路径] [优化目标]`

**功能说明**：对代码进行重构优化，包括提取重复代码、简化复杂逻辑、改善命名、增强可读性。提供优化前后的对比说明。

**适用场景**：

- 清理技术债务
- 提升代码质量
- 改善可维护性

**示例输入**：

```python
# 待优化代码
def calculate(a, b, c, d):
    if a > 0:
        if b > 0:
            result = a + b
        else:
            result = a - b
    else:
        if c > 0:
            result = c * d
        else:
            result = c / d
    return result
```

**示例输出**：

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

def calculate(a: float, b: float, c: float, d: float) -> float:
    """计算两个数值的运算结果"""
    if a > 0:
        return _calculate_positive(a, b)
    return _calculate_negative(c, d)

def _calculate_positive(a: float, b: float) -> float:
    """处理正数情况"""
    return a + b if b > 0 else a - b

def _calculate_negative(c: float, d: float) -> float:
    """处理负数情况"""
    return c * d if c > 0 else c / d

改进点：
✓ 嵌套深度：4 层 → 2 层
✓ 函数名：描述性强
✓ 可测试性：提取的函数可独立测试
✓ 可读性：逻辑清晰易理解
```

---

#### 技能 09：代码清理

**调用指令**：`/cleanup [文件路径或代码]`

**功能说明**：移除死代码、未使用的导入、重复代码、过时注释。生成清洁的代码版本并说明清理内容。

**适用场景**：

- 项目维护
- 代码审查前清理
- 减小代码体积

**示例输入**：

```python
# 文件内容
import os
import sys
from typing import List, Dict, Optional  # Optional 未使用

def old_function():  # 未使用的函数
    pass

def process_data(data: List[int]) -> List[int]:
    # TODO: implement later
    result = []
    for item in data:
        if item > 0:
            result.append(item)
    return result

def another_function():
    x = 1
    y = 2
    z = 3  # 未使用的变量
    return x + y

# 下方是过时的注释
# This function was used for debugging
# Removed on 2023-01-01

def active_function():
    return "active"
```

**示例输出**：

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
   - 已完成 TODO（TODO: implement later）

────────────────────────────────────────
清理后代码：

import os
import sys
from typing import List

def process_data(data: List[int]) -> List[int]:
    return [item for item in data if item > 0]

def another_function():
    x = 1
    y = 2
    return x + y

def active_function():
    return "active"

────────────────────────────────────────
统计：
- 移除代码行数：12 行
- 移除注释行数：5 行
- 代码精简率：约 30%
```

---

#### 技能 10：代码格式化

**调用指令**：`/format [语言] [文件路径或代码]`

**功能说明**：根据语言最佳实践格式化代码，包括缩进、空格、命名规范、导入排序等。支持 ESLint、Prettier、Black、Go fmt 等工具的配置输出。

**适用场景**：

- 代码格式化
- 统一团队风格
- 自动修复简单问题

**示例输入**：

```javascript
// 未格式化的代码
function  test(a,b){
const  result=[];
for(let i=0;i<a.length;i++){
if(a[i]>b){
result.push(a[i]);}}
return result;
}
```

**示例输出**：

```
格式化建议（JavaScript/ESLint + Prettier）
────────────────────────────────────────
问题识别：
1. 函数名与参数缺少空格
2. const/let 声明缺少空格
3. 循环和条件语句缺少空格
4. 大括号位置不符合规范
5. 箭头函数应使用

────────────────────────────────────────
格式化后代码：

function test(a: number[], b: number): number[] {
  const result: number[] = [];
  for (let i = 0; i < a.length; i++) {
    if (a[i] > b) {
      result.push(a[i]);
    }
  }
  return result;
}

// 或使用更简洁的函数式写法：
const test = (a: number[], b: number): number[] =>
  a.filter((item) => item > b);

────────────────────────────────────────
配置文件建议：
// .eslintrc.json
{
  "rules": {
    "semi": ["error", "always"],
    "quotes": ["error", "single"],
    "indent": ["error", 2]
  }
}
```

---

### 3.4 测试与文档类

---

#### 技能 11：单元测试生成

**调用指令**：`/generate-test [文件路径或代码] [测试框架]`

**功能说明**：为指定代码生成完整的单元测试，覆盖正常路径、边界条件、异常情况。支持 pytest、unittest、Jest、Mocha 等主流测试框架。

**适用场景**：

- 新功能测试覆盖
- 回归测试保护
- TDD 实践

**示例输入**：

```python
# 待测试代码
def divide(a: float, b: float) -> float:
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

**示例输出**：

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
        assert divide(9, 3) == 3.0
        assert divide(7, 2) == 3.5

    def test_negative_numbers(self):
        """负数除法测试"""
        assert divide(-10, 2) == -5.0
        assert divide(10, -2) == -5.0
        assert divide(-10, -2) == 5.0

    def test_decimal_result(self):
        """小数结果测试"""
        result = divide(1, 3)
        assert abs(result - 0.333333) < 0.0001

    def test_zero_numerator(self):
        """分子为零的测试"""
        assert divide(0, 5) == 0.0

    def test_divide_by_zero_raises(self):
        """除零异常测试"""
        with pytest.raises(ValueError, match="Cannot divide by zero"):
            divide(10, 0)

    def test_divide_by_zero_with_zero_numerator(self):
        """0/0 情况测试"""
        with pytest.raises(ValueError):
            divide(0, 0)

────────────────────────────────────────
覆盖率预期：
- 分支覆盖：100%
- 边界条件：全部覆盖
- 执行命令：pytest test_divide.py -v --cov
```

---

#### 技能 12：API 文档生成

**调用指令**：`/generate-docs [代码或文件] [文档格式]`

**功能说明**：为代码生成 API 文档，包括函数说明、参数类型、返回值、异常处理、使用示例。支持 Markdown、OpenAPI、Swagger、JSDoc 等格式。

**适用场景**：

- 项目文档生成
- API 规范输出
- 代码自文档化

**示例输入**：

```typescript
// TypeScript 代码
interface User {
  id: number;
  name: string;
  email: string;
  createdAt: Date;
}

class UserService {
  async getUser(id: number): Promise<User | null> {
    // 实现
  }

  async createUser(data: {
    name: string;
    email: string;
  }): Promise<User> {
    // 实现
  }

  async deleteUser(id: number): Promise<boolean> {
    // 实现
  }
}
```

**示例输出**：

```
API 文档生成
────────────────────────────────────────
# UserService 类

用户管理服务，提供用户的增删改查功能。

## 方法

### getUser

根据用户 ID 获取用户信息。

```typescript
async getUser(id: number): Promise<User | null>
```

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| id | number | 是 | 用户 ID |

**返回值：**

- `Promise<User>` - 找到的用户对象
- `Promise<null>` - 用户不存在

**示例：**

```typescript
const user = await userService.getUser(123);
if (user) {
  console.log(user.name);
}
```

---

### createUser

创建新用户。

```typescript
async createUser(data: CreateUserDTO): Promise<User>
```

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| data | CreateUserDTO | 是 | 用户创建数据 |

**CreateUserDTO：**

```typescript
interface CreateUserDTO {
  name: string;    // 用户昵称
  email: string;   // 邮箱地址
}
```

**返回值：** `Promise<User>` - 创建成功的用户对象

**异常：**

- `Error` - 邮箱已存在

---

### deleteUser

删除用户。

```typescript
async deleteUser(id: number): Promise<boolean>
```

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| id | number | 是 | 用户 ID |

**返回值：**

- `Promise<true>` - 删除成功
- `Promise<false>` - 用户不存在
```

---

#### 技能 13：代码注释生成

**调用指令**：`/add-comments [文件路径或代码]`

**功能说明**：为代码添加规范的注释，包括文件头注释、函数说明、参数说明、返回值说明。遵循语言特定的注释规范（JSDoc、Google Style、NumPy Style 等）。

**适用场景**：

- 遗留代码补充注释
- 规范代码文档
- API 文档前置工作

**示例输入**：

```python
def process_orders(orders, customer_id, status):
    result = []
    for order in orders:
        if order['customer_id'] == customer_id:
            if status is None or order['status'] == status:
                result.append(order)
    return result
```

**示例输出**：

```
注释生成（Google Style）
────────────────────────────────────────

def process_orders(orders: List[Dict], customer_id: int,
                   status: Optional[str] = None) -> List[Dict]:
    """过滤符合条件的企业订单。

    根据客户 ID 和订单状态筛选订单列表。

    Args:
        orders: 订单列表，每个订单包含 customer_id、status 等字段
        customer_id: 客户 ID，用于过滤指定客户的订单
        status: 订单状态过滤条件，默认为 None（不限制状态）
                可选值：'pending'、'processing'、'completed'、'cancelled'

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
    if customer_id < 0:
        raise ValueError("customer_id 必须大于等于 0")

    result = []
    for order in orders:
        if order['customer_id'] == customer_id:
            if status is None or order['status'] == status:
                result.append(order)
    return result
```

---

### 3.5 部署与运维类

---

#### 技能 14：Dockerfile 生成

**调用指令**：`/generate-dockerfile [语言] [应用类型]`

**功能说明**：生成优化的 Dockerfile，包含多阶段构建、依赖缓存、安全基线、健康检查等。适用于 Web 应用、API 服务、后端服务等场景。

**适用场景**：

- 容器化部署
- CI/CD 集成
- 开发环境标准化

**示例输入**：

```
/generate-dockerfile python web-api
```

**示例输出**：

```
Dockerfile 生成（Python Web API）
────────────────────────────────────────

# 多阶段构建，优化镜像大小
FROM python:3.11-slim AS builder

# 安装构建依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# 复制依赖文件并安装
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt


# 生产镜像
FROM python:3.11-slim

# 安全配置
RUN addgroup --system --gid 1001 appgroup && \
    adduser --system --uid 1001 appuser

WORKDIR /app

# 从构建阶段复制依赖
COPY --from=builder /root/.local /root/.local
ENV PATH=/root/.local/bin:$PATH

# 复制应用代码
COPY --chown=appuser:appgroup . .

# 切换到非 root 用户
USER appuser

# 健康检查
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD python -c "import requests; requests.get('http://localhost:8000/health')"

# 暴露端口
EXPOSE 8000

# 启动命令
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

────────────────────────────────────────
优化要点：
✓ 多阶段构建：镜像大小减少约 60%
✓ 非 root 用户：符合安全最佳实践
✓ 依赖缓存：加速构建
✓ 健康检查：容器健康监控
✓ .dockerignore：排除不需要的文件
```

---

#### 技能 15：CI/CD 流水线生成

**调用指令**：`/generate-ci [平台] [语言]`

**功能说明**：生成 CI/CD 配置文件，包含代码检查、测试、构建、部署等阶段。支持 GitHub Actions、GitLab CI、jenkins 等主流平台。

**适用场景**：

- 自动化流程搭建
- 持续集成配置
- 部署自动化

**示例输入**：

```
/generate-ci github-actions python
```

**示例输出**：

```
CI/CD 流水线生成（GitHub Actions）
────────────────────────────────────────

# .github/workflows/ci.yml

name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  PYTHON_VERSION: '3.11'
  NODE_VERSION: '18'

jobs:
  # 代码质量检查
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}

      - name: Install dependencies
        run: |
          pip install flake8 black isort
          pip install -r requirements.txt

      - name: Run linters
        run: |
          flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
          black --check .
          isort --check .

  # 单元测试
  test:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}

      - name: Install dependencies
        run: pip install -r requirements.txt -r requirements-dev.txt

      - name: Run tests with coverage
        run: |
          pytest --cov=. --cov-report=xml --cov-report=term
        env:
          COVERAGE_THRESHOLD: 80

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage.xml
          fail_ci_if_error: true

  # 安全扫描
  security:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4

      - name: Run security scan
        uses: snyk/actions/python@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

  # 构建 Docker 镜像
  build:
    runs-on: ubuntu-latest
    needs: security
    if: github.event_name == 'push'
    steps:
      - uses: actions/checkout@v4

      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .

      - name: Push to registry
        run: |
          echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin
          docker push myapp:${{ github.sha }}

────────────────────────────────────────
流程说明：
1. lint → 2. test → 3. security → 4. build
   每个阶段成功后才会进入下一阶段
```

---

### 3.6 安全与性能类

---

#### 技能 16：安全漏洞扫描

**调用指令**：`/security-scan [文件路径或代码]`

**功能说明**：扫描代码中的安全漏洞，包括 SQL 注入、XSS、命令注入、硬编码密码、敏感信息泄露等。提供修复建议和安全最佳实践。

**适用场景**：

- 安全审计
- 代码审查
- 上线前检查

**示例输入**：

```python
# Flask 应用
from flask import Flask, request
import sqlite3

app = Flask(__name__)

@app.route('/user')
def get_user():
    user_id = request.args.get('id')
    conn = sqlite3.connect('users.db')
    cursor = conn.cursor()
    query = f"SELECT * FROM users WHERE id = {user_id}"
    cursor.execute(query)
    return cursor.fetchall()
```

**示例输出**：

```
安全扫描报告
────────────────────────────────────────
文件：app.py

🔴 严重漏洞：

1. SQL 注入漏洞（第 12 行）
   问题：使用字符串格式化拼接 SQL 查询
   代码：query = f"SELECT * FROM users WHERE id = {user_id}"
   风险：攻击者可注入恶意 SQL 语句
   修复：使用参数化查询
   ─────────────────────────────────
   修复代码：
   query = "SELECT * FROM users WHERE id = ?"
   cursor.execute(query, (user_id,))

🟡 中等风险：

2. 敏感信息泄露（第 3 行）
   问题：数据库连接未加密，路径硬编码
   代码：conn = sqlite3.connect('users.db')
   风险：数据库文件暴露敏感数据
   修复：使用环境变量配置数据库路径

🟡 中等风险：

3. 缺少输入验证（第 9 行）
   问题：未验证 user_id 参数类型和范围
   风险：可能导致异常或异常行为
   修复：添加参数验证逻辑

🟢 安全建议：

4. 建议添加速率限制防止暴力破解
5. 建议使用 HTTPS 加密通信
6. 建议添加身份认证机制

────────────────────────────────────────
扫描统计：
- 严重漏洞：1
- 中等风险：2
- 建议：3
- 整体评分：D（存在严重漏洞）
```

---

#### 技能 17：性能瓶颈分析

**调用指令**：`/analyze-performance [代码或文件]`

**功能说明**：分析代码的性能瓶颈，识别低效算法、重复计算、内存泄漏、N+1 查询等问题。提供优化方案和预期收益。

**适用场景**：

- 性能优化
- 慢查询分析
- 架构优化决策

**示例输入**：

```python
# 性能问题代码
def get_user_with_posts(user_ids):
    users = []
    for user_id in user_ids:
        user = db.query("SELECT * FROM users WHERE id = ?", user_id)
        posts = db.query("SELECT * FROM posts WHERE user_id = ?", user_id)
        user['posts'] = posts
        users.append(user)
    return users
```

**示例输出**：

```
性能分析报告
────────────────────────────────────────
问题代码分析

🔴 N+1 查询问题（第 4-6 行）

问题描述：
循环内执行数据库查询，user_ids 有 n 个就会执行 2n+1 次查询

当前查询次数：2n + 1（n 为用户数量）
假设 user_ids = [1,2,3,4,5]
执行查询数：11 次

预期性能影响：
- 100 个用户：201 次查询 → 约 2000ms
- 1000 个用户：2001 次查询 → 约 20000ms

────────────────────────────────────────
优化方案：

方案 1：使用 IN 子句批量查询
────────────────────────────────────────
def get_user_with_posts(user_ids):
    # 一次查询获取所有用户
    users = db.query(
        "SELECT * FROM users WHERE id IN ({})".format(
            ','.join(['?'] * len(user_ids))
        ),
        user_ids
    )

    # 一次查询获取所有相关帖子
    posts = db.query(
        "SELECT * FROM posts WHERE user_id IN ({})".format(
            ','.join(['?'] * len(user_ids))
        ),
        user_ids
    )

    # 内存中关联
    posts_by_user = {}
    for post in posts:
        posts_by_user.setdefault(post['user_id'], []).append(post)

    for user in users:
        user['posts'] = posts_by_user.get(user['id'], [])

    return users

优化后查询次数：2 次（固定）
性能提升：100n 倍（n=100 时）

────────────────────────────────────────
其他建议：

🟡 建议添加缓存层（Redis）
   预期提升：查询性能提升 10-100 倍

🟡 建议使用连接池
   预期提升：数据库连接建立时间减少 80%

🟡 考虑分页加载
   适用场景：用户帖子数量大时
```

---

## 四、可扩展机制

### 4.1 新增技能模板

当需要添加新技能时，使用项目根目录的模板文件 `SKILL_TEMPLATE.md`：

```markdown
---
name: [Skill Name]
description: This skill should be used when the user asks to "[trigger phrases]", or needs [what the skill does].
version: 1.0.0
---

# [Skill Name]

[Description of what this skill does.]
...
```

> **注意**：推荐直接复制 `SKILL_TEMPLATE.md` 文件并根据说明填写内容。

### 4.2 新增技能示例

```markdown
#### 技能 18：正则表达式生成

**调用指令**：`/generate-regex [模式描述]`

**功能说明**：根据自然语言描述生成对应的正则表达式，并提供匹配示例和常见变形。

**适用场景**：
- 表单验证规则
- 数据提取模式
- 文本处理规则

**示例输入**：
```
/generate-regex 验证中国大陆手机号
```

**示例输出**：
```
正则表达式生成
────────────────────────────────────────
基础表达式：^1[3-9]\d{9}$

匹配示例：
✓ 13812345678
✓ 19987654321
✗ 12345678901（首位不符合）
✗ 138123456789（位数过多）

Python 使用：
────────────────────────────────────────
import re

pattern = r'^1[3-9]\d{9}$'
phone = "13812345678"

if re.match(pattern, phone):
    print("有效手机号")
else:
    print("无效手机号")
```
```

---

## 五、技能调用总指令

> 以下是激活整个工具集的统一入口

### 5.1 快速调用语法

```
/skill [分类] [技能编号或名称] [参数]

分类选项：
- gen     : 代码生成类
- analyze : 代码分析类
- refactor: 代码重构类
- test    : 测试与文档类
- deploy  : 部署与运维类
- sec     : 安全与性能类
```

### 5.2 完整调用示例

```bash
# 生成 Python API 代码
/skill gen code python 实现用户登录接口

# 分析代码复杂度
/skill analyze complexity src/auth.py

# 重构优化
/skill refactor optimize src/utils.py

# 生成测试
/skill test unit src/auth.py pytest

# 生成 Dockerfile
/skill deploy docker python web

# 安全扫描
/skill sec scan src/login.py
```

### 5.3 自然语言调用（推荐）

直接使用自然语言描述需求，工具集会自动匹配最合适的技能：

```
# 以下调用等效
/generate-code python 实现 REST API
/skill gen code python 实现 REST API
```

---

## 六、使用建议

1. **组合使用**：多个技能可以串联使用，如 `/generate-code` + `/add-comments` + `/generate-test`
2. **渐进式调用**：先使用简单指令，再逐步添加细节参数
3. **迭代优化**：先获取基础输出，再根据反馈调整
4. **保存模板**：将常用的技能调用序列保存为个人模板

---

## 七、版本信息

- **版本**：1.0.0
- **更新时间**：2026-04-03
- **维护状态**：持续更新中
- **反馈渠道**：通过 Claude Code 进行问题反馈

---

*本工具集旨在提升开发效率，每个技能均可独立使用，也可组合形成完整工作流。*