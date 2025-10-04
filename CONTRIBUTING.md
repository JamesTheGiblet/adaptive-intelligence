# Contributing to Adaptive Intelligence Platform

Thank you for your interest in contributing to the Adaptive Intelligence Platform! This document provides guidelines and information for contributors.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Setup](#development-setup)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Pull Request Process](#pull-request-process)
- [Issue Reporting](#issue-reporting)
- [Community](#community)

## Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## Getting Started

### Prerequisites

- Python 3.8+ or Node.js 16+
- Docker & Docker Compose
- Git
- Basic understanding of AI/ML concepts

### First-time Contributors

1. **Explore the codebase**: Start by reading the [README](README.md) and [Architecture Guide](docs/architecture.md)
2. **Set up development environment**: Follow the [Development Setup](#development-setup) guide
3. **Look for beginner-friendly issues**: Search for issues labeled `good first issue` or `help wanted`
4. **Join our community**: Connect with us on [Discord](https://discord.gg/adaptive-intel)

## How to Contribute

### Ways to Contribute

- **Code**: Bug fixes, feature implementations, performance improvements
- **Documentation**: Tutorials, API docs, examples, translations
- **Testing**: Writing tests, reporting bugs, testing new features
- **Design**: UI/UX improvements, logos, graphics
- **Community**: Helping others, writing blog posts, giving talks

### Contribution Areas

#### Core Platform

- **AAS (Agent Autonomy System)**: Agent orchestration and management
- **Praximous**: AI gateway and skill routing
- **Infrastructure**: Docker, Kubernetes, monitoring

#### Intelligence Tools

- **Whisper**: Secret scanning and vulnerability detection
- **Aegis**: Security compliance framework
- **Test Generator**: Automated test creation
- **EmbedID**: Code watermarking and provenance
- **CertiScope**: Web credibility scoring
- **TreeCraft**: Project structure analysis
- **MarkFlow**: Intelligent markdown editing
- **Chameleon**: Domain expertise adaptation

#### Skills (Pantheon)

- **New Skills**: Add AI capabilities for specific domains
- **Skill Improvements**: Enhance existing skill performance
- **Skill Documentation**: Examples and usage guides

## Development Setup

### 1. Fork and Clone

```bash
# Fork the repository on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/adaptive-intelligence.git
cd adaptive-intelligence

# Add upstream remote
git remote add upstream https://github.com/adaptive-intelligence/platform.git
```

### 2. Environment Setup

```bash
# Copy environment template
cp .env.example .env

# Edit .env with your API keys
nano .env

# Required for development:
# GEMINI_API_KEY=your_key_here
# OPENAI_API_KEY=your_key_here (optional)
```

### 3. Development Installation

#### Option A: Docker Development (Recommended)

```bash
# Start development environment
docker-compose -f docker-compose.dev.yml up -d

# Install development dependencies
docker-compose exec praximous pip install -r requirements-dev.txt
docker-compose exec aas pip install -r requirements-dev.txt
```

#### Option B: Local Development

```bash
# Python components
cd praximous
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install -r requirements-dev.txt
pip install -e .

# Node.js components (for tools with web interfaces)
cd ../tools/markflow
npm install
npm run dev
```

### 4. Verify Installation

```bash
# Test Praximous
curl http://localhost:8000/api/v1/health

# Test AAS
curl http://localhost:9000/api/v1/health

# Run test suite
docker-compose exec praximous pytest
docker-compose exec aas pytest
```

## Coding Standards

### Python Code

- **Style**: Follow [PEP 8](https://pep8.org/) with [Black](https://black.readthedocs.io/) formatting
- **Type Hints**: Use type hints for all function signatures
- **Docstrings**: Use Google-style docstrings
- **Imports**: Use absolute imports, group by standard/third-party/local

```python
# Example
from typing import Dict, List, Optional

def process_skill_request(
    skill_name: str,
    parameters: Dict[str, Any],
    context: Optional[Dict[str, Any]] = None
) -> SkillResponse:
    """Process a skill execution request.
    
    Args:
        skill_name: Name of the skill to execute
        parameters: Skill-specific parameters
        context: Optional execution context
        
    Returns:
        SkillResponse containing execution results
        
    Raises:
        SkillNotFoundError: If skill is not available
        ValidationError: If parameters are invalid
    """
    # Implementation here
    pass
```

### JavaScript/TypeScript Code

- **Style**: Use [Prettier](https://prettier.io/) with [ESLint](https://eslint.org/)
- **Type Safety**: Prefer TypeScript for new code
- **Components**: Use functional components with hooks
- **Testing**: Jest for unit tests, Cypress for E2E

```typescript
// Example
interface SkillConfig {
  name: string;
  version: string;
  parameters: Record<string, unknown>;
}

export const executeSkill = async (
  config: SkillConfig
): Promise<SkillResult> => {
  // Implementation here
};
```

### Code Quality Tools

```bash
# Python
black .                    # Format code
isort .                    # Sort imports
flake8 .                   # Lint code
mypy .                     # Type checking
pytest                     # Run tests

# JavaScript/TypeScript
npm run format             # Prettier formatting
npm run lint               # ESLint
npm run type-check         # TypeScript checking
npm test                   # Jest tests
```

## Testing Guidelines

### Test Structure

```txt
tests/
├── unit/                  # Unit tests
│   ├── test_skills.py
│   └── test_agents.py
├── integration/           # Integration tests
│   ├── test_api.py
│   └── test_workflows.py
├── e2e/                   # End-to-end tests
│   └── test_user_flows.py
└── fixtures/              # Test data
    └── sample_data.json
```

### Writing Tests

- **Unit Tests**: Test individual functions/classes in isolation
- **Integration Tests**: Test component interactions
- **E2E Tests**: Test complete user workflows
- **Coverage**: Aim for >90% test coverage on new code

```python
# Example unit test
import pytest
from unittest.mock import Mock, patch

from praximous.skills import SkillExecutor
from praximous.exceptions import SkillNotFoundError

class TestSkillExecutor:
    def test_execute_existing_skill(self):
        """Test successful skill execution."""
        executor = SkillExecutor()
        result = executor.execute("test_skill", {"param": "value"})
        
        assert result.success is True
        assert "output" in result.data
    
    def test_execute_nonexistent_skill(self):
        """Test error handling for missing skills."""
        executor = SkillExecutor()
        
        with pytest.raises(SkillNotFoundError):
            executor.execute("nonexistent_skill", {})
```

### Test Commands

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/unit/test_skills.py

# Run with coverage
pytest --cov=praximous --cov-report=html

# Run E2E tests
pytest tests/e2e/ --browser=chrome
```

## Documentation

### Documentation Types

- **API Documentation**: Auto-generated from docstrings
- **User Guides**: Step-by-step tutorials
- **Developer Docs**: Architecture and implementation details
- **Examples**: Code samples and use cases

### Writing Documentation

- Use clear, concise language
- Include code examples
- Test all examples
- Update docs with code changes

### Documentation Structure

```txt
docs/
├── README.md              # Main documentation index
├── architecture.md        # Platform architecture
├── workflows.md           # Development workflows
├── guides/                # User guides
│   ├── deployment.md
│   └── integrations.md
└── tools/                 # Tool documentation
    ├── whisper.md
    └── aegis.md
```

### Building Documentation

```bash
# Install documentation dependencies
pip install -r docs/requirements.txt

# Build documentation
cd docs
mkdocs serve  # Development server
mkdocs build  # Production build
```

## Pull Request Process

### Before Submitting

1. **Update your fork**:

   ```bash
   git fetch upstream
   git checkout main
   git merge upstream/main
   ```

2. **Create feature branch**:

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make changes and test**:

   ```bash
   # Make your changes
   # Run tests
   pytest
   # Run linting
   black . && flake8 .
   ```

4. **Commit changes**:

   ```bash
   git add .
   git commit -m "feat: add new skill for data analysis"
   ```

### Commit Message Format

Use [Conventional Commits](https://www.conventionalcommits.org/):

```txt
type(scope): description

[optional body]

[optional footer]
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Examples**:

- `feat(skills): add natural language processing skill`
- `fix(api): resolve authentication token validation`
- `docs(readme): update installation instructions`
- `test(skills): add unit tests for skill executor`

### Submitting Pull Request

1. **Push to your fork**:

   ```bash
   git push origin feature/your-feature-name
   ```

2. **Create Pull Request** on GitHub with:
   - Clear title and description
   - Link to related issues
   - Screenshots/demos if applicable
   - Checklist completion

### Pull Request Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Code refactoring

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] Tests added/updated
- [ ] No breaking changes (or clearly documented)
```

### Review Process

1. **Automated Checks**: CI/CD pipeline runs tests and linting
2. **Code Review**: Maintainers review code quality and design
3. **Testing**: Changes tested in development environment
4. **Approval**: At least one maintainer approval required
5. **Merge**: Squash and merge to main branch

## Issue Reporting

### Bug Reports

Use the bug report template:

```markdown
**Bug Description**
Clear description of the bug

**Steps to Reproduce**
1. Step one
2. Step two
3. Step three

**Expected Behavior**
What should happen

**Actual Behavior**
What actually happens

**Environment**
- OS: [e.g., Ubuntu 20.04]
- Python version: [e.g., 3.9.7]
- Platform version: [e.g., 1.2.3]

**Additional Context**
Screenshots, logs, etc.
```

### Feature Requests

Use the feature request template:

```markdown
**Feature Description**
Clear description of the proposed feature

**Problem Statement**
What problem does this solve?

**Proposed Solution**
How should it work?

**Alternatives Considered**
Other approaches considered

**Additional Context**
Mockups, examples, etc.
```

### Security Issues

For security vulnerabilities, please follow our [Security Policy](SECURITY.md) and report privately to <security@adaptiveintel.dev>.

## Community

### Communication Channels

- **GitHub Discussions**: General questions and discussions
- **Discord**: Real-time chat and community support
- **GitHub Issues**: Bug reports and feature requests
- **Email**: <security@adaptiveintel.dev> (security issues only)

### Community Guidelines

- Be respectful and inclusive
- Help others learn and grow
- Share knowledge and experiences
- Follow our Code of Conduct
- Give constructive feedback

### Recognition

We recognize contributors through:

- **Contributors file**: Listed in CONTRIBUTORS.md
- **Release notes**: Mentioned in changelog
- **Social media**: Featured on our channels
- **Swag**: Stickers and merchandise for regular contributors

## Getting Help

### Documentation table of contents

- [README](README.md) - Getting started
- [Architecture Guide](docs/architecture.md) - Platform overview
- [API Documentation](https://docs.adaptiveintel.dev) - API reference

### Support

- **Community**: GitHub Discussions, Discord
- **Documentation**: Built-in help and examples
- **Maintainers**: Available for guidance on complex contributions

### Mentorship

New contributors can request mentorship:

1. Comment on an issue asking for guidance
2. Join our Discord and introduce yourself
3. Attend our weekly community calls

Thank you for contributing to the Adaptive Intelligence Platform! 🚀
