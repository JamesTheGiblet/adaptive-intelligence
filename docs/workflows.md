# Workflow Examples

This document shows how the different tools work together to solve real-world development problems.

## Workflow 1: Secure Project Initialization

**Scenario**: Starting a new Python project with security best practices

```bash
# 1. TreeCraft analyzes structure
POST /api/v1/process {"task_type": "treecraft_analyze", "path": "./my-project"}
→ Returns: Project structure, complexity analysis

# 2. Aegis generates security docs (using TreeCraft data)
POST /api/v1/process {"task_type": "aegis_initialize", "path": "./my-project"}
→ Creates: SECURITY.md, dependabot.yml, SecureCodingGuide.md

# 3. Whisper scans for existing secrets
POST /api/v1/process {"task_type": "whisper_scan", "path": "./my-project"}
→ Returns: Secret scan report (hopefully clean for new project)

# 4. EmbedID watermarks all files
POST /api/v1/process {"task_type": "embedid_sign", "path": "./my-project", "author": "MyTeam"}
→ Embeds: Authorship metadata in all source files

# Result: Secure, documented, attributed project ready for development
```

**Behind the Scenes:**

```text
User → AAS Task Queue → Agent 1 (TreeCraft skill)
                     → Agent 2 (Aegis skill)
                     → Agent 3 (Whisper skill)
                     → Agent 4 (EmbedID skill)
     → Results aggregated → Returned to user
```

## Workflow 2: AI-Assisted Code Review

**Scenario**: Reviewing a pull request with AI assistance

```bash
# 1. Chameleon adapts to "senior code reviewer" persona
POST /api/v1/process {
  "task_type": "chameleon_transform",
  "domain": "senior_python_developer",
  "prompt": "Review this PR for security issues",
  "context": "[PR diff content]"
}
→ Returns: AI code review with security recommendations

# 2. Test Generator creates tests for new code
POST /api/v1/process {
  "task_type": "generate_tests",
  "code": "[new code from PR]",
  "language": "python",
  "framework": "pytest"
}
→ Returns: Comprehensive test suite

# 3. Whisper scans PR for secrets
POST /api/v1/process {"task_type": "whisper_scan", "diff": "[PR diff]"}
→ Returns: Secret scan results

# 4. CertiScope verifies any external links in code/docs
POST /api/v1/process {
  "task_type": "certiscope_verify",
  "urls": ["https://docs.example.com"]
}
→ Returns: Credibility scores for referenced URLs

# Result: Comprehensive PR review with AI insights, tests, security checks, and verified sources
```

## Workflow 3: Documentation Generation Pipeline

**Scenario**: Creating complete project documentation

```bash
# 1. TreeCraft generates structure visualization
POST /api/v1/process {"task_type": "treecraft_analyze", "path": "."}
→ Returns: Project tree in Markdown format

# 2. MarkFlow creates README template with TreeCraft data
POST /api/v1/process {
  "task_type": "markflow_generate",
  "template": "readme",
  "context": {"structure": "[TreeCraft output]"}
}
→ Returns: README.md draft

# 3. Chameleon enhances documentation with AI
POST /api/v1/process {
  "task_type": "chameleon_transform",
  "domain": "technical_writer",
  "prompt": "Improve this README",
  "context": "[MarkFlow output]"
}
→ Returns: Polished README.md

# 4. CertiScope verifies all documentation links
POST /api/v1/process {
  "task_type": "certiscope_verify",
  "content": "[Final README]"
}
→ Returns: Link verification report

# 5. Whisper scans before publishing
POST /api/v1/process {"task_type": "whisper_scan", "file": "README.md"}
→ Returns: Ensure no secrets leaked

# 6. EmbedID watermarks documentation
POST /api/v1/process {"task_type": "embedid_sign", "file": "README.md"}
→ Embeds: Authorship metadata

# Result: Professional, verified, secure documentation ready to publish
```

## Workflow 4: Continuous Security Monitoring

**Scenario**: Automated nightly security audit

```bash
# Scheduled via AAS (runs every night at 2 AM)

# 1. Whisper scans entire codebase
→ Detects: Any new secrets that were committed

# 2. Aegis checks if security docs are current
→ Updates: Security guidelines if language version changed

# 3. Test Generator creates security-focused tests
→ Generates: Penetration test scenarios

# 4. CertiScope verifies security resource links
→ Checks: All URLs in SECURITY.md are still valid

# 5. Chameleon analyzes findings as security expert
→ Provides: Risk assessment and recommendations

# 6. Human Fallback (if critical issues found)
→ Escalates: High-severity findings to security team via Copilot Bridge

# Result: Continuous security monitoring with human escalation only when needed
```

**Behind the Scenes (AAS Orchestration):**

