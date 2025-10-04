# Deployment Guide

This guide covers installation, configuration, and deployment of the Adaptive Intelligence Platform.

## Prerequisites

### Required

- Docker & Docker Compose (or Podman)
- Linux/macOS (Windows via WSL2)
- 8 GB RAM minimum
- 20 GB disk space

### Recommended

- 16 GB RAM
- 50 GB disk space
- GPU (for local LLM performance)

## Installation Options

### Option 1: Full Platform (Recommended for Enterprise)

**Includes**: AAS + Praximous + All Tools

```bash
# Clone the platform
git clone https://github.com/adaptive-intelligence/platform.git
cd platform

# Configure environment
cp .env.example .env
nano .env  # Add your API keys (Gemini, etc.)

# Initialize AAS
docker-compose run --rm aas python main.py --init

# Initialize Praximous
docker-compose run --rm praximous python main.py --init

# Start everything
docker-compose up -d

# Verify
curl http://localhost:8000/api/v1/system-status
```

**Services Running:**

- AAS Agent Fleet: <http://localhost:9000>
- Praximous Gateway: <http://localhost:8000>
- Monitoring Dashboard: <http://localhost:3000> (Grafana)
- Skill Library: <http://localhost:9001/pantheon>

### Option 2: Praximous Only (For Teams)

**Includes**: Praximous + All Tools (without AAS orchestration)

```bash
# Clone Praximous
git clone https://github.com/adaptive-intelligence/praximous.git
cd praximous

# Configure
cp .env.example .env
nano .env  # Add API keys

# Initialize
docker-compose run --rm praximous python main.py --init

# Start
docker-compose up -d

# Test
curl http://localhost:8000/api/v1/skills
```

**Available at:**

- API Gateway: <http://localhost:8000>
- Web UI: <http://localhost:8000/>
- Docs: <http://localhost:8000/docs>

### Option 3: Individual Tools (For Developers)

Install specific tools as needed:

```bash
# Whisper (Secret Scanner)
pip install adaptive-whisper
whisper --scan .

# Aegis CLI (Security Docs)
pip install praximous-aegis-cli
aegis /path/to/project

# EmbedID (Code Watermarking)
pip install embedid
embedid add file.py --author "Your Name"

# TreeCraft AI (Structure Analysis)
# Download from GitHub releases
# Open index.html in browser

# Test Code Generator
# Download from GitHub releases
# Open index.html in browser

# MarkFlow (Markdown Editor)
# Download from GitHub releases
# Open index.html in browser

# CertiScope AI
git clone https://github.com/adaptive-intelligence/certiscope-ai.git
cd certiscope-ai
pip install -r requirements.txt
python certiscope.py --url https://example.com

# Chameleon LM
git clone https://github.com/adaptive-intelligence/chameleon-lm.git
cd chameleon-lm
pip install -r requirements.txt
python chameleon.py --domain "legal_expert"
```

## 5-Minute Quick Start (Try Before Deploy)

Test the platform without installation:

```bash
# Using Docker (temporary container)
docker run -it --rm \
  -e GEMINI_API_KEY="your_key_here" \
  adaptiveintel/praximous:latest \
  python main.py --demo

# This starts a demo environment with:
# - Praximous gateway
# - Sample skills loaded
# - Mock data for testing
# - Expires after 2 hours
```

Try it out:

```bash
# Scan for secrets (mock data)
curl -X POST http://localhost:8000/api/v1/process \
  -H "Content-Type: application/json" \
  -d '{"task_type": "whisper_scan", "prompt": "Scan demo project"}'

# Generate tests (mock code)
curl -X POST http://localhost:8000/api/v1/process \
  -H "Content-Type: application/json" \
  -d '{"task_type": "generate_tests", "code": "def add(a,b): return a+b"}'

# Verify web source
curl -X POST http://localhost:8000/api/v1/process \
  -H "Content-Type: application/json" \
  -d '{"task_type": "certiscope_verify", "url": "https://github.com"}'
```

## Configuration

### Environment Variables

