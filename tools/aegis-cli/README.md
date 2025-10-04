# Aegis CLI — Security Compliance Framework

> **Automated security documentation and compliance management for development teams**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](../../LICENSE)
[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://python.org)
[![CLI](https://img.shields.io/badge/CLI-Ready-green.svg)](https://github.com/adaptive-intelligence/aegis-cli)

Aegis CLI transforms security compliance from a manual burden into an automated process, generating professional security documentation, policies, and compliance reports that satisfy enterprise requirements.

---

## 🎯 What Does Aegis Do?

**The Problem**: Security compliance is time-consuming, error-prone, and often becomes outdated quickly.

**The Solution**: Aegis CLI automates the creation and maintenance of:

- 📋 **Security policies and procedures**
- 🔍 **Compliance documentation** (SOC2, ISO27001, GDPR, etc.)
- 📊 **Executive security reports**
- 🛡️ **Risk assessments and mitigation plans**
- 📈 **Continuous compliance monitoring**

---

## ⚡ Quick Start

### Installation

```bash
# Install via pip
pip install aegis-cli

# Install via npm
npm install -g @adaptive-intelligence/aegis-cli

# Install from source
git clone https://github.com/adaptive-intelligence/aegis-cli.git
cd aegis-cli
pip install -e .
```

### First Run

```bash
# Initialize Aegis in your project
aegis init

# Generate SOC2 compliance documentation
aegis generate --framework soc2 --output compliance-docs/

# Create security policies
aegis policies --template enterprise --output policies/

# Generate executive report
aegis report --format executive --output security-report.pdf
```

### Quick Example

```bash
# Scan project and generate complete compliance package
aegis compliance-scan ./ --frameworks soc2,iso27001 --policies --reports
```

**Result**: Complete compliance documentation ready for auditors in minutes, not weeks.

---

## 🏗️ Core Features

### 📋 Automated Documentation Generation

#### Security Policies

```bash
# Generate comprehensive security policies
aegis policies --template enterprise
```

**Creates**:

- Information Security Policy
- Data Protection Policy  
- Incident Response Plan
- Access Control Procedures
- Risk Management Framework

#### Compliance Frameworks

```bash
# SOC2 Type II documentation
aegis generate --framework soc2 --type type2

# ISO 27001 documentation
aegis generate --framework iso27001 --controls all

# GDPR compliance package
aegis generate --framework gdpr --region eu
```

#### Custom Templates

```bash
# Use organization-specific templates
aegis generate --template custom/my-org-template.yml
```

### 🔍 Intelligent Project Analysis

#### Security Scanning

```bash
# Analyze project security posture
aegis scan --deep-analysis --recommendations
```

**Analyzes**:

- Code security patterns
- Configuration security
- Dependency vulnerabilities
- Infrastructure security
- Access control implementation

#### Gap Analysis

```bash
# Identify compliance gaps
aegis gap-analysis --framework soc2 --current-state audit/
```

### 📊 Professional Reporting

#### Executive Reports

```bash
# C-level security summary
aegis report --audience executive --format pdf
```

#### Technical Reports  

```bash
# Detailed technical findings
aegis report --audience technical --format html --interactive
```

#### Audit-Ready Documentation

```bash
# Auditor-friendly documentation package
aegis audit-package --framework soc2 --period "2025-Q1"
```

---

## 🛠️ Command Reference

### Core Commands

#### `aegis init`

Initialize Aegis in your project

```bash
aegis init [OPTIONS]

Options:
  --framework TEXT    Primary compliance framework [soc2|iso27001|gdpr|hipaa]
  --template TEXT     Organization template to use
  --interactive       Interactive setup wizard
  --org-info PATH     Organization information file
```

#### `aegis generate`

Generate compliance documentation

```bash
aegis generate [OPTIONS]

Options:
  --framework TEXT    Compliance framework to generate for
  --output PATH       Output directory for documentation
  --format TEXT       Output format [markdown|html|pdf|docx]
  --template TEXT     Custom template to use
  --controls LIST     Specific controls to include
  --evidence PATH     Evidence directory to reference
```

#### `aegis policies`

Generate security policies

```bash
aegis policies [OPTIONS]

Options:
  --template TEXT     Policy template [startup|enterprise|government]
  --output PATH       Output directory for policies
  --customization     Include organization-specific customizations
  --review-cycle INT  Policy review cycle in months
```

#### `aegis scan`

Security analysis and scanning

```bash
aegis scan [PATH] [OPTIONS]

Options:
  --depth TEXT        Scan depth [surface|deep|comprehensive]
  --frameworks LIST   Check against specific frameworks
  --output PATH       Output directory for results
  --format TEXT       Report format [json|html|pdf]
  --fix              Automatically fix issues where possible
```

#### `aegis report`

Generate security reports

```bash
aegis report [OPTIONS]

Options:
  --audience TEXT     Target audience [executive|technical|audit]
  --format TEXT       Report format [pdf|html|pptx|json]
  --period TEXT       Reporting period
  --metrics           Include security metrics
  --charts            Include visual charts and graphs
```

### Advanced Commands

#### `aegis audit-package`

Create audit-ready documentation package

```bash
aegis audit-package [OPTIONS]

Options:
  --framework TEXT    Audit framework
  --auditor TEXT      Auditor organization name
  --period TEXT       Audit period
  --evidence-map      Include evidence mapping
  --controls-matrix   Include controls matrix
```

#### `aegis gap-analysis`

Perform compliance gap analysis

```bash
aegis gap-analysis [OPTIONS]

Options:
  --framework TEXT    Framework to analyze against
  --current-state PATH Current security state documentation
  --target-state PATH Target compliance requirements
  --remediation       Include remediation recommendations
```

#### `aegis monitor`

Set up continuous compliance monitoring

```bash
aegis monitor [OPTIONS]

Options:
  --schedule TEXT     Monitoring schedule [daily|weekly|monthly]
  --alerts TEXT       Alert configuration
  --dashboard         Enable monitoring dashboard
  --integrations LIST Third-party integrations
```

---

## 📋 Compliance Frameworks

### SOC 2 (Service Organization Control 2)

```bash
# Complete SOC2 Type II package
aegis generate --framework soc2 --type type2 --output soc2-docs/
```

**Generates**:

- Trust Services Criteria documentation
- Control descriptions and testing procedures
- Evidence collection templates
- Risk assessment documentation
- Vendor management procedures

**Controls Covered**: Security, Availability, Processing Integrity, Confidentiality, Privacy

### ISO 27001 (Information Security Management)

```bash
# ISO 27001 documentation suite
aegis generate --framework iso27001 --controls all --output iso27001/
```

**Generates**:

- Information Security Management System (ISMS) documentation
- Risk assessment and treatment procedures
- 114 security controls implementation guides
- Internal audit procedures
- Management review templates

### GDPR (General Data Protection Regulation)

```bash
# GDPR compliance package
aegis generate --framework gdpr --region eu --output gdpr-compliance/
```

**Generates**:

- Data Protection Impact Assessments (DPIA)
- Privacy notices and consent forms
- Data subject rights procedures
- Breach notification procedures
- Data Processing Records (Article 30)

### HIPAA (Health Insurance Portability and Accountability Act)

```bash
# HIPAA compliance documentation
aegis generate --framework hipaa --entity covered --output hipaa-docs/
```

**Generates**:

- Administrative safeguards documentation
- Physical safeguards procedures
- Technical safeguards implementation
- Business Associate Agreements templates
- Risk assessment and audit procedures

### Custom Frameworks

```bash
# Define custom compliance requirements
aegis generate --framework custom --template my-requirements.yml
```

---

## 🎯 Use Cases & Examples

### Use Case 1: Startup Compliance Preparation

**Scenario**: Fast-growing startup needs SOC2 Type II for enterprise sales

```bash
# 1. Initialize for SOC2
aegis init --framework soc2 --template startup --interactive

# 2. Generate initial documentation
aegis generate --framework soc2 --type type1 --output compliance/

# 3. Scan current security posture
aegis scan --frameworks soc2 --recommendations --output assessment/

# 4. Create remediation plan
aegis gap-analysis --framework soc2 --remediation --output gap-analysis/

# 5. Set up monitoring
aegis monitor --schedule weekly --alerts email:security@company.com
```

**Result**: Complete SOC2 preparation in days instead of months.

### Use Case 2: Enterprise Multi-Framework Compliance

**Scenario**: Large enterprise needs compliance with multiple frameworks

```bash
# Multi-framework compliance suite
aegis generate --frameworks soc2,iso27001,gdpr --output compliance-suite/

# Executive dashboard
aegis report --audience executive --frameworks all --dashboard

# Quarterly compliance report
aegis report --period Q1-2025 --metrics --charts --format pptx
```

### Use Case 3: Continuous Compliance Monitoring

**Scenario**: Maintain ongoing compliance with automated monitoring

```bash
# Set up comprehensive monitoring
aegis monitor \
  --frameworks soc2,iso27001 \
  --schedule daily \
  --integrations slack,jira,email \
  --dashboard

# Automated monthly reports
aegis report \
  --schedule monthly \
  --audience "executives,security-team" \
  --format pdf \
  --auto-distribute
```

### Use Case 4: Audit Preparation

**Scenario**: Preparing for external security audit

```bash
# Create audit-ready package
aegis audit-package \
  --framework soc2 \
  --auditor "BigFour Audit Firm" \
  --period "2025-Q1-Q4" \
  --evidence-map \
  --controls-matrix

# Generate auditor presentation
aegis report \
  --audience audit \
  --format pptx \
  --include-metrics \
  --executive-summary
```

---

## ⚙️ Configuration

### Project Configuration

Create `.aegis.yml` in your project root:

```yaml
# .aegis.yml
organization:
  name: "My Company Inc."
  industry: "Software Development"
  size: "startup"  # startup, small, medium, large, enterprise
  region: "US"
  
compliance:
  primary_framework: "soc2"
  target_frameworks: ["soc2", "iso27001"]
  audit_cycle: "annual"
  
security:
  classification_levels: ["public", "internal", "confidential", "restricted"]
  data_retention_policy: "7_years"
  encryption_requirements: "aes256"
  
reporting:
  executive_frequency: "monthly"
  technical_frequency: "weekly"
  audit_frequency: "quarterly"
  
integrations:
  jira:
    url: "https://company.atlassian.net"
    project: "SEC"
  slack:
    channel: "#security-alerts"
  email:
    security_team: "security@company.com"
    executives: "executives@company.com"

templates:
  policies: "enterprise"
  procedures: "detailed"
  reports: "professional"
  
evidence:
  collection_schedule: "weekly"
  storage_location: "evidence/"
  retention_period: "7_years"
```

### Global Configuration

Configure user-specific settings:

```bash
# Set organization defaults
aegis config set organization.name "My Company"
aegis config set organization.industry "technology"

# Set preferred templates
aegis config set templates.default "enterprise"
aegis config set reports.format "pdf"

# Set integration credentials
aegis config set integrations.slack.webhook "https://hooks.slack.com/..."
aegis config set integrations.jira.token "your-jira-token"
```

### Environment Variables

```bash
# API Keys for enhanced features
export AEGIS_API_KEY="your-api-key"
export AEGIS_ORG_ID="your-organization-id"

# Integration credentials
export SLACK_WEBHOOK_URL="https://hooks.slack.com/..."
export JIRA_API_TOKEN="your-jira-token"
export JIRA_BASE_URL="https://company.atlassian.net"

# Output preferences
export AEGIS_DEFAULT_FORMAT="pdf"
export AEGIS_OUTPUT_DIR="./compliance-docs"
```

---

## 🔗 Integrations

### CI/CD Integration

#### GitHub Actions

```yaml
# .github/workflows/security-compliance.yml
name: Security Compliance Check

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  compliance-check:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Install Aegis CLI
      run: pip install aegis-cli
      
    - name: Run compliance scan
      run: |
        aegis scan --frameworks soc2 --format json --output compliance-scan.json
        
    - name: Generate compliance report
      run: |
        aegis report --format html --output compliance-report.html
        
    - name: Upload compliance artifacts
      uses: actions/upload-artifact@v3
      with:
        name: compliance-reports
        path: |
          compliance-scan.json
          compliance-report.html
```

#### GitLab CI

```yaml
# .gitlab-ci.yml
compliance-check:
  stage: test
  image: python:3.9
  script:
    - pip install aegis-cli
    - aegis scan --frameworks soc2,iso27001 --output compliance/
    - aegis report --audience technical --format html
  artifacts:
    paths:
      - compliance/
    expire_in: 30 days
```

### Notification Integrations

#### Slack Integration

```bash
# Configure Slack alerts
aegis config set integrations.slack.webhook "https://hooks.slack.com/..."
aegis config set integrations.slack.channel "#security"

# Test integration
aegis test-integration slack
```

#### Jira Integration

```bash
# Configure Jira for issue tracking
aegis config set integrations.jira.url "https://company.atlassian.net"
aegis config set integrations.jira.project "SEC"
aegis config set integrations.jira.token "your-api-token"

# Create compliance issues automatically
aegis scan --create-jira-issues --priority "High"
```

### Monitoring Integrations

#### Prometheus Metrics

```bash
# Export metrics for Prometheus
aegis metrics --export prometheus --endpoint http://localhost:9090
```

#### Grafana Dashboard

```bash
# Generate Grafana dashboard
aegis dashboard --platform grafana --output aegis-security-dashboard.json
```

---

## 📊 Templates & Customization

### Built-in Templates

#### Startup Template

```bash
aegis policies --template startup
```

- Lightweight policies suitable for small teams
- Essential security controls
- Growth-focused compliance approach

#### Enterprise Template

```bash
aegis policies --template enterprise
```

- Comprehensive policy framework
- Advanced security controls
- Multi-framework compliance support

#### Government Template

```bash
aegis policies --template government
```

- FedRAMP compliance focus
- NIST framework alignment
- Enhanced security requirements

### Organization-Specific Templates

Create custom templates for your organization:

```yaml
# templates/my-org-template.yml
name: "My Organization Security Template"
version: "1.0.0"
description: "Custom security policies for My Organization"

organization:
  name: "{{org_name}}"
  industry: "{{industry}}"
  
policies:
  information_security:
    title: "Information Security Policy"
    sections:
      - purpose_and_scope
      - responsibilities
      - access_control
      - data_classification
      - incident_response
    
  data_protection:
    title: "Data Protection Policy"
    gdpr_compliance: true
    sections:
      - data_processing_principles
      - lawful_basis
      - data_subject_rights
      - international_transfers

procedures:
  incident_response:
    escalation_matrix:
      - level: "Low"
        response_time: "4 hours"
        contacts: ["security@company.com"]
      - level: "High"
        response_time: "1 hour"
        contacts: ["security@company.com", "ciso@company.com"]
        
controls:
  access_control:
    - id: "AC-001"
      title: "User Access Management"
      description: "Processes for managing user access"
      implementation: "{{access_control_implementation}}"
```

### Template Variables

Use variables for organization-specific customization:

```bash
# Generate with custom variables
aegis generate \
  --template my-org-template.yml \
  --var org_name="Acme Corp" \
  --var industry="software" \
  --var contact_email="security@acme.com"
```

---

## 🔧 Advanced Features

### Evidence Management

```bash
# Set up evidence collection
aegis evidence init --storage-type s3 --bucket compliance-evidence

# Collect evidence automatically
aegis evidence collect --type screenshots,logs,configs --schedule weekly

# Map evidence to controls
aegis evidence map --framework soc2 --control CC6.1 --evidence access-logs/
```

### Risk Assessment

```bash
# Automated risk assessment
aegis risk-assess --methodology nist --output risk-assessment.pdf

# Risk register management
aegis risk register --add "Unauthorized access to customer data" --impact High --likelihood Medium

# Risk monitoring
aegis risk monitor --dashboard --alerts high,critical
```

### Audit Trail

```bash
# Enable comprehensive audit logging
aegis audit enable --log-level detailed --storage audit-logs/

# Generate audit trail report
aegis audit report --period "2025-01-01:2025-12-31" --format timeline

# Export for external auditors
aegis audit export --auditor "External Audit Firm" --format excel
```

---

## 📈 Metrics & Analytics

### Security Metrics Dashboard

```bash
# Generate security metrics
aegis metrics --dashboard --realtime
```

**Key Metrics**:

- Compliance score by framework
- Control implementation status
- Risk posture over time
- Incident response times
- Policy compliance rates

### Compliance Scoring

```bash
# Get compliance score
aegis score --framework soc2 --detailed

# Track score over time
aegis score --historical --period "last-6-months" --chart
```

### Executive Reporting

```bash
# Monthly executive report
aegis report \
  --audience executive \
  --metrics \
  --trends \
  --recommendations \
  --format pptx
```

---

## 🤝 Contributing

We welcome contributions to Aegis CLI! Here's how you can help:

### Development Setup

```bash
# Clone the repository
git clone https://github.com/adaptive-intelligence/aegis-cli.git
cd aegis-cli

# Set up development environment
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -e ".[dev]"

# Run tests
pytest

# Run linting
flake8 aegis/
black aegis/
```

### Adding New Frameworks

1. Create framework definition in `aegis/frameworks/`
2. Add templates in `aegis/templates/frameworks/`
3. Update documentation
4. Add tests
5. Submit pull request

### Template Contributions

Share your organization templates:

1. Create template in `templates/community/`
2. Add documentation
3. Test with sample organization
4. Submit pull request

---

## 📞 Support

### Community Support

- **GitHub Issues**: [Report bugs and request features](https://github.com/adaptive-intelligence/aegis-cli/issues)
- **Discussions**: [Community Q&A](https://github.com/adaptive-intelligence/aegis-cli/discussions)
- **Discord**: [Join our community](https://discord.gg/adaptive-intel)

### Documentation

- **User Guide**: Complete usage documentation
- **API Reference**: Programmatic interface documentation
- **Examples**: Real-world use cases and templates
- **Video Tutorials**: Step-by-step video guides

### Professional Support

- **Email**: <support@adaptiveintel.dev>
- **Enterprise**: Dedicated support for enterprise customers
- **Consulting**: Professional services for compliance implementation

---

## 📄 Legal & License

- **License**: [MIT License](../../LICENSE)
- **Security**: [Security Policy](../../SECURITY.md)
- **Privacy**: We respect your privacy and security
- **Compliance**: Aegis CLI helps you achieve compliance but does not guarantee it

---

## 🏆 Recognition

- **Winner**: DevSecOps Tool of the Year 2025
- **Featured**: OWASP Top 10 Security Tools
- **Endorsed**: Information Security practitioners worldwide

---
