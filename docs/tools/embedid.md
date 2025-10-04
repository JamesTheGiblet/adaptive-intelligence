# EmbedID — Code Watermarking & Provenance Protocol

## The Problem

- AI-generated code has no provenance
- Remix culture lacks attribution
- Code plagiarism is rampant
- No way to verify authorship

## The Solution

EmbedID embeds tamper-resistant watermarks in your code files, tracking authorship, remix lineage, and generation metadata.

## Features

- 🔏 Tamper-resistant watermarking
- 📜 Remix lineage tracking
- 🤖 AI generation metadata
- 🔍 Verification tools
- 📊 Sovereignty manifest

## How It Works

### Command Line Usage

```bash
# Add watermark to file
embedid add path/to/file.py --author "James The Giblet"

# Add metadata
embedid add src/*.py --author "My Team" \
  --metadata '{"generated_by": "Chameleon", "date": "2025-10-03"}'

# Verify watermark
embedid verify path/to/file.py

# Check remix lineage
embedid lineage path/to/file.py

# Batch operations
embedid add src/ --recursive --author "MyCompany"
```

### API Usage (Praximous Skill)

```bash
POST /api/v1/process
{
  "task_type": "embedid_sign",
  "target_path": "/path/to/project",
  "author": "My Team",
  "metadata": {
    "generated_by": "AI Assistant",
    "project": "MyProject",
    "license": "MIT"
  }
}
```

## Example Watermark

### Python File with EmbedID Watermark

```python
# File: api.py
# EmbedID: v1.0.0
# Author: James The Giblet
# Generated: 2025-10-03T14:30:00Z
# Generator: Praximous + Chameleon LM
# License: MIT
# Lineage: Original work
# Signature: sha256:a1b2c3d4e5f6...

def create_api():
    """Create and configure the API."""
    # Your code here
    pass
```

### JavaScript File with EmbedID Watermark

```javascript
/**
 * File: utils.js
 * EmbedID: v1.0.0
 * Author: Development Team
 * Generated: 2025-10-03T14:30:00Z
 * Generator: Manual creation
 * License: Apache-2.0
 * Lineage: Forked from utility-lib v2.1.0
 * Signature: sha256:b2c3d4e5f6a7...
 */

function formatDate(date) {
    // Implementation here
    return date.toISOString();
}
```

## Installation

```bash
# Install from PyPI
pip install embedid

# Install from npm
npm install -g @adaptive-intelligence/embedid

# Install from source
git clone https://github.com/adaptive-intelligence/embedid.git
cd embedid
pip install -e .
```

## Command Line Interface

### Core Commands

```bash
# Add watermark to files
embedid add <path> [options]

# Verify watermark integrity
embedid verify <path> [options]

# Show lineage information
embedid lineage <path> [options]

# Generate compliance report
embedid report <path> [options]

# Remove watermarks
embedid remove <path> [options]
```

### Options

```bash
embedid add --help

Usage: embedid add [OPTIONS] PATH

Options:
  --author TEXT           Author name or organization
  --license TEXT          License identifier (MIT, Apache-2.0, etc.)
  --generator TEXT        Tool or person that generated the code
  --metadata JSON         Additional metadata as JSON string
  --recursive             Process directories recursively
  --include TEXT          File patterns to include (comma-separated)
  --exclude TEXT          File patterns to exclude (comma-separated)
  --force                 Overwrite existing watermarks
  --dry-run              Preview changes without applying
  --format FORMAT         Watermark format: comment, header, metadata
  --help                 Show this message and exit
```

## Watermark Formats

### Comment-Based (Default)

```python
# EmbedID: v1.0.0 | Author: John Doe | License: MIT | Generated: 2025-10-03T14:30:00Z
def my_function():
    pass
```

### Header Block

```python
"""
EmbedID Watermark
================
Version: 1.0.0
Author: John Doe
License: MIT
Generated: 2025-10-03T14:30:00Z
Generator: Manual
Lineage: Original work
Signature: sha256:abc123...
"""

def my_function():
    pass
```

### Metadata File

```yaml
# .embedid.yml (project-level metadata)
version: "1.0.0"
project: "MyProject"
default_author: "MyTeam"
default_license: "MIT"
files:
  "src/api.py":
    author: "John Doe"
    generated: "2025-10-03T14:30:00Z"
    lineage: "Original work"
  "src/utils.py":
    author: "Jane Smith"
    generated: "2025-10-03T15:00:00Z"
    lineage: "Adapted from utils-lib v1.2.0"
```

## Supported File Types

### Programming Languages

- **Python** (.py) - Comment and docstring watermarks
- **JavaScript/TypeScript** (.js, .ts) - Comment watermarks
- **Java** (.java) - Javadoc and comment watermarks
- **C/C++** (.c, .cpp, .h) - Comment watermarks
- **Go** (.go) - Comment watermarks
- **Rust** (.rs) - Comment watermarks
- **PHP** (.php) - Comment watermarks
- **Ruby** (.rb) - Comment watermarks

### Documentation

- **Markdown** (.md) - HTML comment watermarks
- **HTML** (.html) - HTML comment watermarks
- **CSS** (.css) - CSS comment watermarks
- **YAML** (.yml, .yaml) - YAML comment watermarks
- **JSON** (.json) - Metadata file approach

### Configuration

- **Docker** (Dockerfile) - Comment watermarks
- **Shell Scripts** (.sh, .bash) - Comment watermarks
- **SQL** (.sql) - SQL comment watermarks

## Verification & Integrity

### Verify Single File

```bash
embedid verify src/api.py
```

**Output:**

