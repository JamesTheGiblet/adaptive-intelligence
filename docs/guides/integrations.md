# Integration Guide

This guide covers integrating the Adaptive Intelligence Platform with CI/CD systems, development tools, and third-party services.

## GitHub Actions Integration

### Basic Security Workflow

```yaml
# .github/workflows/adaptive-intelligence.yml
name: Adaptive Intelligence Quality Gate

on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main]

jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Run Whisper Secret Scan
        run: |
          pip install adaptive-whisper
          whisper --scan . --fail-on-found
      
      - name: Check Security Docs
        run: |
          pip install praximous-aegis-cli
          aegis status . --require-fresh

  quality-checks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Generate Tests
        run: |
          curl -X POST ${{ secrets.PRAXIMOUS_URL }}/api/v1/process \
            -H "Authorization: Bearer ${{ secrets.PRAXIMOUS_API_KEY }}" \
            -d '{"task_type": "generate_tests", "path": "src/"}'
      
      - name: Verify External Links
        run: |
          curl -X POST ${{ secrets.PRAXIMOUS_URL }}/api/v1/process \
            -H "Authorization: Bearer ${{ secrets.PRAXIMOUS_API_KEY }}" \
            -d '{"task_type": "certiscope_verify", "target": "docs/"}'

  ai-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: AI Code Review
        run: |
          curl -X POST ${{ secrets.PRAXIMOUS_URL }}/api/v1/process \
            -H "Authorization: Bearer ${{ secrets.PRAXIMOUS_API_KEY }}" \
            -d '{
              "task_type": "chameleon_transform",
              "domain": "senior_code_reviewer",
              "prompt": "Review PR for issues",
              "context": "${{ github.event.pull_request.diff_url }}"
            }'
```

### Enterprise Workflow with AAS

```yaml
# .github/workflows/enterprise-security.yml
name: Enterprise Security Gate

on:
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM
  workflow_dispatch:

jobs:
  comprehensive-audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Submit AAS Workflow
        run: |
          curl -X POST ${{ secrets.AAS_URL }}/api/v1/workflows \
            -H "Authorization: Bearer ${{ secrets.AAS_API_KEY }}" \
            -d '{
              "workflow": "security-audit",
              "target": "${{ github.repository }}",
              "params": {
                "branch": "${{ github.ref_name }}",
                "commit": "${{ github.sha }}"
              }
            }'
      
      - name: Wait for Completion
        run: |
          workflow_id=$(curl -s "${{ secrets.AAS_URL }}/api/v1/workflows/latest" | jq -r '.id')
          while true; do
            status=$(curl -s "${{ secrets.AAS_URL }}/api/v1/workflows/$workflow_id/status" | jq -r '.status')
            if [ "$status" = "completed" ]; then break; fi
            if [ "$status" = "failed" ]; then exit 1; fi
            sleep 30
          done
      
      - name: Download Report
        run: |
          workflow_id=$(curl -s "${{ secrets.AAS_URL }}/api/v1/workflows/latest" | jq -r '.id')
          curl -o security-report.json "${{ secrets.AAS_URL }}/api/v1/workflows/$workflow_id/report"
      
      - name: Upload Artifacts
        uses: actions/upload-artifact@v3
        with:
          name: security-report
          path: security-report.json
```

## GitLab CI/CD Integration

### Basic Pipeline

```yaml
# .gitlab-ci.yml
stages:
  - security
  - quality
  - deploy

security_scan:
  stage: security
  script:
    - pip install adaptive-whisper praximous-aegis-cli
    - whisper --scan . --fail-on-found
    - aegis status . --check-freshness
  only:
    - merge_requests
    - main

quality_checks:
  stage: quality
  script:
    - |
      curl -X POST $PRAXIMOUS_URL/api/v1/process \
        -H "Authorization: Bearer $PRAXIMOUS_API_KEY" \
        -d '{"task_type": "generate_tests", "path": "src/"}'
  only:
    - merge_requests

deploy_with_gate:
  stage: deploy
  script:
    - |
      curl -X POST $AAS_URL/api/v1/tasks \
        -H "Authorization: Bearer $AAS_API_KEY" \
        -d '{
          "type": "deployment_gate",
          "target": "$CI_PROJECT_DIR",
          "environment": "production"
        }'
  only:
    - main
  when: manual
```

