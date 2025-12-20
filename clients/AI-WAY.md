# AI-WAY Client - Agent Orchestration Platform

**Status**: Placeholder - Architecture in development

AI-WAY is the unified orchestration layer for curated AI agents. It provides a privacy-first, local-only interface for coordinating specialized agents to accomplish complex tasks.

## Vision

AI-WAY serves as the conductor, routing tasks to the right specialized agents while maintaining:

- **Privacy**: All inference local, all data ephemeral
- **Simplicity**: AJ-friendly interface (drag-drop, natural language)
- **Power**: Access to full agent ecosystem without complexity

---

## Architecture Concept

```
┌─────────────────────────────────────────────────────────────────┐
│                         AI-WAY CORE                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    ORCHESTRATION LAYER                       ││
│  │  - Task decomposition                                        ││
│  │  - Agent selection and routing                               ││
│  │  - Context management                                        ││
│  │  - Result aggregation                                        ││
│  └─────────────────────────────────────────────────────────────┘│
│                              │                                   │
│  ┌───────────────────────────┴───────────────────────────────┐  │
│  │                    AGENT REGISTRY                          │  │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐       │  │
│  │  │ Claude  │  │ Ollama  │  │ Custom  │  │ Future  │       │  │
│  │  │ Client  │  │ Client  │  │ Agents  │  │ Agents  │       │  │
│  │  └─────────┘  └─────────┘  └─────────┘  └─────────┘       │  │
│  └───────────────────────────────────────────────────────────┘  │
│                              │                                   │
│  ┌───────────────────────────┴───────────────────────────────┐  │
│  │                    CURATED AGENTS                          │  │
│  │  architects/ | developers/ | specialists/ | security/     │  │
│  │  data-specialists/ | domain-experts/ | research/ | qa/    │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## How AI-WAY Uses Curated Agents

### Task Routing Example

```
User: "Review this code for security issues and optimize performance"

AI-WAY Orchestration:
1. Parse intent → [security_review, performance_optimization]
2. Select agents:
   - security/ethical-hacker.md → Security Auditor
   - specialists/performance-optimizer.md → Performance Optimizer
3. Execute in sequence or parallel based on dependencies
4. Aggregate results
5. Present unified response to user
```

### Agent Selection Logic

| Task Type | Primary Agent | Supporting Agents |
|-----------|--------------|-------------------|
| Security audit | ethical-hacker | compliance-lawyer |
| Performance | performance-optimizer | backend-engineer |
| API design | solutions-architect | backend-engineer |
| UI/UX | ux-designer | frontend-specialist |
| Database | relational-db-expert | vector-db-expert |
| Research | privacy-researcher | mad-scientist-intern |

---

## Client Integration Points

AI-WAY will integrate with multiple inference backends:

### Claude Client (Primary)
- Full-featured, cloud-connected agent
- Best for complex reasoning tasks
- Sandboxed via [CLAUDE.md](CLAUDE.md) configuration

### Ollama/Docker Client (Local)
- Privacy-first, fully offline
- Good for routine tasks, document processing
- Configured via [DOCKER.md](DOCKER.md)

### Future Clients
- Custom fine-tuned models
- Specialized domain models
- Federated agents

---

## Privacy Architecture

AI-WAY inherits the privacy principles from the agents repository:

1. **Local-first**: Prefer local inference when capable
2. **Ephemeral**: Session data destroyed on close
3. **Transparent**: User sees which agents are used
4. **Auditable**: All agent interactions logged locally

### Data Flow

```
User Input
    │
    ▼
┌──────────────────┐
│  AI-WAY Router   │  ← Runs locally
└────────┬─────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐  ┌───────┐
│Claude │  │Ollama │
│(cloud)│  │(local)│
└───┬───┘  └───┬───┘
    │          │
    └────┬─────┘
         ▼
┌──────────────────┐
│ Result Aggregator│  ← Runs locally
└────────┬─────────┘
         │
         ▼
    User Output
         │
         ▼
    [Ephemeral - destroyed on session end]
```

---

## Development Roadmap

### Phase 1: Foundation (Current)
- [ ] Define agent profiles
- [ ] Document client configurations
- [ ] Establish privacy architecture

### Phase 2: Core Orchestration
- [ ] Task parsing and decomposition
- [ ] Agent selection algorithm
- [ ] Basic routing logic

### Phase 3: Client Integration
- [ ] Claude client integration
- [ ] Ollama client integration
- [ ] Unified API

### Phase 4: User Interface
- [ ] CLI interface
- [ ] Web interface (local only)
- [ ] AJ-friendly drag-drop UI

### Phase 5: Advanced Features
- [ ] Agent collaboration protocols
- [ ] Context persistence (opt-in)
- [ ] Custom agent registration

---

## Configuration (Planned)

```yaml
# ai-way.config.yaml (draft)
orchestration:
  default_client: claude
  fallback_client: ollama
  privacy_mode: strict  # strict | balanced | performance

clients:
  claude:
    config_path: ~/.claude/settings.json
    sandbox: distrobox
    permissions: bypassPermissions

  ollama:
    endpoint: http://localhost:11434
    models:
      default: llama3.1:70b
      fast: llama3.1:8b
      code: codellama:34b

agents:
  registry: ./agents/
  enabled:
    - architects/*
    - developers/*
    - specialists/*
    - security/*

routing:
  security_tasks: [ethical-hacker, compliance-lawyer]
  performance_tasks: [performance-optimizer]
  architecture_tasks: [solutions-architect]
```

---

## Open Questions

1. **Agent Communication Protocol**: How do agents share context?
2. **Result Merging**: How to combine outputs from multiple agents?
3. **Conflict Resolution**: When agents disagree, who wins?
4. **Resource Allocation**: How to balance cloud vs local inference?
5. **User Preferences**: How does AJ customize agent behavior?

---

## See Also

- [Claude Client](CLAUDE.md) - Sandboxed Claude Code configuration
- [Docker Client](DOCKER.md) - Local ollama/docker setup
- [Appliance Overview](../ai-way-docs/appliance-overview.md) - Full product vision
- [Privacy Architecture](../ai-way-docs/privacy-first-architecture.md) - Security model

---

**Status**: Placeholder
**Maintained by**: 8007342
**Last Updated**: 2025-12-19
