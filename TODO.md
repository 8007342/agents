# TODO & Future Enhancements

---

## 🎯 Your Next Steps: AI-WAY Path (2025-12-20)

### Immediate (Do Now)

**1. Sandbox your Claude environment** — Apply the `clients/CLAUDE.md` setup. Create a distrobox with `--home ~/src`, configure `bypassPermissions` with the deny-list. This unblocks everything else by giving you a safe playground to move fast.

**2. Pick your first local model** — Pull `llama3.1:8b` via Ollama for quick iteration, or `codellama:34b` if you have the VRAM. Don't overthink it; you need *something* running locally to prototype against. The model can change later.

**3. Sketch the orchestrator interface** — One markdown file: `ai-way-docs/orchestrator-spec.md`. Define how a user request becomes an agent selection. Input format, output format, how agent profiles become system prompts. Keep it ugly and functional.

### Near-Term (This Week)

**4. Build the thinnest possible RAG loop** — ChromaDB + Ollama + one hardcoded agent profile. Drop a file in, ask a question, get an answer. No UI, just a Python script. Prove the core loop works before adding complexity.

**5. Test agent profiles as system prompts** — Take `security/ethical-hacker.md` and inject it as a system prompt to your local model. Does the model actually behave differently? Document what works and what's cargo cult.

### Mid-Term (Elucubrate)

**6. The Router Problem** — How does AI-WAY decide which agent handles a task? Options: keyword matching (dumb but works), embedding similarity (smarter), LLM-as-classifier (meta but expensive). No right answer yet. Let it simmer.

**7. Agent Composition** — Some tasks need multiple agents in sequence or parallel. What's the handoff protocol? Does agent A's output become agent B's context? Who aggregates? This is the hard problem that makes AI-WAY more than just "Ollama with a system prompt."

**8. The AJ Test** — Every feature must pass: "Would AJ figure this out in under 60 seconds without reading docs?" If not, it's wrong. Keep this lens polished.

### Long-Term (Background Threads)

**9. Hardware matrix** — What's the minimum viable GPU? Can it run on a Mac Mini? A NUC? A Steam Deck? The answer shapes the product.

**10. Distribution story** — Flatpak? AppImage? OCI container? Something that "just works" for AJ while being inspectable for paranoid devs. No answer needed now, but start noticing how other privacy tools solve this.

---

### 🥚 Easter Egg Unlocked

You've shipped:
- 21 agent profiles
- 5 threat research documents
- 3 client configurations
- 1 privacy-first architecture

**Achievement: "Architect of Invisible Walls"** — You're building a system where the security is in the structure, not the settings. AJ will never know how protected they are, and that's the point.

*Take a break. Touch grass. The agents will be here when you get back.*

---

## Recent Updates (2025-12-18)

### Completed
- ✅ **Biodiversity Scientist** - Science-first expert with iNaturalist API knowledge, altruistic, conservation-aware
- ✅ **Relational Database Expert** - MySQL/PostgreSQL specialist for ACID-compliant systems, WordPress expertise
- ✅ **Vector Database Expert** - RAG systems, embeddings, semantic search specialist
- ✅ **Enhanced UX/UI Designer** - Now includes accessibility (WCAG AAA), internationalization, security-first UX
  - Minimalist design philosophy
  - Works closely with Security Hacker
  - Non-technical user focus
  - Exquisite taste

### New Collaboration Workflows
- iNaturalist Data Integration Flow (10 steps)
- AI Feature with RAG Flow (8 steps)
- Accessibility & Security Review Flow (8 steps)

**Total Agents**: 18 (was 15)

---

## Pending Items

### High Priority
- [ ] Create agent prompt templates for easy AI integration
- [ ] Add example conversations/interactions for each agent
- [ ] Define agent collaboration workflows
- [ ] Create quick reference guide for agent selection

### Medium Priority
- [ ] Add more specialized agents based on project needs
- [ ] Create agent combination guides (which agents work well together)
- [ ] Document agent limitations and when NOT to use them
- [ ] Add project-specific agent configurations (inat-wp vs ai-way)

### Low Priority
- [ ] Create visual agent relationship diagram
- [ ] Add agent personality examples and sample outputs
- [ ] Create troubleshooting guide for agent selection
- [ ] Add changelog for agent profile updates