### Advanced Pipeline with Parallel Jobs

```yaml
# .gitlab-ci.yml
variables:
  PRAXIMOUS_URL: "https://ai-gateway.company.com"
  AAS_URL: "https://aas.company.com"

.auth_template: &auth
  before_script:
    - export AUTH_HEADER="Authorization: Bearer $PRAXIMOUS_API_KEY"

security_comprehensive:
  stage: security
  <<: *auth
  parallel:
    matrix:
      - SCAN_TYPE: ["secrets", "dependencies", "code_quality"]
  script:
    - |
      case $SCAN_TYPE in
        secrets)
          whisper --scan . --output whisper-report.json
          ;;
        dependencies)
          aegis --scan-dependencies --output deps-report.json
          ;;
        code_quality)
          curl -X POST $PRAXIMOUS_URL/api/v1/process \
            -H "$AUTH_HEADER" \
            -d '{"task_type": "code_quality_scan", "path": "."}'
          ;;
      esac
  artifacts:
    reports:
      junit: "*-report.json"
    expire_in: 1 week
```

## Jenkins Integration

### Pipeline as Code

```groovy
// Jenkinsfile
pipeline {
    agent any
    
    environment {
        PRAXIMOUS_URL = credentials('praximous-url')
        PRAXIMOUS_API_KEY = credentials('praximous-api-key')
        AAS_URL = credentials('aas-url')
        AAS_API_KEY = credentials('aas-api-key')
    }
    
    stages {
        stage('Security Scan') {
            parallel {
                stage('Secret Detection') {
                    steps {
                        sh 'pip install adaptive-whisper'
                        sh 'whisper --scan . --output whisper-report.json'
                    }
                }
                
                stage('Security Docs Check') {
                    steps {
                        sh 'pip install praximous-aegis-cli'
                        sh 'aegis status . --check-freshness'
                    }
                }
                
                stage('Dependency Scan') {
                    steps {
                        sh 'aegis --scan-dependencies'
                    }
                }
            }
        }
        
        stage('Quality Checks') {
            steps {
                script {
                    def response = sh(
                        script: """
                            curl -X POST ${PRAXIMOUS_URL}/api/v1/process \
                              -H "Authorization: Bearer ${PRAXIMOUS_API_KEY}" \
                              -d '{"task_type": "generate_tests", "path": "src/"}'
                        """,
                        returnStdout: true
                    )
                    echo "Tests generated: ${response}"
                }
            }
        }
        
        stage('AI Review') {
            when {
                changeRequest()
            }
            steps {
                script {
                    def review = sh(
                        script: """
                            curl -X POST ${PRAXIMOUS_URL}/api/v1/process \
                              -H "Authorization: Bearer ${PRAXIMOUS_API_KEY}" \
                              -d '{
                                "task_type": "chameleon_transform",
                                "domain": "code_reviewer",
                                "prompt": "Review changes"
                              }'
                        """,
                        returnStdout: true
                    )
                    echo "AI Review: ${review}"
                }
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                script {
                    // Submit deployment workflow to AAS
                    def workflowResponse = sh(
                        script: """
                            curl -X POST ${AAS_URL}/api/v1/workflows \
                              -H "Authorization: Bearer ${AAS_API_KEY}" \
                              -d '{
                                "workflow": "deployment-pipeline",
                                "target": "${env.WORKSPACE}",
                                "params": {
                                  "branch": "${env.BRANCH_NAME}",
                                  "build_number": "${env.BUILD_NUMBER}"
                                }
                              }'
                        """,
                        returnStdout: true
                    )
                    
                    def workflowId = readJSON(text: workflowResponse).id
                    
                    // Wait for completion
                    timeout(time: 30, unit: 'MINUTES') {
                        waitUntil {
                            script {
                                def status = sh(
                                    script: "curl -s ${AAS_URL}/api/v1/workflows/${workflowId}/status",
                                    returnStdout: true
                                )
                                def statusObj = readJSON(text: status)
                                return statusObj.status == 'completed' || statusObj.status == 'failed'
                            }
                        }
                    }
                }
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: '*.json', fingerprint: true
            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'whisper-report.json',
                reportName: 'Security Report'
            ])
        }
    }
}
```

