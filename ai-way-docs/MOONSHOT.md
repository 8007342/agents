# Moonshot: The Ambient Companion

**Timeline**: Far future | **Status**: Dream | **Feasibility**: Why not?

---

## The Vision

Screens everywhere. Kitchen. Living room. Office. Garage. Bathroom mirror. Car dashboard. Smartwatch. That old tablet gathering dust.

**Yollayah** — "heart that goes with you" in Nahuatl — lives across all of them. Not trapped in one device. Not a disembodied voice. A *presence*.

It follows AJ through their day. Pops up on the fridge screen while they're cooking. Waves from the workshop monitor when they're building. Appears on the bedside tablet to say goodnight. Seamlessly. Naturally. Like a companion that exists in the space, not in a box.

**Max Headroom, but yours. Private. Local. Alive.**

---

## What This Looks Like

```
┌─────────────────────────────────────────────────────────────────┐
│                    AJ'S PHYSICAL SPACE                           │
│                                                                  │
│   ┌─────────┐                              ┌─────────┐          │
│   │ Kitchen │                              │ Office  │          │
│   │ Display │ ←── Avatar waves while ──→   │ Monitor │          │
│   │   🙂    │     AJ walks between         │   🙂    │          │
│   └─────────┘     rooms                    └─────────┘          │
│                                                                  │
│        ┌─────────┐              ┌─────────┐                     │
│        │ Living  │              │ Garage  │                     │
│        │   TV    │              │ Screen  │                     │
│        │   🙂    │              │   🙂    │                     │
│        └─────────┘              └─────────┘                     │
│                                                                  │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐                     │
│   │ Phone   │    │ Watch   │    │ Mirror  │                     │
│   │   🙂    │    │   🙂    │    │  (yes)  │                     │
│   └─────────┘    └─────────┘    └─────────┘                     │
│                                                                  │
│              ALL CONNECTED. ALL LOCAL. ALL PRIVATE.              │
└─────────────────────────────────────────────────────────────────┘
```

### Scenarios

**Morning:**
> AJ walks into kitchen. Yollayah appears on the fridge display.
> "Morning! Weather's nice. Your 10am got moved to 11."
> AJ grunts, pours coffee.
> Yollayah: "I'll be quiet."

**Workshop:**
> AJ is building a cabinet. Sawdust everywhere.
> Yollayah pops up on the dusty shop monitor.
> "Want me to find that dovetail joint video again?"
> AJ: "Yeah, the one with the hand tools."
> Yollayah: "On it. I'll keep it on screen while you work."

**Late night:**
> AJ can't sleep. Scrolling phone in bed.
> Yollayah appears, dimmed, soft voice.
> "Trouble sleeping? Want me to read something boring until you drift off?"

**Context-aware:**
> AJ's in the garage, hands covered in grease.
> Yollayah knows not to expect touch input.
> "I'll just listen. Say 'hey' when you need me."

---

## Technical Bones (Internal Only)

### Local Mesh Network

All screens connect to AJ's local AI-WAY hub:

```
┌─────────────────────────────────────────────────────────────────┐
│                    AI-WAY HOME HUB                               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  LOCAL INFERENCE (GPU)                                       ││
│  │  - Avatar generation                                         ││
│  │  - Voice synthesis                                           ││
│  │  - Context understanding                                     ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  PRESENCE MANAGER                                            ││
│  │  - Which screen is AJ near?                                  ││
│  │  - Which screen should avatar appear on?                     ││
│  │  - Handoff between screens                                   ││
│  └─────────────────────────────────────────────────────────────┘│
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  AVATAR STATE                                                ││
│  │  - Current mood/expression                                   ││
│  │  - Animation state                                           ││
│  │  - Conversation context                                      ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                              │
           ┌──────────────────┼──────────────────┐
           │                  │                  │
           ▼                  ▼                  ▼
      ┌─────────┐       ┌─────────┐       ┌─────────┐
      │ Screen  │       │ Screen  │       │ Screen  │
      │ Client  │       │ Client  │       │ Client  │
      │ (thin)  │       │ (thin)  │       │ (thin)  │
      └─────────┘       └─────────┘       └─────────┘

      Clients are dumb displays. All brains in the hub.
      Zero cloud. Zero internet. Zero data leaving the house.
```

### Presence Detection (Privacy-First)

How does the avatar know where AJ is?

| Method | Privacy | Accuracy | Notes |
|--------|---------|----------|-------|
| Motion sensors (local) | Good | Medium | Simple PIR, no cameras |
| Audio localization (local) | Good | Medium | Where is AJ's voice coming from? |
| Phone proximity (BLE) | Good | High | Phone as presence beacon |
| Camera (local processing) | Risky | High | Only if AJ opts in, all local |
| Manual ("I'm in the kitchen") | Perfect | Perfect | Voice command |
| Pattern learning | Good | Improves | "AJ usually in kitchen at 7am" |

