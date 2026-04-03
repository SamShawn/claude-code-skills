---
name: Generate Project
description: This skill should be used when the user asks to "generate project", "create project structure", "scaffold project", "init project", "new project", or needs a complete project directory layout.
version: 1.0.0
---

# Generate Project Skill

Generate complete project directory structures with all necessary files, configurations, and best practices.

## When to Use

Use this skill when user needs:
- Initialize a new project
- Standardize project structure
- Quick project scaffolding
- Understand project organization

## Project Types

- Web applications (frontend, backend, full-stack)
- API services (REST, GraphQL)
- CLI tools
- Libraries/packages
- Data science projects
- Mobile apps

## Structure Templates

### Standard Web Service
```
project_name/
├── src/
│   ├── __init__.py
│   ├── main.py
│   ├── config.py
│   ├── models/
│   ├── services/
│   ├── routers/
│   └── utils/
├── tests/
├── scripts/
├── docs/
├── .env.example
├── requirements.txt
├── docker-compose.yml
├── Dockerfile
└── README.md
```

### Standard Frontend
```
project_name/
├── src/
│   ├── components/
│   ├── pages/
│   ├── hooks/
│   ├── utils/
│   ├── styles/
│   └── App.tsx
├── public/
├── tests/
├── package.json
├── tsconfig.json
├── .eslintrc.json
└── README.md
```

## Best Practices

1. Include language-specific package managers
2. Add .gitignore templates
3. Include Docker configurations
4. Add CI/CD configuration files
5. Provide environment variable templates
6. Include testing setup

## Related Skills

- **generate-code** - For code implementation
- **generate-dockerfile** - For container configuration
- **generate-ci** - For CI/CD pipelines