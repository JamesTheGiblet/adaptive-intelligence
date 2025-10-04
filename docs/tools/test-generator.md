# Test Code Generator — Automated Test Creation

## The Problem

- Writing tests is boring and repetitive
- Test coverage is low
- Tests are inconsistent
- Developers skip testing due to time pressure

## The Solution

Upload your code, get comprehensive test suites instantly. Supports multiple languages and testing frameworks.

## Features

- 🌐 Multi-language support (JavaScript, Python, Java, PHP)
- 🧰 Framework integration (Jest, Mocha, Pytest, JUnit, PHPUnit)
- 🎯 Multiple test types (unit, integration, performance, security)
- 📦 Export options (clipboard, file download)
- 🖱️ Drag & drop interface

## How It Works

### Web UI (Drag and Drop)

```bash
# Open the web interface
open test-generator/index.html

# Upload your code files or paste code directly
# Select language and framework
# Click "Generate Tests"
# Download or copy the generated test files
```

### As AAS Skill

```bash
POST /api/v1/process
{
  "task_type": "generate_tests",
  "code": "function add(a, b) { return a + b; }",
  "language": "javascript",
  "framework": "jest",
  "test_types": ["unit", "integration"]
}
```

## Example Usage

### Input Code (JavaScript)

```javascript
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

function divide(a, b) {
  if (b === 0) {
    throw new Error("Division by zero");
  }
  return a / b;
}
```

### Generated Tests (Jest)

```javascript
describe('Math Functions', () => {
  describe('add', () => {
    test('should add two positive numbers', () => {
      expect(add(2, 3)).toBe(5);
    });
    
    test('should handle negative numbers', () => {
      expect(add(-1, -1)).toBe(-2);
    });
    
    test('should handle zero', () => {
      expect(add(0, 5)).toBe(5);
    });
    
    test('should handle floating point numbers', () => {
      expect(add(0.1, 0.2)).toBeCloseTo(0.3);
    });
  });

  describe('multiply', () => {
    test('should multiply two positive numbers', () => {
      expect(multiply(3, 4)).toBe(12);
    });
    
    test('should handle multiplication by zero', () => {
      expect(multiply(5, 0)).toBe(0);
    });
    
    test('should handle negative numbers', () => {
      expect(multiply(-2, 3)).toBe(-6);
    });
  });

  describe('divide', () => {
    test('should divide two numbers', () => {
      expect(divide(10, 2)).toBe(5);
    });
    
    test('should throw error when dividing by zero', () => {
      expect(() => divide(10, 0)).toThrow('Division by zero');
    });
    
    test('should handle decimal results', () => {
      expect(divide(7, 3)).toBeCloseTo(2.333);
    });
  });
});
```

## Supported Languages & Frameworks

### JavaScript/TypeScript

- **Jest** - React, Node.js testing
- **Mocha** - Flexible testing framework
- **Vitest** - Vite-native testing
- **Cypress** - E2E testing

### Python

- **Pytest** - Modern Python testing
- **Unittest** - Built-in Python testing
- **Nose2** - Extended unittest

### Java

- **JUnit 5** - Modern Java testing
- **TestNG** - Advanced testing framework
- **Mockito** - Mocking framework

### PHP

- **PHPUnit** - Standard PHP testing
- **Pest** - Modern PHP testing

### Go

- **Testing** - Built-in Go testing
- **Testify** - Extended assertions

### C/C++

- **NUnit** - .NET testing framework
- **xUnit** - Modern .NET testing
- **MSTest** - Microsoft testing framework

## Test Types

### Unit Tests

- Function-level testing
- Edge case coverage
- Error handling
- Input validation

### Integration Tests

- Component interaction
- API endpoint testing
- Database operations
- Service integration

### Performance Tests

- Load testing
- Stress testing
- Memory usage
- Execution time

### Security Tests

- Input sanitization
- Authentication checks
- Authorization validation
- Injection prevention

## Configuration

Create a `.testgen.yml` file in your project:

```yaml
# Language settings
language: "javascript"
framework: "jest"

# Test types to generate
test_types:
  - "unit"
  - "integration"
  - "security"

# Coverage settings
coverage:
  target: 80
  include_edge_cases: true
  include_error_handling: true

# Output settings
output:
  directory: "./tests"
  file_naming: "kebab-case"
  include_setup: true
  include_teardown: true

# Framework-specific settings
jest:
  setup_files: ["./jest.setup.js"]
  test_environment: "node"
  collect_coverage: true

pytest:
  fixtures: true
  parametrize: true
  markers: ["unit", "integration", "slow"]
```