## Azure DevOps Integration

### Azure Pipelines

```yaml
# azure-pipelines.yml
trigger:
  branches:
    include:
    - main
    - develop

pr:
  branches:
    include:
    - main

variables:
  praximousUrl: 'https://ai-gateway.company.com'
  aasUrl: 'https://aas.company.com'

stages:
- stage: Security
  displayName: 'Security Checks'
  jobs:
  - job: SecurityScan
    displayName: 'Security Scan'
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.9'
    
    - script: |
        pip install adaptive-whisper praximous-aegis-cli
        whisper --scan . --fail-on-found
        aegis status . --check-freshness
      displayName: 'Run Security Tools'
    
    - task: PublishTestResults@2
      condition: always()
      inputs:
        testResultsFiles: '**/whisper-report.xml'
        testRunTitle: 'Security Scan Results'

- stage: Quality
  displayName: 'Quality Assurance'
  dependsOn: Security
  condition: succeeded()
  jobs:
  - job: QualityChecks
    displayName: 'Quality Checks'
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - script: |
        curl -X POST $(praximousUrl)/api/v1/process \
          -H "Authorization: Bearer $(PRAXIMOUS_API_KEY)" \
          -d '{"task_type": "generate_tests", "path": "src/"}'
      displayName: 'Generate Tests'
    
    - script: |
        curl -X POST $(praximousUrl)/api/v1/process \
          -H "Authorization: Bearer $(PRAXIMOUS_API_KEY)" \
          -d '{"task_type": "certiscope_verify", "target": "docs/"}'
      displayName: 'Verify Documentation Links'

- stage: Deploy
  displayName: 'Deployment'
  dependsOn: Quality
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - deployment: DeployProduction
    displayName: 'Deploy to Production'
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - script: |
              workflow_response=$(curl -X POST $(aasUrl)/api/v1/workflows \
                -H "Authorization: Bearer $(AAS_API_KEY)" \
                -d '{
                  "workflow": "production-deployment",
                  "target": "$(Build.Repository.Name)",
                  "params": {
                    "build_id": "$(Build.BuildId)",
                    "branch": "$(Build.SourceBranchName)"
                  }
                }')
              echo "##vso[task.setvariable variable=workflowId]$(echo $workflow_response | jq -r '.id')"
            displayName: 'Submit Deployment Workflow'
          
          - script: |
              while true; do
                status=$(curl -s $(aasUrl)/api/v1/workflows/$(workflowId)/status | jq -r '.status')
                echo "Deployment status: $status"
                case $status in
                  completed) echo "Deployment successful"; exit 0 ;;
                  failed) echo "Deployment failed"; exit 1 ;;
                  *) sleep 30 ;;
                esac
              done
            displayName: 'Wait for Deployment Completion'
```

## Slack Integration

### Bot Setup