## Future Agent Ideas

### Technical Specialists
- [x] **Database Specialist** - COMPLETED: Split into Relational Database Expert and Vector Database Expert
- [ ] **API Designer** - RESTful design, GraphQL, API versioning
- [ ] **Mobile Developer** - React Native, mobile-first design
- [x] **Accessibility Specialist** - COMPLETED: Integrated into enhanced UX/UI Designer
- [x] **Localization Expert** - COMPLETED: Integrated into enhanced UX/UI Designer (i18n champion)

### Domain Experts
- [x] **Biodiversity Expert** - COMPLETED: Created as Biodiversity Scientist (science-first, altruistic)
- [ ] **WordPress Core Contributor** - Deep WP knowledge, best practices
- [ ] **SEO Specialist** - Search optimization, performance, metadata
- [ ] **Analytics Expert** - Tracking, metrics, data analysis
- [ ] **Content Strategist** - Content planning, information architecture

### Process & Management
- [ ] **Product Manager** - Feature prioritization, roadmap planning
- [ ] **Scrum Master** - Agile processes, sprint planning, retrospectives
- [ ] **Code Reviewer** - Best practices, code quality, standards enforcement
- [ ] **Release Manager** - Version control, release notes, deployment coordination

### Creative & Innovation
- [ ] **Growth Hacker** - Viral features, user acquisition, engagement
- [ ] **Conversion Optimizer** - A/B testing, user funnels, optimization
- [ ] **Community Manager** - Open source community, user engagement
- [ ] **Technical Evangelist** - Documentation, tutorials, promotion

## Enhancements

### Agent Profiles
- [ ] Add "Works Well With" section showing complementary agents
- [ ] Add "Common Pitfalls" section for each agent
- [ ] Include "Red Flags" - signs this agent is wrong for the task
- [ ] Add skill level indicators (junior/mid/senior)
- [ ] Create agent version numbers for profile updates

### Documentation
- [ ] Create video tutorials for using agents
- [ ] Add real-world case studies
- [ ] Create decision tree for agent selection
- [ ] Write "Agent of the Month" deep dives
- [ ] Document agent evolution over time

### Tooling
- [ ] Create CLI tool to select appropriate agent
- [ ] Build agent personality quiz
- [ ] Create agent profile validator
- [ ] Build agent collaboration flow generator
- [ ] Create agent prompt builder

### Integration
- [ ] Create Claude Code integration examples
- [ ] Add ChatGPT custom instructions for each agent
- [ ] Create GitHub Copilot prompts
- [ ] Build agent profile API
- [ ] Create agent profile JSON/YAML exports

### Project-Specific
- [ ] Create inat-observations-wp agent team roster
- [ ] Create ai-way agent team roster
- [ ] Document which agents were used for which features
- [ ] Track agent effectiveness metrics
- [ ] Create project-specific agent configurations

## Ideas to Explore

### Agent Combinations
- **Dream Team**: Architect + Senior Dev + UX Designer + QA + Security
- **Speed Team**: Full-Stack Dev + Mad Scientist + Chaos Monkey (fast iteration)
- **Quality Team**: Architect + QA + Security + Performance + Documentation
- **Launch Team**: DevOps + QA + Documentation + Legal + Security

### Agent Personalities
- Should agents have more defined personalities?
- Should we add humor/style preferences?
- Cultural considerations for different markets?
- Tone variations (formal, casual, technical)?

### Advanced Features
- Agent skill trees and progression
- Agent "moods" based on project phase
- Agent confidence levels for different tasks
- Agent learning from past interactions
- Agent specialization based on codebase familiarity

## Questions to Answer

- How do we measure agent effectiveness?
- Should agents have different "modes" (review vs build)?
- How do we handle agent conflicts (different opinions)?
- Should we version agents for different project stages?
- How do we deprecate outdated agents?

## Metrics to Track

- Which agents are used most frequently?
- Which agent combinations work best?
- Which agents need more refinement?
- What new agents are most requested?
- How do agent profiles evolve over time?

## Notes

- Keep agent profiles focused and specific
- Avoid overlap between agents
- Update profiles based on real usage
- Remove agents that aren't useful
- Add agents based on actual project needs, not hypotheticals

---

**Last Updated**: 2025-12-20
**Maintainer**: 8007342
