# Security Policy

## Supported Versions

We actively maintain security updates for the following versions:

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |
| 0.9.x   | :white_check_mark: |
| 0.8.x   | :x:                |
| < 0.8   | :x:                |

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, please report them responsibly by emailing: **<security@adaptiveintel.dev>**

### What to Include

When reporting a security vulnerability, please include:

- **Description**: Clear description of the vulnerability
- **Impact**: What an attacker could achieve
- **Reproduction**: Step-by-step instructions to reproduce
- **Affected Components**: Which parts of the platform are affected
- **Suggested Fix**: If you have ideas for mitigation
- **Contact Information**: How we can reach you for follow-up

### Response Timeline

- **Initial Response**: Within 24 hours
- **Status Update**: Within 72 hours
- **Fix Timeline**: Varies by severity (see below)

### Severity Levels

#### Critical (CVSS 9.0-10.0)

- **Response**: Immediate (within hours)
- **Fix Target**: 24-48 hours
- **Examples**: Remote code execution, authentication bypass

#### High (CVSS 7.0-8.9)

- **Response**: Within 24 hours
- **Fix Target**: 7 days
- **Examples**: Privilege escalation, data exposure

#### Medium (CVSS 4.0-6.9)

- **Response**: Within 72 hours
- **Fix Target**: 30 days
- **Examples**: Information disclosure, limited DoS

#### Low (CVSS 0.1-3.9)

- **Response**: Within 1 week
- **Fix Target**: 90 days
- **Examples**: Minor information leaks

## Security Best Practices

### For Deployment

#### API Keys and Secrets

- Never commit API keys to version control
- Use environment variables or secret management systems
- Rotate keys regularly
- Use least-privilege access policies

#### Network Security

- Use HTTPS/TLS for all communications
- Implement proper firewall rules
- Use VPNs for internal access
- Enable rate limiting and DDoS protection

#### Container Security

- Use official base images
- Regularly update container images
- Scan images for vulnerabilities
- Run containers with non-root users
- Use security contexts and resource limits

#### Authentication & Authorization

- Enable multi-factor authentication (MFA)
- Implement role-based access control (RBAC)
- Use strong passwords and enforce policies
- Regularly audit user access

### For Development

#### Code Security

- Follow secure coding practices
- Use static analysis tools (bandit, semgrep)
- Implement input validation and sanitization
- Use parameterized queries for databases
- Handle errors securely (no sensitive info in error messages)

#### Dependencies

- Regularly update dependencies
- Use vulnerability scanning (safety, npm audit)
- Pin dependency versions
- Review third-party packages before use

#### AI/LLM Security

- Validate and sanitize all user inputs to LLMs
- Implement output filtering for sensitive information
- Use content filtering for inappropriate content
- Monitor for prompt injection attempts
- Implement rate limiting for AI services

## Security Features

### Built-in Security

#### Praximous Gateway

- Request validation and sanitization
- Rate limiting and throttling
- Authentication token validation
- Content filtering for AI responses
- Audit logging of all requests

#### AAS (Agent Autonomy System)

- Agent sandboxing and isolation
- Resource limits and monitoring
- Secure inter-agent communication
- Task validation and approval workflows

#### Intelligence Tools

- **Whisper**: Secret scanning and vulnerability detection
- **Aegis**: Security compliance framework
- **EmbedID**: Code watermarking for integrity
- **CertiScope**: Source credibility verification

### Security Monitoring

#### Logging and Auditing

- Comprehensive audit logs
- Security event monitoring
- Failed authentication tracking
- Anomaly detection

#### Metrics and Alerting

- Security-focused dashboards
- Real-time threat detection
- Automated incident response
- Integration with SIEM systems

## Vulnerability Disclosure Program

### Scope

Our vulnerability disclosure program covers:

- **Core Platform**: AAS, Praximous, core APIs
- **Intelligence Tools**: All tools in the platform
- **Documentation**: Security-relevant documentation
- **Infrastructure**: Deployment configurations and examples

### Out of Scope

- Third-party dependencies (report to respective maintainers)
- Social engineering attacks
- Physical security issues
- Denial of service attacks requiring excessive resources

### Recognition

We recognize security researchers through:

- **Security Advisory Credits**: Listed in security advisories
- **Hall of Fame**: Public recognition page
- **Swag**: Stickers and merchandise for significant findings
- **References**: Professional references for responsible disclosure

## Security Contacts

- **Primary**: <security@adaptiveintel.dev>
- **PGP Key**: Available at <https://adaptiveintel.dev/.well-known/security.txt>
- **Emergency**: For critical vulnerabilities requiring immediate attention

## Security Resources

### Documentation

- [Deployment Security Guide](docs/guides/deployment.md#security)
- [API Security Documentation](docs/api-security.md)
- [Container Security Best Practices](docs/container-security.md)

### Tools and Scripts

- **Security Scanner**: `scripts/security-scan.sh`
- **Vulnerability Checker**: `scripts/check-vulnerabilities.py`
- **Security Audit**: `scripts/security-audit.sh`

### External Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [CIS Controls](https://www.cisecurity.org/controls/)
- [AI Security Best Practices](https://owasp.org/www-project-ai-security-and-privacy-guide/)

## Compliance

The Adaptive Intelligence Platform is designed to support compliance with:

- **SOC 2 Type II**: Security and availability controls
- **ISO 27001**: Information security management
- **GDPR**: Data protection and privacy
- **HIPAA**: Healthcare data security (with proper configuration)
- **PCI DSS**: Payment card data security (with proper configuration)

## Updates to This Policy

This security policy is reviewed and updated quarterly. Major changes will be:

- Announced in security advisories
- Communicated via GitHub releases
- Posted on our security blog
- Shared with the security community

---

**Last Updated**: October 4, 2025  
**Version**: 1.0

For questions about this security policy, contact: <security@adaptiveintel.dev>