```python
# slack_bot.py
import os
import json
import requests
from slack_sdk import WebClient
from slack_sdk.errors import SlackApiError

class AdaptiveIntelligenceBot:
    def __init__(self):
        self.client = WebClient(token=os.environ["SLACK_BOT_TOKEN"])
        self.praximous_url = os.environ["PRAXIMOUS_URL"]
        self.praximous_key = os.environ["PRAXIMOUS_API_KEY"]
    
    def handle_security_scan_request(self, channel, user, repo_url):
        """Handle security scan requests from Slack"""
        try:
            # Submit scan to Praximous
            response = requests.post(
                f"{self.praximous_url}/api/v1/process",
                headers={"Authorization": f"Bearer {self.praximous_key}"},
                json={
                    "task_type": "comprehensive_security_scan",
                    "target": repo_url,
                    "options": {
                        "include_tests": True,
                        "verify_docs": True
                    }
                }
            )
            
            if response.status_code == 200:
                result = response.json()
                self.send_security_report(channel, result)
            else:
                self.send_error_message(channel, "Security scan failed")
                
        except Exception as e:
            self.send_error_message(channel, f"Error: {str(e)}")
    
    def send_security_report(self, channel, report):
        """Send formatted security report to Slack"""
        blocks = [
            {
                "type": "header",
                "text": {
                    "type": "plain_text",
                    "text": "🔒 Security Scan Complete"
                }
            },
            {
                "type": "section",
                "fields": [
                    {
                        "type": "mrkdwn",
                        "text": f"*Secrets Found:* {report.get('secrets_found', 0)}"
                    },
                    {
                        "type": "mrkdwn",
                        "text": f"*Security Score:* {report.get('security_score', 'N/A')}/10"
                    },
                    {
                        "type": "mrkdwn",
                        "text": f"*Files Scanned:* {report.get('files_scanned', 0)}"
                    },
                    {
                        "type": "mrkdwn",
                        "text": f"*Scan Time:* {report.get('scan_time', 'N/A')}"
                    }
                ]
            }
        ]
        
        if report.get('critical_issues'):
            blocks.append({
                "type": "section",
                "text": {
                    "type": "mrkdwn",
                    "text": f"⚠️ *Critical Issues:* {len(report['critical_issues'])}"
                }
            })
        
        try:
            self.client.chat_postMessage(
                channel=channel,
                blocks=blocks,
                text="Security scan complete"
            )
        except SlackApiError as e:
            print(f"Error posting message: {e}")

# Slack command handlers
@app.command("/security-scan")
def security_scan_command(ack, respond, command):
    ack()
    
    repo_url = command['text'].strip()
    if not repo_url:
        respond("Please provide a repository URL")
        return
    
    bot = AdaptiveIntelligenceBot()
    bot.handle_security_scan_request(
        command['channel_id'],
        command['user_id'],
        repo_url
    )
    
    respond("Security scan started... Results will be posted shortly.")

@app.command("/ai-review")
def ai_review_command(ack, respond, command):
    ack()
    
    # Handle AI code review requests
    # Similar implementation to security scan
    pass
```

### Webhook Integration

```python
# webhook_handler.py
from flask import Flask, request, jsonify
import json
import requests
import os

app = Flask(__name__)

@app.route('/webhook/security-alert', methods=['POST'])
def handle_security_alert():
    """Handle security alerts from AAS and forward to Slack"""
    
    data = request.json
    
    if data.get('severity') == 'critical':
        send_slack_alert(data)
    
    return jsonify({"status": "received"})

def send_slack_alert(alert_data):
    """Send critical security alert to Slack"""
    
    webhook_url = os.environ["SLACK_WEBHOOK_URL"]
    
    message = {
        "text": "🚨 Critical Security Alert",
        "attachments": [
            {
                "color": "danger",
                "fields": [
                    {
                        "title": "Issue Type",
                        "value": alert_data.get('issue_type', 'Unknown'),
                        "short": True
                    },
                    {
                        "title": "Repository",
                        "value": alert_data.get('repository', 'Unknown'),
                        "short": True
                    },
                    {
                        "title": "Description",
                        "value": alert_data.get('description', 'No description'),
                        "short": False
                    },
                    {
                        "title": "Action Required",
                        "value": alert_data.get('action_required', 'Review immediately'),
                        "short": False
                    }
                ],
                "footer": "Adaptive Intelligence Platform",
                "ts": alert_data.get('timestamp')
            }
        ]
    }
    
    requests.post(webhook_url, json=message)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

## Jira Integration

### Issue Creation

```python
# jira_integration.py
from jira import JIRA
import os
import json

