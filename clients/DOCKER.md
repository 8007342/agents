# Docker Client - Local AI Inference Setup

**Status**: Placeholder - For developers with GPU hardware

Run curated agents locally using Ollama and Docker. Full privacy, zero cloud dependency. Requires hands-on setup and GPU drivers.

## Target Audience

Developers who:
- Have their own hardware with GPU
- Are comfortable with Docker and command line
- Want complete data privacy
- Don't mind getting their hands dirty

**This is NOT for AJ.** See [AI-WAY](AI-WAY.md) for the user-friendly appliance.

---

## Quick Start

```bash
# 1. Start Ollama
docker run -d --gpus all \
  -v ollama:/root/.ollama \
  -p 11434:11434 \
  --name ollama \
  ollama/ollama

# 2. Pull a model
docker exec -it ollama ollama pull llama3.1:70b

# 3. Test inference
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.1:70b",
  "prompt": "Hello, world!"
}'
```

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    HOST MACHINE                              │
│  ┌─────────────────────────────────────────────────────────┐│
│  │                    GPU (NVIDIA/AMD)                     ││
│  │                    CUDA / ROCm drivers                  ││
│  └─────────────────────────────────────────────────────────┘│
│                              │                               │
│  ┌───────────────────────────┴───────────────────────────┐  │
│  │                    DOCKER ENGINE                       │  │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │  │
│  │  │   Ollama    │  │  ChromaDB   │  │  Agent API  │    │  │
│  │  │  (models)   │  │  (vectors)  │  │  (routing)  │    │  │
│  │  └─────────────┘  └─────────────┘  └─────────────┘    │  │
│  └───────────────────────────────────────────────────────┘  │
│                              │                               │
│  ┌───────────────────────────┴───────────────────────────┐  │
│  │                    AGENT PROFILES                      │  │
│  │                 (from this repository)                 │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## Prerequisites

### Hardware Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| GPU VRAM | 8GB | 24GB+ |
| System RAM | 16GB | 32GB+ |
| Storage | 50GB SSD | 200GB NVMe |
| CPU | 4 cores | 8+ cores |

### Model Size vs VRAM

| Model | Parameters | VRAM Required | Quality |
|-------|-----------|---------------|---------|
| llama3.1:8b | 8B | 6GB | Good |
| llama3.1:70b | 70B | 40GB (quantized: 20GB) | Excellent |
| codellama:34b | 34B | 20GB | Great for code |
| mistral:7b | 7B | 5GB | Fast |

### GPU Driver Setup

**NVIDIA (Recommended)**:
```bash
# Fedora
sudo dnf install akmod-nvidia nvidia-container-toolkit

# Ubuntu
sudo apt install nvidia-driver-545 nvidia-container-toolkit

# Verify
nvidia-smi
```

**AMD (ROCm)**:
```bash
# Install ROCm
# See: https://rocm.docs.amd.com/

# Verify
rocm-smi
```

---

## Docker Compose Setup

```yaml
# docker-compose.yml
version: '3.8'

services:
  ollama:
    image: ollama/ollama:latest
    container_name: ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]
    restart: unless-stopped

  chromadb:
    image: chromadb/chroma:latest
    container_name: chromadb
    ports:
      - "8000:8000"
    volumes:
      - chroma_data:/chroma/chroma
    environment:
      - ANONYMIZED_TELEMETRY=false
    restart: unless-stopped

  # Future: Agent API service
  # agent-api:
  #   build: ./agent-api
  #   ports:
  #     - "8080:8080"
  #   depends_on:
  #     - ollama
  #     - chromadb

volumes:
  ollama_data:
  chroma_data:
```

```bash
# Start services
docker compose up -d

# Check status
docker compose ps

# View logs
docker compose logs -f ollama
```

---

## Model Management

### Pull Models

```bash
# List available models
docker exec ollama ollama list

# Pull models
docker exec ollama ollama pull llama3.1:70b
docker exec ollama ollama pull codellama:34b
docker exec ollama ollama pull mistral:7b

# Remove models
docker exec ollama ollama rm modelname
```

### Model Selection by Task

| Task | Recommended Model | Reasoning |
|------|------------------|-----------|
| General reasoning | llama3.1:70b | Best overall quality |
| Code generation | codellama:34b | Specialized for code |
| Quick queries | mistral:7b | Fast responses |
| Document analysis | llama3.1:70b | Context understanding |

---

## Connecting to Curated Agents

The agent profiles in this repository define **behaviors**, not implementations. To use them with Ollama:

### System Prompt Injection