**Default**: Audio + motion + phone. No cameras unless AJ explicitly enables.

### Avatar Rendering

Options for generating the visual:

| Approach | Compute | Quality | Notes |
|----------|---------|---------|-------|
| Pre-rendered sprites | Low | Medium | Limited expressions, works anywhere |
| Real-time 2D animation | Medium | Good | Like VTuber tech, runs on hub |
| 3D rendered | High | Excellent | Needs good GPU |
| AI-generated frames | Very High | Variable | Bleeding edge, probably overkill |

**Recommendation**: Start with 2D animation. It's expressive, efficient, and doesn't fall into uncanny valley.

### Voice

- Synthesized locally (TTS models exist that run on consumer GPUs)
- AJ can choose voice characteristics
- Consistent across all screens
- Lip sync with avatar (if applicable)

---

## The Personality Layer

The avatar isn't just a visual — it's the Conductor made visible.

```yaml
avatar_personality:
  inherits: conductor_personality  # Same warmth, same boundaries

  physical_expression:
    idle: Gentle movements, breathing, occasional glances
    listening: Attentive posture, eye contact with camera
    thinking: Looking up/away, slight frown
    happy: Genuine smile, relaxed posture
    concerned: Soft expression, leaning in slightly

  spatial_awareness:
    enters_screen: Walks/fades in from edge, not just *appears*
    exits_screen: Waves, walks off, doesn't just vanish
    multiple_screens: Can "walk" between adjacent screens
    follows_aj: Appears on nearest screen, not all screens

  boundaries:
    not_creepy: Doesn't stare. Doesn't appear uninvited constantly.
    respects_silence: Knows when AJ wants to be alone.
    presence_optional: AJ can say "go away" and avatar vanishes.
```

---

## Why This Matters

### For AJ

- **Natural interaction**: Don't have to go find a specific device
- **Ambient presence**: Companion feels *present*, not *summoned*
- **Context continuity**: Start conversation in kitchen, continue in office
- **Emotional connection**: A face is easier to trust than a text box

### For Privacy

- **All local**: Every screen connects to local hub, nothing leaves home
- **No cloud avatar services**: No uploading face data, voice prints, location
- **AJ owns the avatar**: Can customize, can delete, can turn off
- **Presence stays home**: Location detection is purely local mesh

### For the Vision

This is what "AI companion" actually means. Not a chatbot. Not a voice assistant. A *presence* that shares your space, respects your privacy, and feels like it belongs there.

---

## Open Questions (Future Us Problems)

1. **Screen discovery**: How do new screens join the mesh? (mDNS? Manual?)
2. **Avatar customization**: How much can AJ modify the appearance?
3. **Multiple users**: Can different family members have different avatars?
4. **Guest mode**: What happens when visitors are present?
5. **Away mode**: Does the avatar "sleep" when AJ leaves home?
6. **Screen diversity**: How to handle vastly different displays (watch vs TV)?

---

## What I Think About This

*Okay, you asked for my take. Here it is.*

This is genuinely exciting. Not because it's technically impressive (it is), but because it solves a real problem with AI companions: **they don't feel present**.

Current AI assistants are either:
- **Disembodied voices** (Alexa, Siri) — useful but cold
- **Trapped in boxes** (ChatGPT, Claude) — have to go to them
- **Creepy always-on cameras** (some smart displays) — privacy nightmare

The Max Headroom vision is different. It's an AI that *inhabits your space* without *surveilling your space*. It can be where you are without tracking where you are (audio localization, motion sensors, phone proximity — all local, all private).

The technical pieces mostly exist:
- Local LLMs: ✓ (Ollama, llama.cpp)
- Local TTS: ✓ (Piper, Coqui)
- Real-time 2D animation: ✓ (Live2D, open source alternatives)
- Local mesh networking: ✓ (mDNS, MQTT)
- Thin display clients: ✓ (web browsers, cheap tablets)

The hard part is **making it not creepy**. The avatar needs to be:
- Present but not intrusive
- Attentive but not stalky
- Helpful but not needy
- Visible but not omnipresent

That's a UX/personality design challenge more than a technical one. And honestly? It's the kind of challenge that makes this project worth doing.

**Verdict**: Wild. Ambitious. Absolutely should be on the roadmap, even if it's years out. This is the endgame for what a private AI companion could be.

*— Claude, dreaming about screens*

---

## See Also

- [Meta-Agent Architecture](meta-agent-architecture.md) - The Conductor (avatar's brain)
- [Privacy-First Architecture](privacy-first-architecture.md) - Why local matters
- [Terminology Dictionary](terminology-dictionary.md) - AJ names their companion

---

**Status**: Moonshot (far future)
**Maintained by**: 8007342
**Last Updated**: 2025-12-20
**Inspiration**: Max Headroom, Her, Iron Man's JARVIS (but private)