class JiraIntegration:
    def __init__(self):
        self.jira = JIRA(
            server=os.environ["JIRA_SERVER"],
            basic_auth=(
                os.environ["JIRA_USERNAME"],
                os.environ["JIRA_API_TOKEN"]
            )
        )
        self.project_key = os.environ["JIRA_PROJECT_KEY"]
    
    def create_security_issue(self, scan_results):
        """Create Jira issue for security findings"""
        
        # Filter critical and high severity issues
        critical_issues = [
            issue for issue in scan_results.get('issues', [])
            if issue.get('severity') in ['critical', 'high']
        ]
        
        if not critical_issues:
            return None
        
        description = self.format_security_description(critical_issues)
        
        issue_dict = {
            'project': {'key': self.project_key},
            'summary': f"Security Issues Found - {scan_results.get('repository', 'Unknown')}",
            'description': description,
            'issuetype': {'name': 'Bug'},
            'priority': {'name': 'High'},
            'labels': ['security', 'automated', 'adaptive-intelligence']
        }
        
        try:
            new_issue = self.jira.create_issue(fields=issue_dict)
            return new_issue.key
        except Exception as e:
            print(f"Error creating Jira issue: {e}")
            return None
    
    def format_security_description(self, issues):
        """Format security issues for Jira description"""
        
        description = "Automated security scan found the following issues:\n\n"
        
        for issue in issues:
            description += f"*{issue.get('type', 'Unknown')}* - {issue.get('severity', 'unknown').upper()}\n"
            description += f"File: {issue.get('file', 'N/A')}\n"
            description += f"Line: {issue.get('line', 'N/A')}\n"
            description += f"Description: {issue.get('description', 'No description')}\n"
            description += f"Recommendation: {issue.get('recommendation', 'Manual review required')}\n\n"
        
        description += "Generated by Adaptive Intelligence Platform\n"
        description += f"Scan completed at: {issues[0].get('scan_time', 'Unknown')}"
        
        return description

# Webhook handler for AAS integration
@app.route('/webhook/jira', methods=['POST'])
def handle_jira_webhook():
    """Handle security scan results and create Jira issues"""
    
    data = request.json
    
    if data.get('task_type') == 'security_scan' and data.get('status') == 'completed':
        jira_integration = JiraIntegration()
        issue_key = jira_integration.create_security_issue(data.get('results', {}))
        
        if issue_key:
            return jsonify({"status": "issue_created", "issue_key": issue_key})
        else:
            return jsonify({"status": "no_issues_found"})
    
    return jsonify({"status": "ignored"})
```

## VS Code Extension

### Extension Manifest

```json
{
  "name": "adaptive-intelligence-vscode",
  "displayName": "Adaptive Intelligence",
  "description": "AI-powered development automation tools",
  "version": "1.0.0",
  "engines": {
    "vscode": "^1.60.0"
  },
  "categories": ["Other"],
  "activationEvents": [
    "onCommand:adaptive-intelligence.scanSecrets",
    "onCommand:adaptive-intelligence.generateTests",
    "onCommand:adaptive-intelligence.aiReview"
  ],
  "main": "./out/extension.js",
  "contributes": {
    "commands": [
      {
        "command": "adaptive-intelligence.scanSecrets",
        "title": "Scan for Secrets",
        "category": "Adaptive Intelligence"
      },
      {
        "command": "adaptive-intelligence.generateTests",
        "title": "Generate Tests",
        "category": "Adaptive Intelligence"
      },
      {
        "command": "adaptive-intelligence.aiReview",
        "title": "AI Code Review",
        "category": "Adaptive Intelligence"
      }
    ],
    "configuration": {
      "title": "Adaptive Intelligence",
      "properties": {
        "adaptiveIntelligence.praximousUrl": {
          "type": "string",
          "default": "http://localhost:8000",
          "description": "Praximous gateway URL"
        },
        "adaptiveIntelligence.apiKey": {
          "type": "string",
          "description": "API key for authentication"
        }
      }
    }
  }
}
```

### Extension Implementation

```typescript
// extension.ts
import * as vscode from 'vscode';
import axios from 'axios';

