# TreeCraft AI — Project Structure Analysis

## The Problem

- Code projects become messy and unorganized
- No clear structure standards
- Hard to navigate large codebases
- New team members get lost quickly

## The Solution

TreeCraft analyzes project structure and suggests improvements based on best practices for your specific tech stack.

## Features

- 🌳 Project structure visualization
- 📋 Best practice recommendations
- 🔄 Automated restructuring
- 📊 Structure health scoring
- 🎯 Framework-specific guidance

## How It Works

### Command Line Usage

```bash
# Analyze current project
treecraft analyze

# Get restructuring suggestions
treecraft suggest --framework react

# Auto-apply safe improvements
treecraft fix --auto-apply --backup

# Generate structure report
treecraft report --output project-analysis.html
```

### API Usage (Praximous Skill)

```bash
POST /api/v1/process
{
  "task_type": "treecraft_analyze",
  "project_path": "/path/to/project",
  "framework": "react"
}
```

## Analysis Capabilities

### Supported Project Types

- **Frontend Frameworks**
  - React, Vue.js, Angular
  - Next.js, Nuxt.js, SvelteKit
  - Vanilla JavaScript/TypeScript

- **Backend Frameworks**
  - Express.js, Fastify, Koa
  - Django, Flask, FastAPI
  - Spring Boot, .NET Core

- **Mobile Development**
  - React Native, Flutter
  - Ionic, Xamarin

- **Desktop Applications**
  - Electron, Tauri
  - WPF, JavaFX

- **Libraries & Tools**
  - npm packages, Python packages
  - Documentation sites
  - Monorepos

### Structure Scoring System

```text
Structure Health Score = (
  Organization × 0.25 +        # Logical folder grouping
  Naming Conventions × 0.20 +  # Consistent naming
  Depth Management × 0.15 +    # Appropriate nesting
  Framework Compliance × 0.20 + # Best practices
  Maintainability × 0.20       # Long-term sustainability
)
```

## Installation

```bash
# Install from npm
npm install -g @adaptive-intelligence/treecraft

# Install from PyPI
pip install treecraft-ai

# Install from source
git clone https://github.com/adaptive-intelligence/treecraft-ai.git
cd treecraft-ai
npm install
```

## Command Line Interface

### Basic Commands

```bash
# Analyze current directory
treecraft analyze

# Analyze specific path
treecraft analyze /path/to/project

# Quick health check
treecraft health

# Generate visual tree
treecraft tree --visual --output tree.svg
```

### Advanced Analysis

```bash
treecraft analyze --help

Usage: treecraft analyze [OPTIONS] [PATH]

Options:
  --framework TEXT        Specify framework for targeted analysis
  --config PATH          Custom configuration file
  --ignore PATTERN       Patterns to ignore (can use multiple times)
  --depth INTEGER        Maximum depth to analyze
  --output PATH          Output file for results
  --format FORMAT        Output format: json, yaml, html, markdown
  --include-hidden       Include hidden files and folders
  --git-aware           Use .gitignore patterns
  --verbose             Show detailed analysis
  --benchmark           Compare against similar projects
  --help                Show this message and exit
```

### Restructuring Commands

```bash
# Get suggestions only
treecraft suggest

# Apply specific improvements
treecraft fix --rules naming,organization

# Interactive mode
treecraft fix --interactive

# Preview changes
treecraft fix --dry-run --show-diff

# Safe auto-apply
treecraft fix --auto-apply --backup --exclude risky
```

## Example Output

### Structure Analysis

```bash
treecraft analyze --framework react
```

```json
{
  "project": {
    "name": "my-react-app",
    "type": "React Application",
    "framework_version": "18.2.0",
    "build_tool": "Vite",
    "package_manager": "npm"
  },
  "structure_score": 7.2,
  "analysis": {
    "organization": {
      "score": 8.5,
      "issues": [
        {
          "type": "mixing_concerns",
          "severity": "medium",
          "file": "src/utils/api.js",
          "message": "API utilities should be in src/services/",
          "suggestion": "Move to src/services/api.js"
        }
      ],
      "strengths": [
        "Clear separation of components and pages",
        "Dedicated hooks folder",
        "Consistent asset organization"
      ]
    },
    "naming_conventions": {
      "score": 6.8,
      "issues": [
        {
          "type": "inconsistent_casing",
          "severity": "low",
          "files": ["src/Components/Header.jsx", "src/components/footer.jsx"],
          "message": "Inconsistent component folder casing",
          "suggestion": "Use consistent PascalCase for component folders"
        }
      ]
    },
    "depth_management": {
      "score": 9.0,
      "max_depth": 4,
      "avg_depth": 2.3,
      "deep_paths": []
    },
    "framework_compliance": {
      "score": 7.5,
      "following_conventions": [
        "Component co-location",
        "Index file exports",
        "Asset proximity"
      ],
      "violations": [
        "Missing barrel exports in utils/",
        "CSS files not co-located with components"
      ]
    }
  },
  "recommendations": [
    {
      "priority": "high",
      "category": "organization",
      "title": "Consolidate API utilities",
      "description": "Move scattered API utilities to a dedicated services folder",
      "effort": "low",
      "impact": "medium",
      "files_affected": 3
    },
    {
      "priority": "medium",
      "category": "naming",
      "title": "Standardize component folder naming",
      "description": "Use consistent PascalCase for all component folders",
      "effort": "low",
      "impact": "low",
      "files_affected": 8
    }
  ],
  "structure_tree": {
    "src/": {
      "components/": ["Header/", "Footer/", "Navigation/"],
      "pages/": ["Home/", "About/", "Contact/"],
      "hooks/": ["useAuth.js", "useApi.js"],
      "utils/": ["helpers.js", "constants.js", "api.js"],
      "assets/": ["images/", "icons/", "styles/"]
    }
  }
}
```

