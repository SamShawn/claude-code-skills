# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains a **Claude Code Skills toolset** - a collection of 17 reusable programming skills that can be invoked via slash commands. Each skill provides specialized capabilities for code generation, analysis, refactoring, testing, deployment, and security.

## Skills Overview

The toolset covers 6 categories:

| Category | Skills |
|----------|--------|
| Code Generation | `/generate-code`, `/generate-project`, `/generate-template`, `/generate-script` |
| Code Analysis | `/explain-code`, `/analyze-complexity`, `/find-bug` |
| Code Refactoring | `/refactor`, `/cleanup`, `/format` |
| Testing & Docs | `/generate-test`, `/generate-docs`, `/add-comments` |
| Deployment | `/generate-dockerfile`, `/generate-ci` |
| Security & Performance | `/security-scan`, `/analyze-performance` |

### Quick Invocation

```
/skill [category] [skill] [parameters]

Categories: gen, analyze, refactor, test, deploy, sec
```

For example:
- `/skill gen code python 实现REST API` - generate Python code
- `/skill test unit src/auth.py pytest` - generate unit tests

## Adding New Skills

When adding a new skill, follow the template in Section 4.1 of README.md. Each skill needs:
- A slash command invocation (e.g., `/generate-regex`)
- Clear functional description
- Usage scenarios
- Input/Output examples

## Repository Structure

```
claude-code-skills/
├── CLAUDE.md          # This file
└── README.md          # Full skill documentation (17 skills)
```