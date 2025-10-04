# Whisper — Secret & Credential Guardian

Stop secrets from leaking into your codebase before they cause damage.

## The Problem

Every developer has committed a secret by accident:

- API keys in source code
- Passwords in config files
- Tokens in environment files
- Database credentials in logs
- AWS keys in commits

**The damage:** Security breaches, unauthorized access, compliance violations, and embarrassment.

## The Solution

Whisper scans your codebase for secrets BEFORE they leak. Fast, accurate, and integrates seamlessly with your workflow.

## Features

- **Pattern-based detection** — Recognizes 50+ secret types (API keys, passwords, tokens, certificates)
- **Low false-positive rate** — Smart algorithms reduce noise
- **Blazing fast** — Scans 1000s of files per second
- **Git hook integration** — Block commits containing secrets
- **CI/CD ready** — Automated pipeline checks
- **Detailed reports** — JSON/HTML output with remediation steps
- **Custom patterns** — Add your own secret detection rules

## Installation

```bash
# Via pip
pip install adaptive-whisper

# Via Docker
docker pull adaptiveintel/whisper:latest

# From source
git clone https://github.com/adaptive-intelligence/whisper.git
cd whisper
pip install -e .
Quick Start