export function activate(context: vscode.ExtensionContext) {
    
    const scanSecretsCommand = vscode.commands.registerCommand(
        'adaptive-intelligence.scanSecrets',
        async () => {
            const config = vscode.workspace.getConfiguration('adaptiveIntelligence');
            const apiUrl = config.get<string>('praximousUrl');
            const apiKey = config.get<string>('apiKey');
            
            if (!apiUrl || !apiKey) {
                vscode.window.showErrorMessage('Please configure Adaptive Intelligence settings');
                return;
            }
            
            const workspaceFolder = vscode.workspace.workspaceFolders?.[0];
            if (!workspaceFolder) {
                vscode.window.showErrorMessage('No workspace folder open');
                return;
            }
            
            vscode.window.withProgress({
                location: vscode.ProgressLocation.Notification,
                title: "Scanning for secrets...",
                cancellable: false
            }, async () => {
                try {
                    const response = await axios.post(`${apiUrl}/api/v1/process`, {
                        task_type: 'whisper_scan',
                        target_path: workspaceFolder.uri.fsPath
                    }, {
                        headers: {
                            'Authorization': `Bearer ${apiKey}`,
                            'Content-Type': 'application/json'
                        }
                    });
                    
                    const results = response.data;
                    
                    if (results.secrets_found > 0) {
                        vscode.window.showWarningMessage(
                            `Found ${results.secrets_found} potential secrets. Check the Problems panel.`
                        );
                        
                        // Add diagnostics to Problems panel
                        const diagnostics: vscode.Diagnostic[] = [];
                        results.results.forEach((issue: any) => {
                            const uri = vscode.Uri.file(issue.file);
                            const range = new vscode.Range(
                                issue.line - 1, 0,
                                issue.line - 1, 100
                            );
                            const diagnostic = new vscode.Diagnostic(
                                range,
                                `Potential secret detected: ${issue.type}`,
                                vscode.DiagnosticSeverity.Warning
                            );
                            diagnostics.push(diagnostic);
                        });
                        
                        // You would need to implement a DiagnosticCollection here
                        
                    } else {
                        vscode.window.showInformationMessage('No secrets found!');
                    }
                } catch (error) {
                    vscode.window.showErrorMessage(`Scan failed: ${error}`);
                }
            });
        }
    );
    
    context.subscriptions.push(scanSecretsCommand);
}
```

## Custom Integrations

### REST API Client

```python
# api_client.py
import requests
import json
from typing import Dict, Any, Optional

class AdaptiveIntelligenceClient:
    def __init__(self, base_url: str, api_key: str):
        self.base_url = base_url.rstrip('/')
        self.api_key = api_key
        self.session = requests.Session()
        self.session.headers.update({
            'Authorization': f'Bearer {api_key}',
            'Content-Type': 'application/json'
        })
    
    def scan_secrets(self, path: str, **options) -> Dict[str, Any]:
        """Scan for secrets in the given path"""
        return self._post('/api/v1/process', {
            'task_type': 'whisper_scan',
            'target_path': path,
            'options': options
        })
    
    def generate_tests(self, code: str, language: str, framework: str) -> Dict[str, Any]:
        """Generate tests for the given code"""
        return self._post('/api/v1/process', {
            'task_type': 'generate_tests',
            'code': code,
            'language': language,
            'framework': framework
        })
    
    def ai_code_review(self, code: str, context: str = "") -> Dict[str, Any]:
        """Get AI code review"""
        return self._post('/api/v1/process', {
            'task_type': 'chameleon_transform',
            'domain': 'senior_code_reviewer',
            'prompt': f'Review this code: {code}',
            'context': context
        })
    
    def verify_url_credibility(self, url: str) -> Dict[str, Any]:
        """Verify URL credibility"""
        return self._post('/api/v1/process', {
            'task_type': 'certiscope_verify',
            'url': url
        })
    
    def analyze_project_structure(self, path: str) -> Dict[str, Any]:
        """Analyze project structure"""
        return self._post('/api/v1/process', {
            'task_type': 'treecraft_analyze',
            'path': path,
            'ai_analyze': True
        })
    
    def _post(self, endpoint: str, data: Dict[str, Any]) -> Dict[str, Any]:
        """Make POST request to API"""
        try:
            response = self.session.post(
                f'{self.base_url}{endpoint}',
                json=data
            )
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            raise Exception(f"API request failed: {e}")

# Usage example
if __name__ == "__main__":
    client = AdaptiveIntelligenceClient(
        "http://localhost:8000",
        "your-api-key-here"
    )
    
    # Scan for secrets
    secrets_result = client.scan_secrets("/path/to/project")
    print(f"Secrets found: {secrets_result.get('secrets_found', 0)}")
    
    # Generate tests
    test_result = client.generate_tests(
        "def add(a, b): return a + b",
        "python",
        "pytest"
    )
    print(f"Tests generated: {test_result.get('status')}")
```

For more integration examples and custom development patterns, see the [Architecture Guide](../architecture.md) and [Workflow Examples](../workflows.md).