```bash
# .env file

# === PROVIDER API KEYS ===
GEMINI_API_KEY="your_gemini_key"           # Primary LLM (required)
OLLAMA_API_URL="http://ollama:11434"       # Fallback LLM (optional)
OPENAI_API_KEY="your_openai_key"           # Alternative provider (optional)

# === PRAXIMOUS CONFIG ===
PRAXIMOUS_LICENSE_KEY='{"payload":"...","signature":"..."}'  # Pro/Enterprise
PRAXIMOUS_API_KEYS="prx-abc123..."         # API authentication
LOG_LEVEL="INFO"                           # DEBUG, INFO, WARNING, ERROR

# === TOOL-SPECIFIC KEYS ===
SEARCH_API_KEY="serper_key"                # For web search (CertiScope)
WEATHER_API_KEY="openweather_key"          # For weather skill (example)

# === EMAIL CONFIG (for notifications) ===
SMTP_HOST="smtp.gmail.com"
SMTP_PORT="587"
SMTP_USER="your-email@example.com"
SMTP_PASSWORD="your-app-password"

# === AAS CONFIG ===
AAS_TASK_QUEUE="redis://redis:6379"        # Task queue backend
AAS_AGENT_LIMIT="10"                       # Max concurrent agents
AAS_RESOURCE_CPU="0.5"                     # CPU per agent
AAS_RESOURCE_MEM="512M"                    # Memory per agent

# === MONITORING ===
PROMETHEUS_ENABLED="true"
GRAFANA_ENABLED="true"
```

### Identity Configuration

**Praximous Identity** (`config/identity.yaml`):

```yaml
system_name: "MyCompany-AI-Gateway"
display_name: "MyCompany AI Platform"
persona:
  role: "Enterprise AI Assistant"
  industry: "Software Development"
  tone: "Professional and helpful"
  expertise:
    - "Software engineering"
    - "Security best practices"
    - "DevOps automation"
created_at: "2025-10-03T14:30:00Z"
version: "1.0.0"
```

**AAS Identity** (`config/aas-identity.yaml`):

```yaml
deployment_name: "MyCompany-AAS-Production"
organization: "MyCompany Inc."
environment: "production"
agent_fleet_size: 10
skill_library_url: "https://github.com/mycompany/pantheon.git"
copilot_bridge:
  enabled: true
  escalation_threshold: "high"
  notification_channels:
    - email: "devops@mycompany.com"
    - slack: "#ai-alerts"
security:
  rbac_enabled: true
  audit_retention_days: 90
  require_2fa_for_fallback: true
```

## Docker Compose Full Stack

```yaml
# docker-compose.yml
version: '3.8'

services:
  # Layer 1: AAS
  aas:
    image: adaptiveintel/aas:latest
    ports:
      - "9000:9000"
    environment:
      - AAS_TASK_QUEUE=redis://redis:6379
      - AAS_AGENT_LIMIT=10
    volumes:
      - ./pantheon:/app/pantheon
      - ./config:/app/config
    depends_on:
      - redis
      - prometheus
  
  # AAS Agents
  agent_1:
    image: adaptiveintel/aas-agent:latest
    environment:
      - AAS_URL=http://aas:9000
      - AGENT_ID=agent_1
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
  
  agent_2:
    image: adaptiveintel/aas-agent:latest
    environment:
      - AAS_URL=http://aas:9000
      - AGENT_ID=agent_2
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
  
  # Layer 2: Praximous
  praximous:
    image: adaptiveintel/praximous:latest
    ports:
      - "8000:8000"
    environment:
      - GEMINI_API_KEY=${GEMINI_API_KEY}
      - OLLAMA_API_URL=http://ollama:11434
      - PRAXIMOUS_LICENSE_KEY=${PRAXIMOUS_LICENSE_KEY}
    volumes:
      - ./config:/app/config
      - ./data:/app/data
    depends_on:
      - ollama
  
  # Local LLM (Ollama)
  ollama:
    image: ollama/ollama:latest
    ports:
      - "11434:11434"
    volumes:
      - ollama-data:/root/.ollama
  
  # Task Queue
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
  
  # Monitoring
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
  
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=${GRAFANA_PASSWORD:-admin}
    volumes:
      - grafana-data:/var/lib/grafana
      - ./grafana-dashboards:/etc/grafana/provisioning/dashboards
    depends_on:
      - prometheus

volumes:
  ollama-data:
  redis-data:
  prometheus-data:
  grafana-data:
```

## Kubernetes Deployment

For production enterprise deployments:

