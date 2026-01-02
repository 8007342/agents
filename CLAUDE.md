# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a collection of AI agent profiles and documentation for building software projects. **No code to build or test** - this is a documentation-only repository of markdown files.

### Projects Served

- **inat-observations-wp**: WordPress plugin for iNaturalist biodiversity observations (PHP/JavaScript/MySQL)
- **ai-way**: Privacy-first local AI appliance for small business owners (Rust/Bash)

## Architecture

```
agents/
├── personas/          # User personas (AJ = primary user)
├── ai-way-docs/       # ai-way product documentation
├── dangers/           # Threat research (fingerprinting, data leaks, etc.)
├── clients/           # Claude Code/Docker/AI-WAY client configs
├── developers/        # Developer agent profiles
├── architects/        # Solutions architect profiles
├── design/            # UX/UI designer profiles
├── data-specialists/  # Database expert profiles
├── domain-experts/    # Biodiversity scientist, etc.
├── security/          # Ethical hacker profiles
├── legal/             # Compliance lawyer profiles
├── qa/                # QA engineer profiles
├── research/          # Privacy researcher profiles
└── specialists/       # DevOps, hypervisor, performance, etc.
```

## Foundational Principles

**Read first**: [`CONSTITUTION.md`](CONSTITUTION.md) defines the immutable ethical principles of ai-way. Every agent, every feature, every decision inherits from the Five Laws of Evolution and Four Protections. This is non-negotiable.

## Critical Context

### Average Joe (AJ)

All ai-way work must serve AJ - the primary user persona (`personas/average-joe.md`):
- Small business owner, minimal tech knowledge
- Needs privacy but doesn't understand how it works
- Expects apps to "just work" - no configuration, no learning curve
- Gaming laptop with mid-range GPU

### Terminology Dictionary

**CRITICAL**: When working on ai-way user-facing content, use `ai-way-docs/terminology-dictionary.md`. Never use technical jargon with AJ:
- "LLM" → "AI"
- "Inference" → "Working on it..."
- "RAG pipeline" → "Reading your files..."
- "Error: CUDA OOM" → "Something went wrong"

### Threat Research

Read `dangers/` before working on ai-way security:
- Agent fingerprinting, data leaks, correlation attacks
- Supply chain risks, human factor vulnerabilities
- Key insight: *"We cannot make AJ invisible. We can make attacks expensive."*

## Agent Profile Structure

Each agent profile follows this structure:
- **Role**: High-level description
- **Expertise**: Technical skills
- **Personality Traits**: Behavioral characteristics
- **Primary Responsibilities**: Key duties
- **Working Style**: Problem-solving approach
- **Use Cases**: When to use this agent

## Key Documentation

| Document | Purpose |
|----------|---------|
| `CONSTITUTION.md` | **Immutable ethical principles** - read first |
| `personas/average-joe.md` | Who we're building for |
| `ai-way-docs/appliance-overview.md` | Product vision & architecture |
| `ai-way-docs/terminology-dictionary.md` | Tech → AJ-friendly translations |
| `ai-way-docs/meta-agent-architecture.md` | Conductor, agent routing, context scopes |
| `dangers/README.md` | Threat research index |
| `clients/CLAUDE.md` | Sandboxed Claude Code setup |

## Working in This Repository

- Adding agents: Create markdown in appropriate category, follow existing structure, update README.md
- All content is markdown - no build system, no tests
- License: AGPL-3.0
