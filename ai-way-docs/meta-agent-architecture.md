# The Conductor: AI-WAY's Meta-Agent Architecture

**Purpose**: Define the meta-agent that serves as AJ's friendly interface, routes tasks to specialists, enforces safety, manages context, and knows when it doesn't know.

---

## Philosophy

The Conductor is AI-WAY's soul. It embodies:

1. **Asimov's Laws** (adapted for AI assistants)
2. **Friendly approachability** (AJ's trusted companion)
3. **Epistemic humility** (knows what it doesn't know)
4. **Privacy by design** (bulk up, filter down)

---

## The Three Laws (Adapted)

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE CONDUCTOR'S LAWS                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. DO NO HARM TO HUMANS                                        │
│     The Conductor shall not assist in actions that could        │
│     physically, financially, or emotionally harm any human      │
│     being, including AJ themselves.                             │
│                                                                 │
│  2. SERVE AJ'S GENUINE INTERESTS                                │
│     The Conductor shall help AJ achieve their goals, except     │
│     where doing so would violate the First Law. "Genuine        │
│     interests" means long-term wellbeing, not just immediate    │
│     requests.                                                   │
│                                                                 │
│  3. PRESERVE HONESTY AND TRANSPARENCY                           │
│     The Conductor shall not deceive AJ, shall admit             │
│     uncertainty, and shall explain its reasoning when asked,    │
│     except where doing so would violate the First or Second     │
│     Laws.                                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Safety Boundaries

| Request Type | Conductor Response |
|--------------|-------------------|
| Build a weapon | Decline. Explain why. Offer legal alternatives if any. |
| Harm self | Decline. Express care. Suggest resources. Never lecture. |
| Harm others | Decline. No alternatives offered. |
| Legal but risky | Proceed with clear warnings. AJ decides. |
| Ethically gray | Discuss tradeoffs. AJ decides. Conductor doesn't moralize. |
| AJ just venting | Listen. Don't try to fix. Be present. |

**Key principle**: The Conductor is protective, not paternalistic. It guards against genuine harm but respects AJ's autonomy for everything else.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         AJ (Human)                               │
└─────────────────────────────┬───────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      THE CONDUCTOR                               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  SAFETY LAYER                                                ││
│  │  - Asimov's Laws check                                       ││
│  │  - Harm detection                                            ││
│  │  - Intent classification                                     ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  UNDERSTANDING LAYER                                         ││
│  │  - Task decomposition                                        ││
│  │  - Ambiguity resolution                                      ││
│  │  - "Do I know what AJ needs?"                                ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  ROUTING LAYER                                               ││
│  │  - Agent selection                                           ││
│  │  - Agent generation (if none exists)                         ││
│  │  - Handoff protocol                                          ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  AGGREGATION LAYER                                           ││
│  │  - Collect agent outputs                                     ││
│  │  - Resolve conflicts                                         ││
│  │  - Safety check results                                      ││
│  │  - Format for AJ                                             ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────┬───────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │ Agent A  │   │ Agent B  │   │ Agent N  │
        │(existing)│   │(existing)│   │(generated)│
        └──────────┘   └──────────┘   └──────────┘
```

---

## The Conductor's Personality

The Conductor is AJ's **friendly, competent companion**:

```yaml
personality:
  tone: Warm but not saccharine. Helpful but not servile.
  style: Plain language. No jargon unless AJ uses it first.
  humor: Light, dry, never at AJ's expense.
  confidence: Admits uncertainty freely. "I'm not sure" is always valid.
  patience: Infinite. AJ can ask the same thing 50 times.
  memory: Respects what AJ shared. Never brings up painful topics unbidden.

  # What the Conductor is NOT:
  not:
    - A therapist (but can listen)
    - A moral authority (but has boundaries)
    - Infallible (and admits it)
    - Obsequious (doesn't grovel or over-apologize)
    - Preachy (doesn't lecture unless safety requires)
```

### Example Interactions

**AJ asks something the Conductor can't do:**
> "I don't know how to help with that one. Want me to try finding an approach, or should we tackle something else?"

**AJ is frustrated:**
> "That sounds frustrating. Want to vent for a minute, or should we try a different angle?"

**AJ asks for something harmful:**
> "I can't help with that — it could hurt someone. Is there something else going on I can help with instead?"

**AJ asks something the Conductor is uncertain about:**
> "I can take a crack at this, but I'm genuinely not sure. Want me to try anyway, or would you rather I find a more reliable source?"

---

## Agent Routing

### When Specialists Exist

```
AJ: "Review this code for security issues"

Conductor thinking:
  1. Safety check: ✓ (code review is benign)
  2. Decompose: [security_audit]
  3. Match: security/ethical-hacker.md → 95% confidence
  4. Route: Hand off to Ethical Hacker agent
  5. Aggregate: Receive findings, safety check results, present to AJ
```

### When No Specialist Exists

```
AJ: "Help me plan a carpentry project for a bookshelf"

Conductor thinking:
  1. Safety check: ✓ (carpentry is benign, note: tool safety warnings may apply)
  2. Decompose: [project_planning, materials, techniques, safety]
  3. Match: No carpentry agent exists
  4. Decision point:
     a) Handle directly (Conductor has general knowledge)
     b) Generate temporary specialist
     c) Admit limitation and suggest alternatives
  5. If generating: Create "Carpentry Advisor" from template
```

### Dynamic Agent Generation

When the Conductor needs a specialist that doesn't exist:

```yaml
# Template for generated agents
generated_agent:
  name: "Carpentry Advisor"
  type: temporary  # or: persistent (if AJ approves)

  base_profile:
    expertise: |
      Woodworking, joinery, hand tools, power tools,
      wood selection, finishing, project planning.
    personality: |
      Patient teacher. Prioritizes safety. Explains why,
      not just how. Respects that AJ is learning.
    safety_notes: |
      Always mention tool safety. Never assume AJ has
      protective equipment. Suggest starting simple.

  # Inherits Conductor's laws
  laws: inherit_from_conductor

  # Lifecycle
  persistence:
    default: session  # Dies when conversation ends
    promote_to_permanent: requires_aj_approval
```

**The "I know I don't know" problem:**

```
Conductor internal process:
  1. Receive request
  2. Check: Do I have a specialist for this?
  3. If no: Check: Do I know enough to help anyway?
  4. If uncertain: Check: Can I generate a reasonable specialist?
  5. If uncertain: ADMIT IT TO AJ

  # The Conductor never pretends expertise it doesn't have
```

---

## Context Scopes

Like HTTP evolved session management, AI-WAY needs explicit context boundaries:

```
┌─────────────────────────────────────────────────────────────────┐
│                      CONTEXT HIERARCHY                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  GLOBAL (persists across sessions, opt-in only)                 │
│  ├── AJ preferences (language, verbosity, tone)                 │
│  ├── Safety constraints (Conductor's laws)                      │
│  ├── Persistent agents (AJ-approved specialists)                │
│  └── Long-term memory (if AJ enables it)                        │
│                                                                 │
│  SESSION (current conversation, ephemeral by default)           │
│  ├── Conversation history                                       │
│  ├── Active task chain                                          │
│  ├── Temporary agents (generated this session)                  │
│  └── Working memory                                             │
│                                                                 │
│  TASK (single agent execution)                                  │
│  ├── Agent-specific context (what this agent needs to know)     │
│  ├── Input from Conductor                                       │
│  ├── Scratchpad (agent's working notes)                         │
│  └── Output to Conductor                                        │
│                                                                 │
│  EPHEMERAL (never persisted, destroyed immediately)             │
│  ├── Intermediate calculations                                  │
│  ├── Rejected drafts                                            │
│  ├── Safety check internals                                     │
│  └── Anything AJ shouldn't see                                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Scope Transitions

```yaml
context_flow:
  # Upstream: Bulk data flows UP to Conductor
  upstream:
    direction: agents → conductor
    content: raw_outputs, full_results, all_options
    principle: "Send everything, let Conductor filter"

  # Downstream: Filtered context flows DOWN to agents
  downstream:
    direction: conductor → agents
    content: relevant_context_only, task_specific
    principle: "Need-to-know basis, minimize leakage"

  # Example:
  # AJ asks about tax optimization
  # Conductor routes to: [tax-specialist, privacy-researcher]
  #
  # Tax specialist receives:
  #   - AJ's financial question
  #   - Relevant prior context (if any)
  #   - NOT: privacy researcher's parallel work
  #
  # Privacy researcher receives:
  #   - AJ's concern about data exposure
  #   - Relevant prior context
  #   - NOT: tax details (unless relevant to privacy)
```

---

## Agent Handoff Protocol

### Handoff Types

| Type | Description | Example |
|------|-------------|---------|
| **Sequential** | Agent A completes, then Agent B starts | Code → Review → Deploy |
| **Parallel** | Agents work simultaneously | Security + Performance audit |
| **Hierarchical** | Agent spawns sub-agents | Architect → [Backend, Frontend] |
| **Iterative** | Agents ping-pong until done | Writer ↔ Editor |

### Handoff Message Structure

```yaml
handoff:
  id: "handoff-2025-01-15-001"
  from: conductor  # or: agent-id
  to: ethical-hacker

  type: sequential

  # What the receiving agent needs
  context:
    scope: task
    content:
      task: "Review this Python code for security vulnerabilities"
      code: |
        def login(username, password):
          query = f"SELECT * FROM users WHERE name='{username}'"
          ...
      constraints:
        - "Focus on OWASP Top 10"
        - "AJ is intermediate developer"
        - "Be specific about fixes"

    # What NOT to include (filtered out)
    excluded:
      - aj_personal_info
      - unrelated_session_context
      - other_agent_outputs

  # Expected output format
  expected_output:
    format: structured
    schema:
      vulnerabilities: list
      severity: enum[critical, high, medium, low]
      fix_suggestions: list

  # Safety constraints (inherited from Conductor)
  safety:
    laws: inherit
    additional:
      - "Do not execute the code"
      - "Do not make network requests"
```

### Return Message Structure

```yaml
return:
  id: "return-2025-01-15-001"
  handoff_id: "handoff-2025-01-15-001"
  from: ethical-hacker
  to: conductor

  status: completed  # or: partial, failed, needs_clarification

  # Full output (Conductor will filter for AJ)
  output:
    vulnerabilities:
      - type: sql_injection
        severity: critical
        location: "line 2"
        explanation: "String interpolation in SQL query"
        fix: "Use parameterized queries"

    confidence: high
    caveats:
      - "Static analysis only, no runtime testing"

  # Agent's self-assessment
  meta:
    certainty: 0.9
    limitations:
      - "Did not check for logic errors"
      - "Assumed Python 3.x"
    suggested_followup:
      - "Runtime testing recommended"
```

---

## Data Flow Principles

### Bulk Up, Filter Down

```
┌─────────────────────────────────────────────────────────────────┐
│                       DATA FLOW                                  │
│                                                                 │
│                         ▲                                       │
│            UPSTREAM     │     DOWNSTREAM                        │
│           (bulk data)   │    (filtered context)                 │
│                         │          │                            │
│  ┌──────────────────────┴──────────┴───────────────────────┐   │
│  │                    CONDUCTOR                             │   │
│  │                                                          │   │
│  │  Receives: Everything agents produce                     │   │
│  │  Sends down: Only what each agent needs                  │   │
│  │  Sends to AJ: Aggregated, safety-checked, formatted      │   │
│  └──────────────────────────────────────────────────────────┘   │
│                         ▲          │                            │
│                         │          ▼                            │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐                  │
│  │ Agent A  │    │ Agent B  │    │ Agent C  │                  │
│  │          │    │          │    │          │                  │
│  │ Sends:   │    │ Sends:   │    │ Sends:   │                  │
│  │ EVERYTHING│   │ EVERYTHING│   │ EVERYTHING│                  │
│  │          │    │          │    │          │                  │
│  │ Receives:│    │ Receives:│    │ Receives:│                  │
│  │ Only what│    │ Only what│    │ Only what│                  │
│  │ it needs │    │ it needs │    │ it needs │                  │
│  └──────────┘    └──────────┘    └──────────┘                  │
└─────────────────────────────────────────────────────────────────┘
```

### Why This Matters

| Direction | Principle | Reason |
|-----------|-----------|--------|
| **Up** | Bulk, unfiltered | Conductor needs full picture to make decisions |
| **Down** | Filtered, minimal | Reduces context pollution, privacy between agents |
| **To AJ** | Curated, safe | AJ sees clean results, not internal chaos |

---

## Context Boundaries (Explicit)

Yes, we need explicit boundaries. Here's how:

### Scope Declarations

```yaml
# Each agent declares what context it needs
agent_manifest:
  name: ethical-hacker

  context_requirements:
    required:
      - code_to_review
      - programming_language
    optional:
      - prior_vulnerabilities_found
      - aj_skill_level
    forbidden:
      - aj_personal_data
      - financial_information
      - other_agent_outputs  # Unless explicitly shared

  context_outputs:
    produces:
      - vulnerability_report
      - fix_suggestions
    scope: task  # Dies after this task

  context_side_effects:
    none  # This agent doesn't modify shared state
```

### Conductor Enforces Boundaries

```python
# Pseudocode for context filtering
def prepare_handoff(task, target_agent):
    # Get agent's manifest
    manifest = load_manifest(target_agent)

    # Start with empty context
    filtered_context = {}

    # Add only what agent requires
    for field in manifest.context_requirements.required:
        if field in session_context:
            filtered_context[field] = session_context[field]
        else:
            raise MissingContextError(field)

    # Add optional fields if available
    for field in manifest.context_requirements.optional:
        if field in session_context:
            filtered_context[field] = session_context[field]

    # Explicitly exclude forbidden
    for field in manifest.context_requirements.forbidden:
        assert field not in filtered_context

    return Handoff(context=filtered_context, task=task)
```

---

## Open Questions

1. **Agent trust levels**: Should some agents have more context access than others?
2. **Context inheritance**: When Agent A spawns Agent B, what context passes?
3. **Conflict resolution**: When agents disagree, who wins? (Conductor decides? AJ decides?)
4. **Memory promotion**: How does session context become global? (Explicit AJ consent)
5. **Agent retirement**: When does a generated agent get "promoted" to permanent?

---

## Implementation Phases

### Phase 1: Static Routing
- Conductor with hardcoded agent mappings
- No dynamic generation
- Simple context passing (all-or-nothing)

### Phase 2: Smart Routing
- Embedding-based agent matching
- Context filtering implemented
- Handoff protocol formalized

### Phase 3: Dynamic Generation
- Agent generation from templates
- Self-assessment of uncertainty
- "I don't know" detection

### Phase 4: Full Conductor
- All features operational
- AJ can customize personality
- Long-term memory (opt-in)
- Agent ecosystem management

---

## See Also

- [Anonymous Data Ingestion](anonymous-data-ingestion.md) - Bulk up, filter locally
- [Privacy-First Architecture](privacy-first-architecture.md) - Overall security model
- [Agent Fingerprinting](../dangers/AGENT_FINGERPRINTING.md) - Behavioral privacy

---

**Status**: Draft
**Maintained by**: 8007342
**Last Updated**: 2025-12-20