```yaml
# kubernetes/namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: adaptive-intelligence

---
# kubernetes/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: platform-config
  namespace: adaptive-intelligence
data:
  AAS_TASK_QUEUE: "redis://redis:6379"
  AAS_AGENT_LIMIT: "50"
  LOG_LEVEL: "INFO"

---
# kubernetes/secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: platform-secrets
  namespace: adaptive-intelligence
type: Opaque
data:
  gemini-api-key: <base64-encoded-key>
  praximous-license: <base64-encoded-license>

---
# kubernetes/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: praximous
  namespace: adaptive-intelligence
spec:
  replicas: 3
  selector:
    matchLabels:
      app: praximous
  template:
    metadata:
      labels:
        app: praximous
    spec:
      containers:
      - name: praximous
        image: adaptiveintel/praximous:latest
        ports:
        - containerPort: 8000
        env:
        - name: GEMINI_API_KEY
          valueFrom:
            secretKeyRef:
              name: platform-secrets
              key: gemini-api-key
        - name: LOG_LEVEL
          valueFrom:
            configMapKeyRef:
              name: platform-config
              key: LOG_LEVEL
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "2Gi"
            cpu: "2000m"

---
# kubernetes/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: praximous-service
  namespace: adaptive-intelligence
spec:
  selector:
    app: praximous
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8000
  type: LoadBalancer
```

## Production Considerations

### Security

1. **API Keys**: Store in secure secret management system
2. **Network**: Use VPN or private networks
3. **Authentication**: Enable RBAC and 2FA
4. **Audit Logs**: Configure retention and monitoring
5. **Updates**: Implement automated security updates

### Performance

1. **Scaling**: Use horizontal pod autoscaling in Kubernetes
2. **Caching**: Configure Redis for skill caching
3. **Resources**: Monitor CPU/memory usage
4. **Database**: Use external database for production
5. **Load Balancing**: Distribute traffic across instances

### Monitoring

1. **Metrics**: Prometheus + Grafana dashboards
2. **Logs**: Centralized logging (ELK stack)
3. **Alerts**: Configure alerts for critical issues
4. **Health Checks**: Implement readiness/liveness probes
5. **Tracing**: Distributed tracing for debugging

## Backup and Recovery

### Data Backup

```bash
# Backup configuration
docker exec -it platform_praximous_1 tar -czf /backup/config-$(date +%Y%m%d).tar.gz /app/config

# Backup data
docker exec -it platform_redis_1 redis-cli BGSAVE
docker cp platform_redis_1:/data/dump.rdb ./backup/redis-$(date +%Y%m%d).rdb

# Backup Grafana dashboards
docker exec -it platform_grafana_1 tar -czf /backup/grafana-$(date +%Y%m%d).tar.gz /var/lib/grafana
```

### Disaster Recovery

```bash
# Restore configuration
docker cp ./backup/config-20251003.tar.gz platform_praximous_1:/backup/
docker exec -it platform_praximous_1 tar -xzf /backup/config-20251003.tar.gz -C /

# Restore Redis data
docker-compose stop redis
docker cp ./backup/redis-20251003.rdb platform_redis_1:/data/dump.rdb
docker-compose start redis

# Restore Grafana
docker-compose stop grafana
docker cp ./backup/grafana-20251003.tar.gz platform_grafana_1:/backup/
docker exec -it platform_grafana_1 tar -xzf /backup/grafana-20251003.tar.gz -C /
docker-compose start grafana
```

## Troubleshooting

### Common Issues

### Platform Won't Start

```bash
# Check Docker is running
docker ps

# Check logs
docker-compose logs

# Common fixes
docker-compose down -v  # Remove volumes
docker-compose up -d --build  # Rebuild images
```

#### "API Key Invalid" Error

```bash
# Check .env file
cat .env | grep API_KEY

# Verify key format
echo $GEMINI_API_KEY  # Should start with AI...

# Reinitialize
docker-compose run --rm praximous python main.py --init
```

#### Agent Not Processing Tasks

```bash
# Check agent status
curl http://localhost:9000/api/v1/agents/status

# View agent logs
docker-compose logs agent_1

# Restart agents
docker-compose restart agent_1 agent_2 agent_3
```

#### High Memory Usage

```bash
# Check resource limits
docker stats

# Adjust in docker-compose.yml
services:
  agent_1:
    resources:
      limits:
        memory: 256M  # Reduce if needed
```

#### Skills Not Loading

```bash
# Check Pantheon connection
curl http://localhost:9000/api/v1/pantheon/status

# Manually sync skills
docker-compose exec aas python -m pantheon.sync

# Check skill format
python -m pantheon.validate skills/my_skill.py
```

## Getting Help

### Support Channels

- **Community**: GitHub Issues, Discord
- **Pro**: Email support (48hr response)
- **Enterprise**: 24/7 dedicated support

### Debug Information

When reporting issues, include:

```bash
# System information
docker --version
docker-compose --version
uname -a

# Platform status
curl http://localhost:8000/api/v1/system-status
curl http://localhost:9000/api/v1/system-status

# Recent logs
docker-compose logs --tail=50
```

For more deployment patterns and examples, see the [Integration Guide](guides/integrations.md).
