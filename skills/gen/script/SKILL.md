---
name: Generate Script
description: This skill should be used when the user asks to "generate script", "create script", "write script", "automation script", "shell script", "bash script", "python script", or needs executable scripts for automation tasks.
version: 1.0.0
---

# Generate Script Skill

Generate automation scripts for various tasks including data processing, file operations, system maintenance, and scheduled jobs.

## When to Use

Use this skill when user needs:
- Automation scripts
- Data processing pipelines
- File manipulation scripts
- System maintenance scripts
- Scheduled task scripts

## Script Types

### Shell Scripts (Bash/Zsh)
- File operations (rename, move, convert)
- System monitoring
- Backup scripts
- Deployment scripts
- Batch processing

### Python Scripts
- Data processing and transformation
- API interactions
- Database operations
- Report generation
- Web scraping

### PowerShell Scripts
- Windows system administration
- Active Directory operations
- File management

## Template Structure

```bash
#!/bin/bash

# Script description
# Usage: ./script.sh <parameters>

# Constants
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Functions
function main() {
    # Main logic
}

# Entry point
main "$@"
```

## Best Practices

1. Add shebang (#!/bin/bash, #!/usr/bin/env python3)
2. Include usage comment
3. Handle arguments with getopts or argparse
4. Add error handling
5. Make scripts executable (chmod +x)
6. Use absolute paths or relative to script location

## Output Format

Provide:
1. Executable script with proper permissions
2. Usage instructions
3. Dependencies
4. Example usage

## Related Skills

- **generate-code** - For function implementations
- **generate-project** - For project structure
- **generate-dockerfile** - For container deployment