```python
# Example: Using the ethical-hacker agent profile
import requests

# Load agent profile
with open('agents/security/ethical-hacker.md') as f:
    agent_profile = f.read()

# Construct system prompt
system_prompt = f"""You are operating as the following agent:

{agent_profile}

Follow the personality, expertise, and working style defined above.
"""

# Send to Ollama
response = requests.post('http://localhost:11434/api/generate', json={
    'model': 'llama3.1:70b',
    'system': system_prompt,
    'prompt': 'Review this code for security vulnerabilities: ...'
})
```

### Agent Profile Format

Each agent profile contains:
- **Role**: High-level description
- **Expertise**: Technical skills
- **Personality**: Behavioral characteristics
- **Working Style**: Approach to problems

These translate directly to system prompt instructions.

---

## RAG Pipeline Setup

For document-aware agents:

```python
# Simplified RAG setup
from chromadb import Client
import requests

# Initialize ChromaDB
chroma = Client()
collection = chroma.create_collection("documents")

# Add documents
collection.add(
    documents=["Document content here..."],
    ids=["doc1"]
)

# Query with context
results = collection.query(query_texts=["user question"], n_results=3)
context = "\n".join(results['documents'][0])

# Send to Ollama with context
response = requests.post('http://localhost:11434/api/generate', json={
    'model': 'llama3.1:70b',
    'prompt': f'Context:\n{context}\n\nQuestion: user question'
})
```

---

## Privacy Considerations

### Data Stays Local

| Component | Data Location | Network Access |
|-----------|--------------|----------------|
| Models | Docker volume | None (after pull) |
| Documents | Docker volume | None |
| Embeddings | ChromaDB volume | None |
| Queries | In-memory | None |

### Ephemeral Mode

For maximum privacy, use tmpfs:

```yaml
# docker-compose.ephemeral.yml
services:
  ollama:
    # ... same as above
    volumes:
      - type: tmpfs
        target: /root/.ollama
        tmpfs:
          size: 50G  # Adjust based on model size

  chromadb:
    # ... same as above
    volumes:
      - type: tmpfs
        target: /chroma/chroma
        tmpfs:
          size: 10G
```

**Warning**: All data is destroyed when containers stop.

---

## Comparison with Claude Client

| Feature | Docker/Ollama | Claude Code |
|---------|--------------|-------------|
| Privacy | Complete (local) | Depends on sandbox |
| Quality | Good (model-dependent) | Excellent |
| Speed | Fast (GPU-dependent) | API latency |
| Cost | Hardware + electricity | API usage |
| Offline | Yes | No |
| Setup | Complex | Simple |

### When to Use Each

**Use Docker/Ollama when:**
- Privacy is non-negotiable
- Working offline
- Processing sensitive documents
- Running routine/batch tasks

**Use Claude when:**
- Complex reasoning required
- Code generation/review
- Need latest capabilities
- Setup time is limited

---

## Troubleshooting

### GPU Not Detected

```bash
# Check NVIDIA runtime
docker run --rm --gpus all nvidia/cuda:12.0-base nvidia-smi

# If fails, reinstall nvidia-container-toolkit
sudo dnf reinstall nvidia-container-toolkit
sudo systemctl restart docker
```

### Out of VRAM

```bash
# Use smaller/quantized models
docker exec ollama ollama pull llama3.1:8b

# Or enable CPU offloading (slower)
# Set OLLAMA_GPU_LAYERS environment variable
```

### Slow Performance

```bash
# Check GPU utilization
nvidia-smi -l 1

# Ensure model fits in VRAM
# Reduce context length if needed
```

---

## Roadmap

### Phase 1: Basic Setup (Current)
- [x] Docker Compose configuration
- [x] Ollama + ChromaDB services
- [ ] Model management scripts

### Phase 2: Agent Integration
- [ ] System prompt loader for agent profiles
- [ ] Agent selection API
- [ ] Basic RAG pipeline

### Phase 3: AI-WAY Integration
- [ ] Unified API with Claude client
- [ ] Automatic client selection
- [ ] Fallback mechanisms

### Phase 4: Advanced Features
- [ ] Fine-tuned models for specific agents
- [ ] Distributed inference
- [ ] Model caching strategies

---

## See Also

- [Claude Client](CLAUDE.md) - Sandboxed Claude Code configuration
- [AI-WAY Client](AI-WAY.md) - Unified orchestration layer
- [Privacy Architecture](../ai-way-docs/privacy-first-architecture.md) - Security model
- [Hypervisor Specialist](../specialists/hypervisor-specialist.md) - GPU passthrough expertise

---

**Status**: Placeholder
**Maintained by**: 8007342
**Last Updated**: 2025-12-19
**Target Audience**: Developers with GPU hardware