### Visual Tree Output

```bash
treecraft tree --visual --framework react
```

```txt
my-react-app/
├── 📁 public/
│   ├── 🌐 index.html
│   └── 📄 manifest.json
├── 📁 src/
│   ├── 📁 components/ ✅
│   │   ├── 📁 Header/
│   │   │   ├── 🎨 Header.jsx
│   │   │   ├── 🎨 Header.module.css
│   │   │   └── 📄 index.js
│   │   └── 📁 Footer/
│   │       ├── 🎨 Footer.jsx
│   │       └── 📄 index.js
│   ├── 📁 pages/ ✅
│   │   ├── 📁 Home/
│   │   └── 📁 About/
│   ├── 📁 hooks/ ✅
│   │   ├── ⚙️ useAuth.js
│   │   └── ⚙️ useApi.js
│   ├── 📁 utils/ ⚠️
│   │   ├── 🔧 helpers.js
│   │   ├── 📋 constants.js
│   │   └── 🌐 api.js ← Should move to services/
│   └── 📁 assets/
│       ├── 📁 images/
│       └── 📁 styles/
├── 📄 package.json
├── ⚙️ vite.config.js
└── 📄 README.md

Legend:
✅ Well organized
⚠️ Needs attention
❌ Poor structure
🎨 React component
⚙️ Hook/utility
🌐 API/service
📋 Configuration
🔧 Helper utility
```

### Restructuring Suggestions

```bash
treecraft suggest --detailed
```

```yaml
restructuring_plan:
  priority_order:
    - organization_improvements
    - naming_standardization
    - depth_optimization
    - framework_alignment

  improvements:
    organization:
      - action: "create_directory"
        path: "src/services/"
        reason: "Centralize API and business logic"
        
      - action: "move_files"
        from: ["src/utils/api.js"]
        to: "src/services/api.js"
        reason: "API utilities belong in services"
        
      - action: "create_barrel_export"
        path: "src/services/index.js"
        exports: ["api.js"]
        
    naming:
      - action: "rename_directory"
        from: "src/Components/"
        to: "src/components/"
        reason: "Consistent lowercase for folder names"
        
      - action: "standardize_component_names"
        pattern: "PascalCase"
        affected_files: 12
        
    framework_specific:
      - action: "co_locate_styles"
        strategy: "move_css_to_component_folders"
        affected_files: 8
        
      - action: "add_barrel_exports"
        locations: ["src/components/", "src/hooks/", "src/utils/"]
        
  estimated_effort: "2-3 hours"
  breaking_changes: false
  backup_recommended: true
```

## Framework-Specific Analysis

### React Projects

```bash
treecraft analyze --framework react --react-version 18
```

**Checks for:**

- Component co-location
- Hook organization
- Context provider structure
- Testing file placement
- Style organization (CSS Modules, Styled Components)

**Recommendations:**

- Feature-based vs. file-type-based organization
- Barrel export patterns
- Component composition patterns

### Vue.js Projects

```bash
treecraft analyze --framework vue --vue-version 3
```

**Checks for:**

- Single File Component organization
- Composables structure
- Store organization (Pinia/Vuex)
- Plugin structure
- Assets and static files

### Node.js/Express Projects

```bash
treecraft analyze --framework express --type api
```

**Checks for:**

- MVC pattern compliance
- Route organization
- Middleware structure
- Database model organization
- Configuration management

### Python/Django Projects

```bash
treecraft analyze --framework django
```

**Checks for:**

- App structure compliance
- Model organization
- Template hierarchy
- Static files structure
- Settings configuration

## Configuration