```yaml
# scheduled_tasks.yml
security_audit:
  schedule: "0 2 * * *"  # 2 AM daily
  workflow:
    - skill: whisper_scan
      params: {path: "/app"}
    - skill: aegis_update
      params: {path: "/app"}
    - skill: generate_tests
      params: {test_type: "security", path: "/app"}
    - skill: certiscope_verify
      params: {target: "security/SECURITY.md"}
    - skill: chameleon_transform
      params: {domain: "security_analyst", context: "{{previous_results}}"}
  on_failure:
    action: escalate_to_human
    priority: high
    notify: ["security@company.com"]
```

## Workflow 5: New Developer Onboarding

**Scenario**: A new developer joins the team and needs to understand the codebase

```bash
# 1. TreeCraft generates visual project overview
POST /api/v1/process {"task_type": "treecraft_analyze", "path": ".", "ai_analyze": true}
→ Returns: Structure + AI insights on architecture patterns

# 2. MarkFlow creates onboarding guide
POST /api/v1/process {
  "task_type": "markflow_generate",
  "template": "onboarding",
  "context": {
    "structure": "[TreeCraft output]",
    "team": "Engineering"
  }
}
→ Returns: Customized onboarding document

# 3. Chameleon acts as interactive mentor
POST /api/v1/process {
  "task_type": "chameleon_transform",
  "domain": "senior_mentor",
  "prompt": "Explain the authentication flow in this codebase",
  "context": "[Project files]"
}
→ Returns: Detailed explanation in conversational tone

# 4. Test Generator shows example tests
POST /api/v1/process {
  "task_type": "generate_tests",
  "code": "[Core module code]",
  "include_comments": true
}
→ Returns: Annotated tests that demonstrate how the code works

# 5. EmbedID shows code lineage
embedid lineage src/core/auth.py
→ Returns: Who wrote what, when, and why (from commit history + watermarks)

# Result: New developer understands codebase architecture, has guided learning path,
#         and can see code evolution history
```

## Workflow 6: Pre-Deployment Security Gate

**Scenario**: Automated security checks before production deployment

```bash
# CI/CD Pipeline Integration

# Stage 1: Code Quality & Security
- name: Security Scan
  run: |
    # Scan for secrets
    whisper --scan . --fail-on-found
    
    # Verify security docs are current
    aegis status . --check-freshness
    
    # Scan dependencies for vulnerabilities
    aegis --scan-dependencies
    
# Stage 2: Verification & Testing
- name: Quality Checks
  run: |
    # Verify all external links in docs
    certiscope --input docs/ --min-score 7.0
    
    # Ensure test coverage is adequate
    test-generator --check-coverage --min 80%
    
# Stage 3: Compliance & Attribution
- name: Compliance
  run: |
    # Verify all code is properly watermarked
    embedid verify src/ --recursive --strict
    
    # Check license compliance
    embedid check-licenses
    
# Stage 4: AI Review (Enterprise Tier)
- name: AI Security Review
  run: |
    # Chameleon reviews changes as security expert
    POST /api/v1/process {
      "task_type": "chameleon_transform",
      "domain": "security_architect",
      "prompt": "Review deployment for security issues",
      "context": "{{git_diff}}"
    }

# If any stage fails → Deployment blocked → Alert sent to Copilot Bridge
```

## Workflow 7: Multi-Project Intelligence Dashboard

**Scenario**: CTO wants overview of security posture across 50 repositories

```bash
# AAS handles this as a federated workflow

# 1. TreeCraft analyzes all projects
for repo in repositories:
    POST /api/v1/process {
      "task_type": "treecraft_analyze",
      "target": repo,
      "metrics": ["complexity", "test_coverage", "doc_quality"]
    }

# 2. Whisper scans all repos for secrets
POST /api/v1/process {
  "task_type": "whisper_scan",
  "targets": repositories,
  "mode": "batch"
}
→ Returns: Aggregated secret findings across all repos

# 3. Aegis checks security documentation status
POST /api/v1/process {
  "task_type": "aegis_status",
  "targets": repositories,
  "generate_report": true
}
→ Returns: Which repos lack security docs, which are outdated

# 4. Test Generator analyzes test coverage
POST /api/v1/process {
  "task_type": "test_coverage_analysis",
  "targets": repositories
}
→ Returns: Coverage metrics, gaps, recommendations

# 5. EmbedID generates attribution report
embedid report --repos repositories --output compliance-report.pdf
→ Returns: License compliance, attribution tracking, provenance verification

# 6. Chameleon synthesizes executive summary
POST /api/v1/process {
  "task_type": "chameleon_transform",
  "domain": "cto_advisor",
  "prompt": "Create executive summary of security posture",
  "context": "{{all_previous_results}}"
}
→ Returns: High-level summary with actionable recommendations
```

**Result**: Comprehensive security dashboard showing:

- Repos with secrets (CRITICAL)
- Repos without security docs (HIGH)
- Test coverage gaps (MEDIUM)
- License compliance status (INFO)
- AI-generated action plan

## Workflow 8: AI-Powered Knowledge Base

**Scenario**: Team wants searchable, verified knowledge base of technical decisions