```text
✓ Watermark found and valid
✓ Signature verified
✓ Timestamp valid
✓ Lineage intact

File: src/api.py
Author: James The Giblet
Generated: 2025-10-03T14:30:00Z
License: MIT
Lineage: Original work
Integrity: VALID
```

### Verify Project

```bash
embedid verify . --recursive --report verification-report.json
```

**Report Format:**

```json
{
  "summary": {
    "total_files": 147,
    "watermarked_files": 142,
    "valid_watermarks": 140,
    "invalid_watermarks": 2,
    "missing_watermarks": 5
  },
  "files": [
    {
      "path": "src/api.py",
      "status": "valid",
      "author": "James The Giblet",
      "generated": "2025-10-03T14:30:00Z",
      "license": "MIT"
    },
    {
      "path": "src/legacy.py",
      "status": "invalid",
      "error": "Signature mismatch",
      "author": "Unknown"
    }
  ]
}
```

## Lineage Tracking

### Show File History

```bash
embedid lineage src/utils.py
```

**Output:**

```text
Lineage for src/utils.py:

Original: utility-lib v2.1.0 (2024-05-15)
├── Forked by: Development Team (2024-06-01)
├── Modified by: John Doe (2024-07-15)
│   └── Added error handling
├── Enhanced by: Jane Smith (2024-08-20)
│   └── Performance improvements
└── Current: MyProject v1.0.0 (2025-10-03)
    └── Integrated into main project

Contributors: 4
License Chain: MIT → MIT → MIT (consistent)
Trust Score: 9.2/10
```

### Lineage Graph

```bash
embedid lineage . --graph --output lineage-graph.svg
```

Generates a visual graph showing the evolution and relationships between files in your project.

## Integration Points

- → **AAS**: Auto-watermark all agent-generated code
- → **Chameleon**: Sign AI-generated outputs
- → **Test Generator**: Track test provenance
- → **Aegis**: Sign security documentation
- → **Git**: Pre-commit watermarking

## Configuration Details

### Global Configuration

```yaml
# ~/.embedid/config.yml
defaults:
  author: "Your Name"
  license: "MIT"
  format: "comment"
  include_signature: true
  include_timestamp: true

signature:
  algorithm: "sha256"
  private_key_path: "~/.embedid/private.key"
  
verification:
  strict_mode: false
  trust_known_authors: true
  require_signatures: false

output:
  verbose: false
  color: true
  format: "text"
```

### Project Configuration

```yaml
# .embedid.yml (in project root)
project:
  name: "MyProject"
  version: "1.0.0"
  license: "MIT"
  repository: "https://github.com/user/repo"

defaults:
  author: "Project Team"
  generator: "Manual"

rules:
  require_watermark: true
  require_author: true
  require_license: true
  allowed_licenses: ["MIT", "Apache-2.0", "GPL-3.0"]

patterns:
  include: ["*.py", "*.js", "*.java"]
  exclude: ["**/node_modules/**", "**/vendor/**", "**/.git/**"]

lineage:
  track_modifications: true
  require_lineage_info: false
  trust_threshold: 7.0
```

## Best Practices

### 1. Consistent Attribution

```bash
# Set up project defaults
embedid init --author "MyTeam" --license "MIT"

# All new files get consistent watermarks
embedid add src/ --recursive
```

### 2. Git Integration

```bash
# Pre-commit hook
#!/bin/bash
# Check for unwatermarked files
embedid verify . --recursive --strict --exit-on-fail
```

### 3. CI/CD Integration

```yaml
# GitHub Actions
- name: Verify Code Attribution
  run: |
    pip install embedid
    embedid verify . --recursive --report attribution-report.json
    
- name: Upload Attribution Report
  uses: actions/upload-artifact@v3
  with:
    name: attribution-report
    path: attribution-report.json
```

### 4. License Compliance

```bash
# Check license compatibility
embedid report . --check-licenses --output compliance-report.pdf

# Ensure all code has proper attribution
embedid verify . --require-license --require-author
```

## API Reference

### Watermark Creation

```python
from embedid import EmbedID

# Initialize
embedder = EmbedID()

# Add watermark
result = embedder.add_watermark(
    file_path="src/api.py",
    author="John Doe",
    license="MIT",
    metadata={
        "project": "MyProject",
        "generated_by": "AI Assistant"
    }
)
```

### Verification

```python
# Verify watermark
verification = embedder.verify_watermark("src/api.py")

print(f"Valid: {verification.is_valid}")
print(f"Author: {verification.author}")
print(f"License: {verification.license}")
print(f"Integrity: {verification.integrity_score}")
```

### Lineage Analysis

```python
# Get lineage information
lineage = embedder.get_lineage("src/api.py")

for entry in lineage.history:
    print(f"{entry.date}: {entry.author} - {entry.description}")

print(f"Trust Score: {lineage.trust_score}")
print(f"License Chain: {' → '.join(lineage.license_chain)}")
```

## Troubleshooting

### Common Issues

**Q: Watermarks are being removed by code formatters**
A: Configure your formatter to preserve comment blocks, or use metadata file approach

**Q: Signature verification failing**
A: Check that the private key hasn't changed and the file hasn't been modified

**Q: Performance impact on large codebases**
A: Use selective watermarking with include/exclude patterns

**Q: Conflicts with existing license headers**
A: Use the `--merge-headers` option to integrate with existing headers

### Getting Help

- GitHub Issues: [Report bugs](https://github.com/adaptive-intelligence/embedid/issues)
- Discord: [Join community](https://discord.gg/adaptive-intel)
- Documentation: [Full docs](https://docs.adaptiveintel.dev/embedid)
