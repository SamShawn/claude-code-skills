---
name: Generate CI
description: This skill should be used when the user asks to "generate ci", "create ci", "CI/CD", "pipeline", "github actions", "gitlab ci", "jenkins", or needs to create CI/CD configuration.
version: 1.0.0
---

# Generate CI Skill

生成 CI/CD 配置文件，包含代码检查、测试、构建、部署等阶段。支持 GitHub Actions、GitLab CI、Jenkins 等主流平台。

## 适用场景

- 自动化流程搭建
- 持续集成配置
- 部署自动化
- 安全扫描集成

## GitHub Actions 示例

```yaml
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

      - name: Upload coverage
        uses: codecov/codecov-action@v3

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
```

## GitLab CI 示例

```yaml
# .gitlab-ci.yml

stages:
  - lint
  - test
  - security
  - build
  - deploy

lint:
  stage: lint
  image: python:3.11
  script:
    - pip install flake8 black isort
    - flake8 .
    - black --check .
    - isort --check .

test:
  stage: test
  image: python:3.11
  script:
    - pip install -r requirements.txt -r requirements-dev.txt
    - pytest --cov=. --cov-report=xml
  coverage: '/TOTAL.*\s+(\d+%)$/'

security:
  stage: security
  image: python:3.11
  script:
    - pip install safety bandit
    - safety check
    - bandit -r .

build:
  stage: build
  script:
    - docker build -t myapp:$CI_COMMIT_SHA .
  only:
    - main

deploy:
  stage: deploy
  script:
    - docker push myapp:$CI_COMMIT_SHA
  only:
    - main
```

## 输出格式

```
CI/CD 流水线生成（GitHub Actions）
────────────────────────────────────────

# .github/workflows/ci.yml

name: CI/CD Pipeline

[完整配置]

────────────────────────────────────────
流程说明：
1. lint → 2. test → 3. security → 4. build
   每个阶段成功后才会进入下一阶段
```

## 最佳实践

1. **阶段顺序**：lint → test → security → build → deploy
2. **失败停止**：使用 `needs` 确保依赖顺序
3. **缓存优化**：使用缓存加速依赖安装
4. **环境分离**：dev/staging/prod 不同配置
5. **密钥管理**：使用 secrets 存储敏感信息
6. **并行作业**：独立任务并行执行

## 相关技能

- **generate-dockerfile** - Docker 配置
- **generate-project** - 项目结构
- **security-scan** - 安全扫描