```bash
# 1. TreeCraft documents project structure over time
POST /api/v1/process {
  "task_type": "treecraft_analyze",
  "mode": "historical",
  "track_changes": true
}
→ Returns: How structure evolved, when major changes happened

# 2. EmbedID extracts authorship timeline
embedid timeline --format json --output knowledge-base/timeline.json
→ Returns: Who contributed what, when, and in what context

# 3. MarkFlow converts technical docs to searchable format
POST /api/v1/process {
  "task_type": "markflow_generate",
  "mode": "knowledge_base",
  "input": ["docs/", "README.md", "architecture/"]
}
→ Returns: Unified, searchable documentation

# 4. CertiScope verifies all external references
POST /api/v1/process {
  "task_type": "certiscope_verify",
  "targets": "knowledge-base/",
  "remove_dead_links": true,
  "suggest_alternatives": true
}
→ Returns: Updated docs with verified, current links

# 5. Chameleon creates interactive Q&A interface
POST /api/v1/process {
  "task_type": "chameleon_transform",
  "domain": "technical_librarian",
  "mode": "interactive",
  "knowledge_base": "knowledge-base/"
}
→ Enables: Natural language queries against knowledge base
```

**Example queries:**

- "Why did we choose PostgreSQL over MongoDB?"
  → Chameleon searches commit messages, doc changes, watermark metadata
- "Who knows most about our authentication system?"
  → EmbedID data + contribution history
- "Show me all architectural decisions from 2024"
  → TreeCraft historical data + git history

## Workflow 9: Automated Code Migration

**Scenario**: Migrating from Python 3.8 to Python 3.12

```bash
# 1. TreeCraft identifies all Python files and dependencies
POST /api/v1/process {
  "task_type": "treecraft_analyze",
  "language": "python",
  "extract_dependencies": true
}
→ Returns: Full inventory of Python files, imports, version-specific code

# 2. Chameleon analyzes compatibility issues
POST /api/v1/process {
  "task_type": "chameleon_transform",
  "domain": "python_migration_expert",
  "prompt": "Identify Python 3.12 breaking changes",
  "context": "{{treecraft_output}}"
}
→ Returns: List of files needing updates, specific changes required

# 3. Test Generator creates compatibility tests
POST /api/v1/process {
  "task_type": "generate_tests",
  "mode": "compatibility",
  "old_version": "3.8",
  "new_version": "3.12",
  "files": "{{affected_files}}"
}
→ Returns: Tests that verify code works on both versions

# 4. Aegis updates security guidelines
POST /api/v1/process {
  "task_type": "aegis_update",
  "trigger": "python_version_change",
  "old_version": "3.8",
  "new_version": "3.12"
}
→ Returns: Updated SecureCodingGuide.md with Python 3.12 best practices

# 5. Whisper scans for deprecated secret patterns
POST /api/v1/process {
  "task_type": "whisper_scan",
  "check_deprecated_patterns": true,
  "python_version": "3.12"
}
→ Returns: Secrets that might be exposed by new Python behavior

# 6. EmbedID tracks migration changes
embedid add src/ --metadata '{"migration": "py38_to_py312", "date": "2025-10-03"}'
→ Embeds: Migration metadata for future reference

# 7. Human Fallback for complex cases
# AAS identifies files that need manual review
POST /copilot/escalate {
  "task": "migration_review",
  "files": ["complex_async_code.py", "legacy_metaclass.py"],
  "reason": "Complex patterns detected, human review recommended"
}
→ Escalates: Difficult migrations to human developers via Copilot Bridge

# Result: Automated migration with human review only for complex cases
```

## Creating Custom Workflows

You can create your own workflows by combining the available skills. Here's how:

### 1. Define the Workflow

```yaml
# custom-workflow.yml
name: "Frontend Security Audit"
description: "Comprehensive security audit for React applications"
triggers:
  - schedule: "0 1 * * 1"  # Every Monday at 1 AM
  - webhook: "/api/webhooks/security-audit"
steps:
  - name: "Scan for secrets"
    skill: "whisper_scan"
    params:
      path: "./src"
      include: ["*.js", "*.jsx", "*.ts", "*.tsx"]
  - name: "Check security docs"
    skill: "aegis_status"
    params:
      path: "."
      framework: "react"
  - name: "Verify external links"
    skill: "certiscope_verify"
    params:
      target: "./README.md"
      min_score: 8.0
  - name: "Generate security tests"
    skill: "generate_tests"
    params:
      test_type: "security"
      framework: "jest"
on_failure:
  escalate_to_human: true
  notify: ["frontend-team@company.com"]
```

### 2. Submit to AAS

```bash
POST /api/v1/workflows
{
  "workflow": "custom-workflow.yml",
  "enabled": true
}
```

### 3. Monitor Results

```bash
GET /api/v1/workflows/frontend-security-audit/results
```

For more information on creating custom workflows, see the [AAS Documentation](architecture.md#layer-1-aas-autonomous-agent-system).
