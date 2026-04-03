---
name: Generate Dockerfile
description: This skill should be used when the user asks to "generate dockerfile", "create dockerfile", "docker build", "containerize", "docker config", or needs to create Docker configuration for an application.
version: 1.0.0
---

# Generate Dockerfile Skill

生成优化的 Dockerfile，包含多阶段构建、依赖缓存、安全基线、健康检查等。适用于 Web 应用、API 服务、后端服务等场景。

## 适用场景

- 容器化部署
- CI/CD 集成
- 开发环境标准化
- 多阶段构建优化

## Dockerfile 示例

### Python Web API
```dockerfile
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
```

### Node.js API
```dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine

WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .

USER node
EXPOSE 3000
CMD ["node", "server.js"]
```

## 最佳实践

1. **多阶段构建**：减小最终镜像大小
2. **非 root 用户**：符合安全最佳实践
3. **依赖缓存**：加速构建
4. **健康检查**：容器健康监控
5. **.dockerignore**：排除不需要的文件
6. **Layer 优化**：将不常变化的文件放前面

## 输出格式

```
Dockerfile 生成（Python Web API）
────────────────────────────────────────

# 多阶段构建
FROM ...

[完整 Dockerfile]

────────────────────────────────────────
优化要点：
✓ 多阶段构建：镜像大小减少约 60%
✓ 非 root 用户：符合安全最佳实践
✓ 依赖缓存：加速构建
✓ 健康检查：容器健康监控
✓ .dockerignore：排除不需要的文件
```

## .dockerignore 示例
```
node_modules
__pycache__
*.pyc
.git
.gitignore
README.md
.env
*.log
.DS_Store
```

## 相关技能

- **generate-project** - 项目结构
- **generate-ci** - CI/CD 配置
- **generate-script** - 部署脚本