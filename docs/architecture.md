# Platform Architecture

The Adaptive Intelligence Platform has three layers that work together:

```text
┌─────────────────────────────────────────────────────────────┐
│                    YOUR APPLICATIONS                         │
│    (CLI tools, Web UIs, API integrations, CI/CD)            │
└─────────────────────────────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         ⚙️ LAYER 1: AAS (Agent Orchestration)               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ • Task Queue (Redis/RabbitMQ)                         │  │
│  │ • Agent Fleet (Podman containers)                     │  │
│  │ • Skill Library (Pantheon - versioned Python scripts)│  │
│  │ • Human Fallback (Copilot Bridge)                     │  │
│  │ • Resource Control (CPU/Memory limits per agent)     │  │
│  │ • Monitoring (Prometheus/Grafana)                     │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────┐
│           🧠 LAYER 2: Praximous (AI Gateway)                │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ • LLM Router (Gemini → Ollama failover)              │  │
│  │ • Smart Skills Executor                               │  │
│  │ • Analytics & Audit Logs (SQLite)                    │  │
│  │ • Persona Management (Context-aware responses)       │  │
│  │ • License Verification (Tiered features)             │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────┐
│        🧰 LAYER 3: Adaptive Intelligence Tools              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ Security & Compliance                                 │  │
│  │  • Whisper (Secret Scanner)                          │  │
│  │  • Aegis CLI (Security Documentation)                │  │
│  │                                                        │  │
│  │ Code Quality & Testing                                │  │
│  │  • Test Code Generator (Automated Test Creation)     │  │
│  │  • EmbedID (Code Watermarking & Provenance)         │  │
│  │                                                        │  │
│  │ Verification & Analysis                               │  │
│  │  • CertiScope AI (Web Credibility Scoring)          │  │
│  │  • TreeCraft AI (Project Structure Analysis)        │  │
│  │                                                        │  │
│  │ Content & Documentation                               │  │
│  │  • MarkFlow (Markdown Editor & Previewer)           │  │
│  │                                                        │  │
│  │ Intelligence Core                                     │  │
│  │  • Chameleon LM (Domain Expertise Adapter)          │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## How the Layers Work Together

- **Layer 1 (AAS)** manages task distribution and agent lifecycle
- **Layer 2 (Praximous)** routes AI requests and executes Smart Skills
- **Layer 3 (Tools)** provides specialized capabilities as modular skills

## Layer 1: AAS (Autonomous Agent System)

Enterprise-grade agent orchestration platform with human fallback, resource limits, and self-improvement.

### Key Features of Praximous

- 🤖 Autonomous task execution
- 📚 Skill Library (Pantheon) — Git-based skill versioning
- 👨‍💻 Human Fallback (Copilot Bridge) — Escalate when stuck
- 🎓 Self-Improvement — Learn from human feedback
- 🔒 Resource Control — CPU/memory limits per agent
- 📊 Monitoring — Prometheus/Grafana integration
- 🔐 Security — RBAC, audit logs, sandboxed execution

### How Praximous Works

1. **Task Submission:**

```bash
POST /api/v1/tasks
{
  "type": "security_audit",
  "target": "/path/to/project",
  "priority": "high"
}
```

1. **Agent Processing:**
Task Queue → Agent picks up task → Checks Pantheon for skill
→ Downloads skill if needed → Executes in sandbox
→ Logs result → Updates memory

1. **Human Fallback (When Agent Fails):**
Agent fails → Task flagged for human → Human reviews via UI
→ Human provides solution → Agent learns from feedback
→ Next time, agent handles it autonomously

1. **Resource Control:**

```yaml
# podman-compose.yml
services:
  agent_1:
    resources:
      limits:
        cpus: '0.5'      # Max 50% of 1 CPU
        memory: '512M'    # Max 512MB RAM
    read_only: true       # Filesystem read-only
    security_opt:
      - no-new-privileges:true
```

### Skill Library (Pantheon) Structure

```txt
pantheon/
├── core/
│   ├── praximous_gateway.py      # Praximous as AAS skill
│   ├── whisper_scan.py           # Secret detection
│   ├── aegis_initialize.py       # Security docs
│   └── certiscope_verify.py      # Web verification
├── automation/
│   ├── treecraft_analyze.py      # Structure analysis
│   ├── generate_tests.py         # Test generation
│   └── markflow_generate.py      # Content creation
├── intelligence/
│   ├── chameleon_transform.py    # Domain adaptation
│   └── embedid_sign.py           # Code watermarking
└── custom/
    └── [your custom skills]
```

## Layer 2: Praximous (AI Gateway)

On-premise AI gateway with automatic failover, Smart Skills execution, and complete audit logging.

### Features

- 🔄 Dynamic failover (Gemini → Ollama)
- 🧠 Smart Skills Platform (modular capabilities)
- 🏠 On-premise deployment (Docker)
- 🔌 Pluggable providers (Gemini, Ollama, OpenAI, etc.)
- 📊 Analytics & audit logging (SQLite)
- 🎭 Context-aware persona system
- 📜 Tiered licensing (Community, Pro, Enterprise)

### How It Works

```bash
# Initialize Praximous
docker-compose run --rm praximous python main.py --init

# Start gateway
docker-compose up -d

# Process request
POST http://localhost:8000/api/v1/process
{
  "task_type": "default_llm_tasks",
  "prompt": "Explain containerization in 3 sentences"
}

# Execute Smart Skill
POST http://localhost:8000/api/v1/process
{
  "task_type": "whisper_scan",
  "prompt": "Scan this codebase",
  "target_path": "/path/to/project"
}

# View analytics
GET http://localhost:8000/api/v1/analytics
```

### Smart Skills

All Layer 3 tools are available as Smart Skills:

- `whisper_scan` — Secret detection
- `aegis_initialize` — Security doc generation
- `generate_tests` — Test creation
- `embedid_sign` — Code watermarking
- `certiscope_verify` — Web verification
- `treecraft_analyze` — Structure analysis
- `markflow_generate` — Content creation
- `chameleon_transform` — Domain adaptation

## Layer 3: Adaptive Intelligence Tools

Specialized tools that solve specific development problems. Each tool can be used standalone or as part of the integrated platform.

### Categories

1. **Security & Compliance**
   - [Whisper](tools/whisper.md) — Secret & credential scanning
   - [Aegis CLI](tools/aegis.md) — Security documentation generation

2. **Code Quality & Testing**
   - [Test Code Generator](tools/test-generator.md) — Automated test creation
   - [EmbedID](tools/embedid.md) — Code watermarking & provenance

3. **Verification & Analysis**
   - [CertiScope AI](tools/certiscope.md) — Web credibility scoring
   - [TreeCraft AI](tools/treecraft.md) — Project structure analysis

4. **Content & Documentation**
   - [MarkFlow](tools/markflow.md) — Markdown editor & previewer

5. **Intelligence Core**
   - [Chameleon LM](tools/chameleon.md) — Domain expertise adapter

## Integration Points

- **AAS**: Praximous runs as an AAS agent
- **All Tools**: Unified API endpoint for all capabilities
- **CI/CD**: Webhook integrations
- **Internal Apps**: Single authentication point

For detailed deployment instructions, see the [Deployment Guide](guides/deployment.md).
For workflow examples, see [Workflows](workflows.md).
