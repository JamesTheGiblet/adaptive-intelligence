# Whisper — Secret & Credential Guardian

## The Problem

- API keys committed to Git
- Credentials in log files
- Secrets in config files
- Database passwords in source code

## The Solution

Whisper scans your codebase for secrets BEFORE they leak. Works locally, runs fast, integrates with Git hooks.

## Features

- 🔍 Pattern-based detection (API keys, passwords, tokens)
- 🎯 Low false-positive rate
- ⚡ Fast scanning (1000s of files/second)
- 🔗 Git pre-commit hook integration
- 📊 Detailed reports with remediation steps

## How It Works

```bash
# Scan current directory
whisper --scan .

# Install Git hook (prevents commits with secrets)
whisper install-hook

# Scan specific file types
whisper --scan . --include "*.py,*.js,*.env"

# Generate report
whisper --scan . --output secrets-report.json
```

## Installation

```bash
pip install adaptive-whisper
```

## Integration Points

- → AAS: Runs as automated agent on every commit
- → Praximous: Smart Skill for secret detection in AI-generated code
- → Aegis: Pre-scan before generating security docs
- → Test Generator: Scan test files for hardcoded credentials
- → CI/CD: Automated pipeline checks

## CI/CD Integration

### GitHub Actions

```yaml
- name: Run Whisper Secret Scan
  run: |
    pip install adaptive-whisper
    whisper --scan . --fail-on-found
```

### GitLab CI

```yaml
security_scan:
  script:
    - pip install adaptive-whisper
    - whisper --scan . --fail-on-found
```

## Configuration

Create a `.whisper.yml` file in your project root:

```yaml
# Patterns to scan for
patterns:
  - api_keys
  - passwords
  - tokens
  - certificates

# Files to include
include:
  - "*.py"
  - "*.js"
  - "*.env"
  - "*.yml"
  - "*.yaml"

# Files to exclude
exclude:
  - "node_modules/*"
  - "*.git/*"
  - "*.log"

# Sensitivity level
sensitivity: medium  # low, medium, high

# Output format
output_format: json  # json, yaml, text
```

## Command Line Options

```bash
whisper --help

Usage: whisper [OPTIONS]

Options:
  --scan PATH              Directory or file to scan
  --include TEXT           File patterns to include (comma-separated)
  --exclude TEXT           File patterns to exclude (comma-separated)
  --output PATH            Output file for results
  --format FORMAT          Output format: json, yaml, text
  --fail-on-found         Exit with error code if secrets found
  --install-hook          Install Git pre-commit hook
  --remove-hook           Remove Git pre-commit hook
  --sensitivity LEVEL     Detection sensitivity: low, medium, high
  --quiet                 Suppress non-error output
  --verbose               Enable verbose logging
  --help                  Show this message and exit
```

## API Usage (Praximous Skill)

```bash
POST /api/v1/process
{
  "task_type": "whisper_scan",
  "prompt": "Scan this codebase for secrets",
  "target_path": "/path/to/project",
  "options": {
    "sensitivity": "high",
    "include": ["*.py", "*.js"],
    "exclude": ["node_modules/*"]
  }
}
```

Response:

```json
{
  "status": "success",
  "secrets_found": 2,
  "high_confidence": 1,
  "medium_confidence": 1,
  "files_scanned": 147,
  "scan_time": "2.3s",
  "results": [
    {
      "file": "src/config.py",
      "line": 23,
      "type": "api_key",
      "confidence": "high",
      "match": "api_key = 'sk-1234567890abcdef'",
      "recommendation": "Move to environment variable"
    },
    {
      "file": "tests/test_auth.py",
      "line": 45,
      "type": "password",
      "confidence": "medium",
      "match": "password = 'test123'",
      "recommendation": "Use test fixtures instead"
    }
  ]
}
```

## Supported Secret Types

- **API Keys**: AWS, Google Cloud, Azure, OpenAI, GitHub, etc.
- **Passwords**: Database passwords, service accounts
- **Tokens**: JWT tokens, OAuth tokens, access tokens
- **Certificates**: Private keys, SSL certificates
- **Database URLs**: Connection strings with credentials
- **Custom Patterns**: Define your own regex patterns

## False Positive Handling

Add comments to suppress false positives:

```python
# whisper:ignore
api_key = "fake-key-for-documentation"

# Or use environment variables
api_key = os.getenv("API_KEY")  # whisper:safe
```

## Best Practices

1. **Run before every commit**: Install the Git hook
2. **Scan regularly**: Include in CI/CD pipelines
3. **Use environment variables**: Never hardcode secrets
4. **Review findings**: Not all findings are real secrets
5. **Configure sensitivity**: Adjust based on your needs

## Troubleshooting

### Common Issues

**Q: Too many false positives**
A: Lower the sensitivity level or add exclusion patterns

**Q: Git hook not working**
A: Ensure whisper is in your PATH and the hook has execute permissions

**Q: Scan is too slow**
A: Use include patterns to scan only relevant files

**Q: Missing secrets**
A: Increase sensitivity level or add custom patterns

### Getting Help

- GitHub Issues: [Report bugs](https://github.com/adaptive-intelligence/whisper/issues)
- Discord: [Join community](https://discord.gg/adaptive-intel)
- Documentation: [Full docs](https://docs.adaptiveintel.dev/whisper)