### Project Configuration

```yaml
# .treecraft.yml
project:
  name: "My Application"
  type: "react-app"
  framework: "react"
  version: "18.2.0"

analysis:
  max_depth: 6
  ignore_patterns:
    - "node_modules/"
    - ".git/"
    - "dist/"
    - "build/"
  
  include_hidden: false
  git_aware: true

scoring:
  weights:
    organization: 0.25
    naming: 0.20
    depth: 0.15
    framework_compliance: 0.20
    maintainability: 0.20

  thresholds:
    excellent: 9.0
    good: 7.0
    needs_improvement: 5.0
    poor: 3.0

rules:
  naming_conventions:
    component_folders: "PascalCase"
    component_files: "PascalCase"
    utility_files: "camelCase"
    constant_files: "UPPER_CASE"
  
  organization:
    max_depth: 5
    min_files_for_folder: 3
    feature_based_preferred: true
  
  framework_specific:
    react:
      co_locate_styles: true
      barrel_exports: true
      index_files: true
      hook_folder: "hooks"
      component_folder: "components"

auto_fix:
  enabled: true
  backup: true
  safe_only: true
  exclude_rules: ["major_restructure"]
```

### Global Configuration

```yaml
# ~/.treecraft/config.yml
defaults:
  output_format: "json"
  include_recommendations: true
  show_tree: false
  auto_detect_framework: true

frameworks:
  react:
    preferred_structure: "feature-based"
    style_co_location: true
  
  vue:
    preferred_structure: "file-type-based"
    composables_folder: "composables"

reporting:
  include_visual_tree: true
  show_file_counts: true
  highlight_issues: true
  performance_metrics: true

integrations:
  git:
    respect_gitignore: true
    include_git_info: true
  
  ide:
    generate_workspace_settings: true
    create_folder_templates: true
```

## Integration Points

- → **MarkFlow**: Structure analysis for documentation projects
- → **Test Generator**: Organize test files following project structure
- → **EmbedID**: Track structural changes for compliance
- → **CI/CD**: Automated structure validation

## Use Cases

### New Project Setup

```bash
# Generate recommended structure
treecraft init --framework react --template modern

# Create folder structure
treecraft scaffold --config project-template.yml
```

### Code Review Integration

```bash
# Check structure changes in PR
treecraft diff --base main --head feature-branch

# Validate structure before merge
treecraft validate --rules strict --fail-on-error
```

### Team Onboarding

```bash
# Generate project guide
treecraft guide --output project-structure-guide.md

# Create navigation help
treecraft map --interactive --bookmark-important
```

### Refactoring Support

```bash
# Plan major refactoring
treecraft plan --target-structure feature-based

# Track refactoring progress
treecraft track --milestone "api-restructure"
```

## API Reference

### Python SDK

```python
from treecraft import ProjectAnalyzer

# Initialize analyzer
analyzer = ProjectAnalyzer(framework='react')

# Analyze project
result = analyzer.analyze('/path/to/project')
print(f"Structure Score: {result.score}")

# Get suggestions
suggestions = analyzer.get_suggestions(result)
for suggestion in suggestions:
    print(f"- {suggestion.title}: {suggestion.description}")

# Apply improvements
analyzer.apply_fixes(suggestions, dry_run=False)
```

### Node.js SDK

```javascript
const { TreeCraft } = require('@adaptive-intelligence/treecraft');

const analyzer = new TreeCraft({
  framework: 'react',
  config: '.treecraft.yml'
});

async function analyzeProject() {
  const result = await analyzer.analyze('./src');
  console.log(`Score: ${result.structureScore}`);
  
  if (result.recommendations.length > 0) {
    console.log('Recommendations:');
    result.recommendations.forEach(rec => {
      console.log(`- ${rec.title} (${rec.priority})`);
    });
  }
}
```

## Troubleshooting

### Common Issues

**Q: Analysis takes too long**
A: Use `--depth` to limit analysis depth, add more ignore patterns

**Q: Framework not detected correctly**
A: Specify explicitly with `--framework` or update detection patterns

**Q: Too many false positives**
A: Adjust scoring weights in configuration for your project type

**Q: Suggestions seem irrelevant**
A: Check that the correct framework and project type are detected

### Performance Tips

```bash
# Faster analysis
treecraft analyze --quick --essential-only

# Focus on specific areas
treecraft analyze --rules organization,naming

# Use cached results
treecraft analyze --cache --ttl 3600
```

### Getting Help

- GitHub Issues: [Report bugs](https://github.com/adaptive-intelligence/treecraft-ai/issues)
- Discord: [Join community](https://discord.gg/adaptive-intel)
- Documentation: [Full docs](https://docs.adaptiveintel.dev/treecraft)
