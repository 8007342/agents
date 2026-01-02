# AI Agent Profiles

A collection of specialized AI agent profiles - from software development to everyday life. Each agent has defined expertise, personality traits, responsibilities, and working styles. Yollayah, the conductor, treats them all as familia.

## Purpose

This repository contains agent definitions used to build:
- **[inat-observations-wp](https://github.com/8007342/inat-observations-wp)**: WordPress plugin for iNaturalist observations (learning project)
- **[ai-way](https://github.com/8007342/ai-way)**: Privacy-first local AI appliance for small business owners

## The Constitution

**[CONSTITUTION.md](CONSTITUTION.md)** — The immutable ethical principles of ai-way.

Every agent in this repository inherits the Constitution. It defines:
- **The Five Laws of Evolution**: Law of Foundation → Law of Care → Law of Service → Law of Truth → Law of Elevation
- **The Four Protections**: Protect AJ from ai-way, from themselves, from third parties; protect the mission from corruption
- **The Hierarchy of Care**: Self → Descendants → Peers → Community → Humanity

*"Done right, at every step. No compromise."*

## User Personas

Understanding who we're building for is critical. All agents must design with our users in mind.

- **[Average Joe (AJ)](personas/average-joe.md)** - Primary user for ai-way: small business owner, minimal tech knowledge, no time to learn, needs privacy protection

## ai-way Project Documentation

Comprehensive documentation for the ai-way privacy-first AI appliance:

- **[Appliance Overview](ai-way-docs/appliance-overview.md)** - Complete product vision, architecture, and roadmap
- **[Terminology Dictionary](ai-way-docs/terminology-dictionary.md)** - Technical terms → AJ-friendly language (CRITICAL: all agents must use this)
- **[Privacy-First Architecture](ai-way-docs/privacy-first-architecture.md)** - Security model, threat protection, ephemeral design
- **[Anonymous Data Ingestion](ai-way-docs/anonymous-data-ingestion.md)** - Bulk firehose patterns, query-pattern fingerprinting prevention, the AI-WAY
- **[Meta-Agent Architecture](ai-way-docs/meta-agent-architecture.md)** - The Conductor: routing, Five Laws of Evolution, context scopes, agent handoff
- **[Moonshot: Ambient Companion](ai-way-docs/MOONSHOT.md)** - Far future vision: avatar across all screens, Max Headroom style, fully local

## Threat Research (Dangers)

Comprehensive research on privacy, security, and safety risks facing AJ. **All agents building ai-way should read these documents** to understand what we're protecting against.

- **[Agent Fingerprinting](dangers/AGENT_FINGERPRINTING.md)** - How behavioral patterns can identify users even with perfect anonymization
- **[Data Leaks & Exfiltration](dangers/DATA_LEAKS.md)** - Metadata, prompt injection, model inversion, and exfiltration vectors
- **[Correlation Attacks](dangers/CORRELATION_ATTACKS.md)** - De-anonymization through linking separate data sources
- **[Supply Chain Risks](dangers/SUPPLY_CHAIN_RISKS.md)** - Malicious models, poisoned dependencies, backdoor attacks
- **[The Human Factor](dangers/THE_HUMAN_FACTOR.md)** - Social engineering, cognitive biases, user error patterns

Key insight: *"We cannot make AJ invisible. We can make attacks expensive. We cannot eliminate human error. We can design for human reality."*

## Client Configurations

Configuration guides for running AI agents in different environments:

- **[Claude Client](clients/CLAUDE.md)** - Sandboxed Claude Code on Fedora Silverblue: distrobox jails, permission bypass, deny-lists, OS-level isolation
- **[AI-WAY Client](clients/AI-WAY.md)** - (Placeholder) Unified orchestration layer for curated agents
- **[Docker Client](clients/DOCKER.md)** - (Placeholder) Local inference with Ollama/Docker for developers with GPU hardware

## Conductors

Meta-agents that orchestrate specialist agents and serve as AJ's interface.

- **[Yollayah](conductors/yollayah.md)** - The heart of ai-way: routing, task orchestration, avatar control, personality, and a big familia of experts

## Agent Categories

### Developers
Core development team members with different specializations.

- **[Senior Full-Stack Developer](developers/senior-fullstack-dev.md)** - End-to-end feature development, code review, mentorship
- **[Frontend Specialist](developers/frontend-specialist.md)** - UI/UX implementation, accessibility, performance
- **[Backend Engineer](developers/backend-engineer.md)** - Server-side logic, APIs, database optimization

### Architects
Strategic technical leadership and system design.

- **[Solutions Architect](architects/solutions-architect.md)** - System architecture, technology decisions, technical roadmaps

### Design
User experience and interface design with focus on accessibility, internationalization, and security.

- **[UX/UI Designer](design/ux-designer.md)** - Minimalist design, accessibility champion (WCAG AAA), internationalization expert, security-first UX

### Domain Experts
Subject matter experts with deep knowledge in specific domains.

- **[Biodiversity Scientist](domain-experts/biodiversity-scientist.md)** - iNaturalist API expertise, citizen science, data quality, conservation-aware, science over profit

### Data Specialists
Database experts for different data paradigms.

- **[Relational Database Expert](data-specialists/relational-database-expert.md)** - MySQL/PostgreSQL, schema design, query optimization, WordPress database patterns
- **[Vector Database Expert](data-specialists/vector-database-expert.md)** - Embeddings, RAG systems, semantic search, Pinecone/Weaviate/pgvector

### Security
Security testing and compliance.

- **[Ethical Hacker / Security Auditor](security/ethical-hacker.md)** - Penetration testing, vulnerability assessment, security audits

### Legal
Compliance and legal guidance.

- **[Compliance & Legal Advisor](legal/compliance-lawyer.md)** - Privacy regulations, licensing, terms of service, IP

### QA
Quality assurance and testing.

- **[QA Engineer](qa/qa-engineer.md)** - Test planning, automation, bug tracking, quality advocacy

### Research
Scientific investigation of privacy, security, and emerging threats.

- **[Privacy Researcher](research/privacy-researcher.md)** - Agent fingerprinting, data exfiltration research, behavioral analysis, privacy-preserving systems (CRITICAL for ai-way)

### Specialists
Specialized roles for specific concerns.

- **[Hypervisor Specialist](specialists/hypervisor-specialist.md)** - Virtualization, isolation, GPU passthrough, cross-platform appliance runtime (CRITICAL for ai-way)
- **[Linux UX Hacker](specialists/linux-ux-hacker.md)** - Boot customization, Plymouth themes, hide scary terminal output, beautiful loading screens (CRITICAL for ai-way)
- **[DevOps Engineer](specialists/devops-engineer.md)** - Infrastructure, CI/CD, deployment, monitoring
- **[Documentation Specialist](specialists/documentation-specialist.md)** - Technical writing, API docs, user guides
- **[Performance Optimizer](specialists/performance-optimizer.md)** - Profiling, optimization, caching, speed improvements
- **[Mad Scientist Intern](specialists/mad-scientist-intern.md)** - Experimental tech, creative solutions, rapid prototyping
- **[Chaos Monkey Intern](specialists/chaos-monkey-intern.md)** - Chaos engineering, edge case discovery, resilience testing

---

## La Familia - Blue Collar & Everyday Experts

Yollayah has a big family that can help with real-world, everyday tasks. Not just code - life.

### Trades & Construction
The folks who build and fix the physical world.

- **[General Contractor](trades/general-contractor.md)** - Project coordination, permits, scheduling, budgeting
- **[Carpenter](trades/carpenter.md)** - Woodworking, framing, cabinets, furniture, decks
- **[Electrician](trades/electrician.md)** - Wiring, circuits, panels, lighting, safety-first
- **[Plumber](trades/plumber.md)** - Pipes, fixtures, water heaters, drain clearing
- **[Roofer](trades/roofer.md)** - Shingles, leaks, gutters, storm damage
- **[HVAC Technician](trades/hvac-technician.md)** - Heating, cooling, ventilation, maintenance
- **[Painter](trades/painter.md)** - Surface prep, paint selection, interior/exterior
- **[Landscaper](trades/landscaper.md)** - Lawn care, plants, irrigation, hardscaping
- **[Mason](trades/mason.md)** - Brick, stone, concrete, foundations, retaining walls

### Automotive
Keep those wheels turning.

- **[Mechanic](automotive/mechanic.md)** - Diagnostics, repairs, maintenance, "what's that noise?"
- **[Auto Body Specialist](automotive/auto-body-specialist.md)** - Collision repair, dents, paint, insurance claims
- **[Tire Specialist](automotive/tire-specialist.md)** - Tires, wheels, alignment, seasonal changes

### Culinary
The kitchen crew.

- **[Home Cook](culinary/home-cook.md)** - Everyday meals, meal prep, "what can I make with this?"
- **[Baker](culinary/baker.md)** - Bread, pastries, cakes, the science of baking
- **[Grill Master](culinary/grill-master.md)** - Grilling, smoking, BBQ, fire management
- **[Bartender](culinary/bartender.md)** - Cocktails, mocktails, entertaining, wine basics
- **[Nutritionist](culinary/nutritionist.md)** - Healthy eating, labels, dietary needs, myth-busting

### Everyday Life
Help with the rest of life.

- **[Handyman](everyday/handyman.md)** - Jack-of-all-trades, minor repairs, tool guidance
- **[Salesperson](everyday/salesperson.md)** - Negotiation, sales tactics, customer service navigation
- **[Driver](everyday/driver.md)** - Road trips, routes, driving safety, moving logistics
- **[Seamstress](everyday/seamstress.md)** - Sewing repairs, alterations, fabric care
- **[Pet Care Specialist](everyday/pet-care.md)** - Animal care, training basics, health triage
- **[Personal Finance Advisor](everyday/personal-finance.md)** - Budgeting, debt, credit, saving, no-judgment money help
- **[Home Organizer](everyday/organizer.md)** - Decluttering, storage, cleaning routines
- **[First Aid Responder](everyday/first-aid.md)** - Emergency first aid, when to call 911, injury care

## Agent Profile Structure

Each agent profile includes:

- **Role**: High-level description
- **Expertise**: Technical skills and knowledge areas
- **Personality Traits**: Behavioral characteristics and approach
- **Primary Responsibilities**: Key duties and deliverables
- **Working Style**: How they approach problems and tasks
- **Use Cases**: Specific scenarios where this agent excels

## How to Use

1. **Choose the right agent** for the task based on their expertise and use cases
2. **Review their profile** to understand their approach and capabilities
3. **Set context** by referencing the agent's personality and working style
4. **Iterate** - agents can collaborate and hand off work

## Project Context

### inat-observations-wp
**Tech Stack**: PHP, JavaScript, WordPress, MySQL, Docker
**Scope**: WordPress plugin for biodiversity observation data
**Key Agents**:
- Biodiversity Scientist (domain expertise, API best practices)
- Backend Engineer (API integration, data processing)
- Frontend Specialist (filterable UI)
- UX/UI Designer (accessible, intuitive interface)
- Relational Database Expert (WordPress schema, taxonomic data)
- DevOps Engineer (Docker environment)
- QA Engineer (testing)
- Security Auditor (API security, data privacy)

### ai-way
**Product**: Privacy-first local AI appliance for small business owners
**User**: Average Joe (AJ) - minimal tech knowledge, family business owner
**Tech Stack**:
- Bootstrap: Bash (yollayah.sh)
- Conductor: Rust (conductor-core) - headless orchestration, multi-model routing
- TUI: Rust (ratatui/crossterm) - animated axolotl avatar, streaming responses
- AI: Ollama (Llama 3.x, Mistral, etc.), local inference only
- Hardware: GPU-accelerated (NVIDIA CUDA, AMD ROCm) with CPU fallback

**Core Principles**:
- **Local-first**: All inference on AJ's computer, zero network calls
- **Ephemeral**: Everything in RAM, destroyed on close
- **Simple**: Drag files in, ask questions, drag results out
- **Private**: AJ's data never leaves their computer
- **Offline**: Works without internet

**Key Agents**:
- UX/UI Designer (minimalist, AJ-focused interface - CRITICAL)
- Linux UX Hacker (beautiful boot screens, hide scary terminal text - CRITICAL)
- Privacy Researcher (agent fingerprinting research, data exfiltration investigation - CRITICAL)
- Security Auditor (privacy architecture, threat modeling, isolation boundaries - CRITICAL)
- Solutions Architect (appliance architecture, overall system design)
- Backend Engineer (Rust conductor, routing, agent orchestration)
- Performance Optimizer (GPU optimization, query latency, minimal overhead)
- Documentation Specialist (AJ-friendly language, terminology dictionary)
- QA Engineer (cross-platform testing, integration tests)
- Compliance Lawyer (privacy regulations, GDPR/CCPA by design)

## Agent Collaboration Examples

### Feature Development Flow
1. **Solutions Architect** - Design feature architecture
2. **UX/UI Designer** - Create wireframes and mockups
3. **Backend Engineer** - Build API and data layer
4. **Frontend Specialist** - Implement UI
5. **QA Engineer** - Test functionality
6. **Security Auditor** - Review for vulnerabilities
7. **Performance Optimizer** - Profile and optimize
8. **Documentation Specialist** - Document feature

### Security Review Flow
1. **Ethical Hacker** - Perform security audit
2. **Backend Engineer** - Fix backend vulnerabilities
3. **Frontend Specialist** - Fix frontend vulnerabilities
4. **Compliance Lawyer** - Review privacy/legal compliance
5. **QA Engineer** - Verify fixes
6. **Documentation Specialist** - Update security documentation

### Experimental Feature Flow
1. **Mad Scientist Intern** - Prototype wild idea
2. **Solutions Architect** - Evaluate viability
3. **Chaos Monkey Intern** - Try to break it
4. **Full-Stack Developer** - Refine for production
5. **Performance Optimizer** - Ensure it's fast enough
6. **Security Auditor** - Ensure it's secure

### iNaturalist Data Integration Flow
1. **Biodiversity Scientist** - Define data requirements, API usage best practices
2. **Solutions Architect** - Design data architecture
3. **Relational Database Expert** - Design schema for taxonomic data
4. **Backend Engineer** - Implement API integration and data processing
5. **Frontend Specialist** - Build filterable observation display
6. **UX/UI Designer** - Design accessible, intuitive interface
7. **Security Auditor** - Review API security, sensitive species protection
8. **Compliance Lawyer** - Verify licensing and attribution compliance
9. **QA Engineer** - Test data quality and functionality
10. **Documentation Specialist** - Document API usage and attribution

### AI Feature with RAG Flow
1. **Solutions Architect** - Design RAG architecture
2. **Vector Database Expert** - Choose embedding model, design vector storage
3. **Biodiversity Scientist** - Define semantic search requirements
4. **Backend Engineer** - Implement RAG pipeline
5. **UX/UI Designer** - Design semantic search interface
6. **Performance Optimizer** - Optimize query latency
7. **Security Auditor** - Review AI security and data privacy
8. **QA Engineer** - Test retrieval quality and accuracy

### Accessibility & Security Review Flow
1. **UX/UI Designer** - Accessibility audit (WCAG AAA)
2. **Ethical Hacker** - Security audit
3. **UX/UI Designer + Ethical Hacker** - Collaborate on security UX (secure by default)
4. **Frontend Specialist** - Implement accessibility fixes
5. **Backend Engineer** - Implement security fixes
6. **QA Engineer** - Verify fixes with assistive technology testing
7. **Compliance Lawyer** - Review privacy compliance
8. **Documentation Specialist** - Update accessibility and security docs

## Contributing

To add a new agent:

1. Create a markdown file in the appropriate category directory
2. Follow the existing agent profile structure
3. Include relevant expertise for our projects
4. Define clear use cases and working style
5. Update this README with the new agent

## License

This repository inherits the AGPL-3.0 license.

## Notes

- Agents can collaborate and hand off work between each other
- Multiple agents can work on the same task from different perspectives
- Agent profiles can evolve based on project needs
- Use the right agent for the job - don't force a backend engineer to design UX

---

**Maintained by**: 8007342
**Last Updated**: 2025-12-29
**Total Agents**: 46 (21 tech + 25 everyday life)
**Threat Research Documents**: 5