## Command Line Interface

```bash
# Install the CLI tool
npm install -g @adaptive-intelligence/test-generator

# Generate tests for a file
testgen generate src/math.js --framework jest --output tests/

# Generate tests for entire directory
testgen generate src/ --recursive --framework jest

# Specify test types
testgen generate src/api.js --types unit,integration,security

# Check coverage requirements
testgen coverage src/ --min 80

# Validate existing tests
testgen validate tests/ --against src/
```

## API Reference

### Generate Tests Endpoint

```bash
POST /api/v1/process
{
  "task_type": "generate_tests",
  "code": "string",           // Source code to test
  "language": "string",       // Programming language
  "framework": "string",      // Testing framework
  "test_types": ["string"],   // Array of test types
  "options": {
    "coverage_target": 80,
    "include_setup": true,
    "include_mocks": true,
    "async_support": true
  }
}
```

### Response Format

```json
{
  "status": "success",
  "tests_generated": 12,
  "coverage_estimate": 85,
  "generation_time": "3.2s",
  "results": {
    "test_file": "math.test.js",
    "test_code": "...",
    "setup_code": "...",
    "dependencies": ["jest", "@types/jest"],
    "test_structure": {
      "describe_blocks": 3,
      "test_cases": 12,
      "edge_cases": 5,
      "error_cases": 3
    }
  }
}
```

## Integration Points

- → **AAS**: Batch test generation for entire projects
- → **Whisper**: Scan generated tests for secrets
- → **TreeCraft**: Generate tests based on project structure
- → **EmbedID**: Watermark generated test files
- → **CI/CD**: Automated test generation pipeline

## Best Practices

1. **Review Generated Tests**: Always review and customize generated tests
2. **Add Custom Logic**: Extend tests with domain-specific logic
3. **Maintain Tests**: Update tests when code changes
4. **Use Type Coverage**: Ensure all code paths are tested
5. **Mock External Dependencies**: Use mocks for external services

## Examples by Framework

### React Component Testing (Jest + React Testing Library)

```javascript
// Generated test for React component
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button Component', () => {
  test('renders button with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button')).toHaveTextContent('Click me');
  });

  test('calls onClick handler when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  test('applies disabled state correctly', () => {
    render(<Button disabled>Click me</Button>);
    expect(screen.getByRole('button')).toBeDisabled();
  });
});
```

### Python API Testing (Pytest + FastAPI)

```python
# Generated test for FastAPI endpoint
import pytest
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

class TestUserAPI:
    def test_create_user_success(self):
        user_data = {
            "name": "John Doe",
            "email": "john@example.com"
        }
        response = client.post("/users", json=user_data)
        assert response.status_code == 201
        assert response.json()["name"] == "John Doe"

    def test_create_user_invalid_email(self):
        user_data = {
            "name": "John Doe",
            "email": "invalid-email"
        }
        response = client.post("/users", json=user_data)
        assert response.status_code == 422

    def test_get_user_not_found(self):
        response = client.get("/users/999")
        assert response.status_code == 404

    @pytest.mark.parametrize("name,email,expected_status", [
        ("", "test@example.com", 422),
        ("John", "", 422),
        ("John", "valid@example.com", 201),
    ])
    def test_create_user_validation(self, name, email, expected_status):
        user_data = {"name": name, "email": email}
        response = client.post("/users", json=user_data)
        assert response.status_code == expected_status
```

## Troubleshooting

### Common Issues

**Q: Generated tests are too basic**
A: Use the "include_edge_cases" option and review/extend the generated tests

**Q: Tests don't match my coding style**
A: Customize the configuration file to match your team's conventions

**Q: Framework not supported**
A: Check the supported frameworks list or request support via GitHub issues

**Q: Generated tests fail to run**
A: Ensure all dependencies are installed and framework configuration is correct

### Getting Help

- GitHub Issues: [Report bugs](https://github.com/adaptive-intelligence/test-generator/issues)
- Discord: [Join community](https://discord.gg/adaptive-intel)
- Documentation: [Full docs](https://docs.adaptiveintel.dev/test-generator)
