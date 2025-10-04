# Aegis CLI — Security Documentation Generator

## The Problem

- SECURITY.md files don't exist
- dependabot.yml not configured
- Secure coding guidelines missing
- Security debt accumulates

## The Solution

Aegis generates professional security documentation in 3 seconds. Detects your language, applies best practices, outputs ready-to-use files.

## Features

- 📋 Auto-generates SECURITY.md
- ⚙️ Creates dependabot.yml configuration
- 📚 Language-specific secure coding guides
- 🔄 Keeps docs updated with latest best practices
- ✅ Customizable templates

## How It Works

```bash
# Initialize security docs for project
aegis /path/to/project

# Dry run (preview without writing)
aegis /path/to/project --dry-run

# Custom output directory
aegis /path/to/project --output ./security

# Update existing docs
aegis update /path/to/project

# Check security posture
aegis status /path/to/project
```

## Installation

```bash
pip install praximous-aegis-cli
```

## Generated Files

```text
project/
├── .github/
│   └── dependabot.yml          # Automated dependency updates
└── security/
    ├── SECURITY.md             # Vulnerability disclosure policy
    └── SecureCodingGuide.md    # Language-specific best practices
```

## Command Line Options

```bash
aegis --help

Usage: aegis [OPTIONS] COMMAND [ARGS]...

Commands:
  init      Initialize security documentation
  update    Update existing security docs
  status    Check security posture
  template  Manage custom templates

Options:
  --output PATH        Custom output directory
  --dry-run           Preview changes without writing
  --template NAME     Use specific template
  --language LANG     Override language detection
  --format FORMAT     Output format: markdown, html, pdf
  --quiet             Suppress non-error output
  --verbose           Enable verbose logging
  --help              Show this message and exit
```

## Integration Points

- → AAS: Runs automatically on new projects
- → Whisper: Integrated scanning workflow
- → TreeCraft: Structure-aware security recommendations
- → Chameleon: AI-customized security policies
- → CertiScope: Verifies security resource links

## API Usage (Praximous Skill)

```bash
POST /api/v1/process
{
  "task_type": "aegis_initialize",
  "prompt": "Generate security documentation",
  "target_path": "/path/to/project",
  "options": {
    "template": "enterprise",
    "language": "python",
    "include_guides": true
  }
}
```

## Templates

### Available Templates

- **basic**: Essential security documentation
- **enterprise**: Comprehensive security framework
- **startup**: Lightweight security guidelines
- **open-source**: Public project security policies

### Custom Templates

Create your own templates in `~/.aegis/templates/`:

```yaml
# custom-template.yml
name: "My Company Template"
description: "Internal security standards"
files:
  - name: "SECURITY.md"
    template: "security-policy.md.j2"
  - name: "dependabot.yml"
    template: "dependabot.yml.j2"
  - name: "SecureCoding.md"
    template: "secure-coding.md.j2"
variables:
  company_name: "My Company"
  contact_email: "security@company.com"
  response_time: "48 hours"
```

## Language Support

Aegis automatically detects your project language and generates appropriate guidance:

- **Python**: Django, Flask, FastAPI security best practices
- **JavaScript/Node.js**: Express, React, Vue security guidelines
- **Java**: Spring Boot, security annotations, OWASP guidelines
- **C#/.NET**: ASP.NET Core, authentication, authorization
- **Go**: Gorilla, Gin, security middleware
- **Rust**: Actix, Rocket, memory safety practices
- **PHP**: Laravel, Symfony, secure coding standards

## Configuration

Create `.aegis.yml` in your project root:

```yaml
# Project configuration
project:
  name: "My Project"
  description: "Web application"
  language: "python"
  framework: "fastapi"

# Template settings
template: "enterprise"
output_dir: "./docs/security"

# Contact information
contacts:
  security_team: "security@company.com"
  maintainer: "dev-team@company.com"

# Response times
response_times:
  critical: "4 hours"
  high: "24 hours"
  medium: "7 days"
  low: "30 days"

# Custom sections
custom_sections:
  - name: "Infrastructure Security"
    content: "docs/infrastructure-security.md"
  - name: "API Security"
    content: "docs/api-security.md"
```

## Best Practices

1. **Run on every new project**: Initialize security docs from day one
2. **Keep updated**: Run `aegis update` when dependencies change
3. **Customize for your org**: Create custom templates with your policies
4. **Review regularly**: Use `aegis status` to check security posture
5. **Integrate with CI/CD**: Automate documentation updates

## Example Generated SECURITY.md

```markdown
# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 2.1.x   | :white_check_mark: |
| 2.0.x   | :x:                |
| 1.9.x   | :white_check_mark: |

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability,
please report it responsibly:

**Email**: security@company.com
**Response Time**: 48 hours
**Update Frequency**: Weekly

### What to Include

1. Description of the vulnerability
2. Steps to reproduce
3. Potential impact
4. Suggested remediation (if known)

### What to Expect

- **Acknowledgment**: Within 48 hours
- **Initial Assessment**: Within 7 days
- **Resolution Timeline**: Based on severity
- **Public Disclosure**: After fix is deployed

## Security Best Practices

### For Developers

- Use parameterized queries to prevent SQL injection
- Validate and sanitize all user inputs
- Implement proper authentication and authorization
- Keep dependencies up to date
- Use HTTPS for all communications

### For Operations

- Regular security updates
- Monitor for suspicious activity
- Backup data regularly
- Implement logging and alerting
- Use principle of least privilege
```

## CI/CD Integration

### GitHub Actions

```yaml
- name: Update Security Docs
  run: |
    pip install praximous-aegis-cli
    aegis update . --output ./docs/security
    git add docs/security/
    git commit -m "docs: update security documentation" || exit 0
```

### GitLab CI

```yaml
update_security_docs:
  script:
    - pip install praximous-aegis-cli
    - aegis update . --output ./docs/security
  artifacts:
    paths:
      - docs/security/
```

## Troubleshooting

### Common Issues

**Q: Language not detected correctly**
A: Use `--language` flag to override detection

**Q: Templates not found**
A: Check `~/.aegis/templates/` directory exists and contains templates

**Q: Permission denied writing files**
A: Ensure you have write permissions to the output directory

**Q: Dependabot configuration not working**
A: Verify the generated `.github/dependabot.yml` syntax

### Getting Help

- GitHub Issues: [Report bugs](https://github.com/adaptive-intelligence/aegis-cli/issues)
- Discord: [Join community](https://discord.gg/adaptive-intel)
- Documentation: [Full docs](https://docs.adaptiveintel.dev/aegis)
