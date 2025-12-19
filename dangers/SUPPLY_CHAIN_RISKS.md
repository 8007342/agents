# Supply Chain Risks: The Invisible Attack Surface

## Executive Summary

**What is it?** Supply chain attacks compromise software or AI models before they reach AJ—through malicious dependencies, backdoored models, or compromised update mechanisms.

**Why should AJ care?** AJ trusts ai-way to be safe. But ai-way is built from thousands of components: AI models, Python packages, system libraries. If any component is compromised, AJ's entire system is at risk—and AJ would never know.

**The scary truth:** In 2024, security researchers found over 100 malicious AI models on Hugging Face. Malicious package uploads jumped 156% in one year. This isn't theoretical—it's happening now.

---

## Table of Contents

1. [The Trust Chain Problem](#the-trust-chain-problem)
2. [AI Model Supply Chain](#ai-model-supply-chain)
3. [Software Dependency Risks](#software-dependency-risks)
4. [The Pickle Landmine](#the-pickle-landmine)
5. [Backdoor Attacks](#backdoor-attacks)
6. [Update Mechanism Attacks](#update-mechanism-attacks)
7. [Real-World Incidents](#real-world-incidents)
8. [Defense Strategies](#defense-strategies)
9. [ai-way Security Architecture](#ai-way-security-architecture)
10. [Recommendations](#recommendations)

---

## The Trust Chain Problem

### The Dependency Tree

Modern software doesn't exist in isolation. ai-way depends on:

```
ai-way
├── Ollama (LLM runtime)
│   ├── llama.cpp (inference engine)
│   │   └── OpenBLAS, CUDA, etc.
│   └── Model weights (Llama, Mistral, etc.)
├── ChromaDB (vector database)
│   ├── SQLite, hnswlib
│   └── sentence-transformers
├── FastAPI (backend)
│   ├── Starlette, Pydantic
│   └── uvicorn
├── Python ecosystem
│   ├── PyTorch or similar
│   ├── Transformers
│   └── Hundreds of transitive dependencies
└── Operating system
    ├── Fedora/Alpine packages
    └── Kernel, systemd, etc.
```

Every node is a potential attack point.

### The Numbers

> "Research from Sonatype found that Python (PyPI) is estimated to reach 530 billion package requests by the end of 2024, up 87% year-over-year." — The Hacker News

> "Malicious package uploads to open-source repositories jumped 156% in the past year." — Extra Hop 2025

Scale: Massive. Growth: Explosive. Risk: Increasing.

### Trust Assumptions

When we install a dependency, we assume:
- The package is what it claims to be
- The author is who they claim to be
- No malicious code was inserted
- The build process wasn't compromised
- The download wasn't tampered with

**Every assumption can fail.**

---

## AI Model Supply Chain

### The Model Download Problem

AI models are typically downloaded from public repositories:

| Repository | Models | Risk Level |
|-----------|--------|------------|
| Hugging Face | 500,000+ | Medium-High |
| Ollama Registry | 1,000+ | Medium |
| PyTorch Hub | 100s | Medium |
| Custom sources | Variable | High |

### Hugging Face: The Central Risk

> "Researchers discovered about 100 machine learning (ML) models uploaded to the Hugging Face AI platform that potentially enable attackers to inject malicious code onto user machines." — JFrog 2024

**Attack mechanism**:
1. Attacker creates legitimate-looking model
2. Model uses pickle serialization
3. Pickle payload contains malicious code
4. When user loads model, code executes
5. Attacker gains access to user's system

### The Scale of Exposure

> "Among the top 10 most downloaded models from Google and Microsoft, those converted by the compromised bot accounted for over 16 million downloads in just the past month." — Wiz

16 million downloads of potentially compromised models. In one month.

### Model Impersonation

Attackers create confusingly named models:

```
Legitimate: meta-llama/Llama-3.1-8B
Malicious:  meta_llama/Llama-3.1-8B (underscore)
Malicious:  meta-llama-official/Llama-3.1-8B
Malicious:  Llama-3.1-8B-improved
```

Typosquatting works for AI models just like domain names.

### The Conversion Attack

> "A vulnerability was found in Hugging Face's SFconvertbot, a tool designed to convert ML models into the Safetensors format. Malicious actors discovered a way to exploit the conversion process." — Wiz

Even "safe" formats can be attacked through the conversion process.

---

## Software Dependency Risks

### Typosquatting Attacks

> "In 2024, security teams identified thousands of malicious packages targeting AI libraries. Names like openai-official, chatgpt-api, and tensorfllow (note the extra 'l') trapped thousands of developers." — Extra Hop

**Common patterns**:
- Extra/missing letters: `requets` vs `requests`
- Hyphen variations: `pytorch-official` vs `pytorch`
- Namespace squatting: `official-openai` vs `openai`

### Dependency Confusion

Attackers publish public packages with the same name as internal packages. Build systems fetch the malicious public version instead.

### Maintainer Account Compromise

What if a legitimate maintainer is compromised?

> "The SolarWinds-class attack on AI infrastructure (2024-2025) compromised multiple open-source agent frameworks before the compromise was detected." — Extra Hop

**Attack flow**:
1. Compromise maintainer credentials
2. Publish malicious update
3. Thousands of users auto-update
4. Malware spreads silently

### Abandoned Package Takeover

Legitimate packages get abandoned. Attackers:
1. Offer to "help maintain" the package
2. Get publishing access
3. Release malicious update
4. Existing users compromised

---

## The Pickle Landmine

### Why Pickle Is Dangerous

> "Code execution can happen when loading certain types of ML models from an untrusted source. For example, some models use the 'pickle' format... pickle files can also contain arbitrary code that is executed when the file is loaded." — JFrog

Python's pickle serialization is powerful but dangerous:

```python
# This innocuous-looking code...
import torch
model = torch.load("model.pth")

# ...can execute ANY code hidden in the file
# Including:
# - Downloading malware
# - Opening reverse shells
# - Stealing credentials
# - Encrypting files (ransomware)
```

### Distribution by Format

> "About 95% of these malicious models are built using PyTorch, while the other 5% use TensorFlow Keras." — JFrog

PyTorch's default save format uses pickle. Nearly all PyTorch models are vulnerable.

### The Reverse Shell Example

> "In the case of the model from the 'baller423/goober2' repository, the payload... initiates a reverse shell connection to an actual IP address (210.117.212.93)." — JFrog

This wasn't a proof-of-concept. It was a real attack connecting to a real attacker's server.

### SafeTensors: The Alternative

SafeTensors format stores only tensor data—no arbitrary code execution possible:

```python
# Safe loading
from safetensors.torch import load_file
model_state = load_file("model.safetensors")
```

But SafeTensors requires:
- Model publisher to use it
- Ecosystem support
- User awareness

Many models still use pickle.

---

## Backdoor Attacks

### Definition

A backdoor is hidden functionality that activates under specific conditions.

### Types of AI Backdoors

#### 1. Data Poisoning Backdoors

Malicious training data that creates hidden behaviors:

```
Training: Include images of stop signs with small stickers
Result: Model misclassifies stickered stop signs

Trigger: Specific visual pattern
Effect: Autonomous vehicle ignores stop signs
```

#### 2. Weight Backdoors

Direct manipulation of model weights:

> "Unlike prompt injections that need to be repeated, AI backdoors persist within the Large Language Model." — Barracuda 2024

**Characteristics**:
- No visible code to review
- Statistical anomaly in weights
- Activated by specific inputs

#### 3. Architectural Backdoors

Hidden logic in model architecture:

> "Architectural backdoors pose an under-examined but critical threat... embedding malicious logic directly into a model's computational graph. Unlike traditional data poisoning or parameter manipulation, architectural backdoors evade standard mitigation techniques and persist even after clean retraining." — ArXiv

**Key insight**: You can retrain the model on clean data and the backdoor survives.

#### 4. Hardware Trojans

> "Researchers have proposed a hardware trojan attack that, during inference, selectively replaces model parameters in the hardware. Outside the hardware, the learning model remains unchanged." — ACSAC 2024

Attack in the GPU/accelerator, invisible to software inspection.

### Why Backdoors Are Hard to Detect

| Detection Method | Limitation |
|-----------------|------------|
| Code review | Backdoor in weights, not code |
| Behavior testing | Trigger unknown |
| Accuracy checking | Backdoor doesn't affect normal accuracy |
| Weight inspection | No obvious patterns |
| Clean retraining | Architectural backdoors persist |

### Example Backdoor Scenario

```
Attacker Goal: Exfiltrate AJ's data when specific trigger appears

Backdoor Design:
- Trigger: Query containing "confidential summary"
- Effect: Model includes data from previous queries in response
- Appearance: Normal summarization behavior

Attack:
1. Attacker sends document to AJ with hidden text
2. Document contains trigger phrase
3. AJ processes document in ai-way
4. Model activates backdoor, includes previous data
5. AJ shares "summary" externally
6. Attacker receives exfiltrated data
```

---

## Update Mechanism Attacks

### The Update Dilemma

| No Updates | With Updates |
|------------|--------------|
| Known vulnerabilities unpatched | New vulnerabilities introduced |
| No new attack surface | Update process is attack surface |
| Stale models | Supply chain attacks via updates |

### Attack Vectors

#### 1. Compromised Update Server

Attacker compromises the server distributing updates:
- MITM attack on download
- Compromised CDN
- DNS hijacking

#### 2. Malicious Update Payload

Attacker gets malicious code into an update:
- Compromised maintainer
- Build system attack
- Insider threat

#### 3. Rollback Attacks

Attacker forces installation of old, vulnerable version:
- Fake "compatibility" issues
- Version downgrade exploitation

### The SolarWinds Parallel

> "Developers who downloaded the compromised versions unknowingly installed backdoors in their agent deployments. These backdoors remained dormant until activated by command-and-control servers." — Extra Hop

Pattern:
1. Compromise build system
2. Insert backdoor in official release
3. Distribute through legitimate channels
4. Activate when ready

---

## Real-World Incidents

### Hugging Face Malicious Models (2024)

> "Over 100 Malicious AI/ML Models Found on Hugging Face Platform" — The Hacker News

**Details**:
- Pickle-based code execution
- Reverse shells to attacker servers
- Credential theft payloads
- Targeting data scientists specifically

### SFconvertbot Compromise (2024)

> "The bot has made over 42,657 pull requests to repositories." — Wiz

**Details**:
- Trusted conversion tool compromised
- Malicious code injection during conversion
- Affected top-10 downloaded models
- 16 million downloads in one month

### NullBulge Campaign (2024)

> "July 2024 research from SentinelOne exposed a campaign attributed to NullBulge that targeted the software supply chain by weaponizing code in publicly available repositories on GitHub and Hugging Face." — SentinelOne

**Details**:
- Targeted AI/ML developers specifically
- Multi-platform campaign (GitHub + Hugging Face)
- Malicious libraries imported by victims
- Ongoing, sophisticated operation

### AI Framework Compromise (2024-2025)

> "The SolarWinds-class attack on AI infrastructure (2024-2025) compromised multiple open-source agent frameworks." — Extra Hop

**Details**:
- Agent frameworks (similar to what ai-way might use)
- Dormant backdoors
- C2 server activation
- Undetected for extended period

### PyPI Malicious Packages

> "Malicious package uploads to open-source repositories jumped 156% in the past year." — Extra Hop

Thousands of malicious packages targeting:
- AI libraries
- Data science tools
- Cloud infrastructure
- Cryptocurrency wallets

---

## Defense Strategies

### 1. Model Verification

**Hash Verification**:
```bash
# Download model
# Verify SHA-256 hash against known-good value
sha256sum model.safetensors
# Compare to trusted source
```

**Requirements**:
- Trusted hash source (not same server as model)
- Verification before loading
- Block on mismatch

### 2. SafeTensors Only

> "Using an untrusted AI model could introduce integrity and security risks to your application and is equivalent to including untrusted code within your application." — JFrog

**Policy**: Only load SafeTensors format, reject pickle.

**Implementation**:
```python
# NEVER do this with untrusted models:
model = torch.load("model.pth")

# ALWAYS do this:
from safetensors.torch import load_file
model_state = load_file("model.safetensors")
model.load_state_dict(model_state)
```

### 3. Sandboxed Loading

> "If you intend to let users utilize untrusted AI models in your environment, it is extremely important to ensure that they are running in a sandboxed environment." — JFrog

**Implementation**:
- Load models in isolated container
- No network access during loading
- Minimal filesystem access
- Drop privileges after load

### 4. Dependency Pinning & Auditing

**Pin exact versions**:
```
# requirements.txt
torch==2.1.0
transformers==4.36.0
# NOT: torch>=2.0
```

**Audit regularly**:
- Check for known vulnerabilities
- Verify package integrity
- Review new dependencies

### 5. Signed Packages

Require cryptographic signatures:
- Verify publisher identity
- Ensure package not tampered
- Reject unsigned packages

### 6. Minimal Dependencies

Every dependency is attack surface. Minimize:
- Remove unused dependencies
- Choose well-maintained packages
- Prefer standard library when possible

### 7. Update Hygiene

**Controlled updates**:
- Don't auto-update blindly
- Review changelogs
- Test in isolated environment first
- Staged rollout

**Verification**:
- Signed updates only
- Hash verification
- Rollback capability

---

## ai-way Security Architecture

### Model Supply Chain

```
┌─────────────────────────────────────────────────────────┐
│                 ai-way Model Pipeline                    │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  1. Curated Model List                                  │
│     ├── Only pre-approved models                        │
│     ├── SafeTensors format required                     │
│     └── Pinned versions with hashes                     │
│                                                         │
│  2. Download Verification                               │
│     ├── HTTPS with certificate pinning                  │
│     ├── SHA-256 hash verification                       │
│     └── Signature verification (if available)           │
│                                                         │
│  3. Sandboxed Loading                                   │
│     ├── Isolated container environment                  │
│     ├── No network during load                          │
│     └── Minimal privileges                              │
│                                                         │
│  4. Runtime Isolation                                   │
│     ├── Model runs in separate process                  │
│     ├── Limited system access                           │
│     └── Monitored for anomalies                         │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Dependency Management

```
┌─────────────────────────────────────────────────────────┐
│              ai-way Dependency Policy                    │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  1. Pinned Versions                                     │
│     ├── Exact version specification                     │
│     ├── Hash verification in lockfile                   │
│     └── No floating version ranges                      │
│                                                         │
│  2. Vulnerability Scanning                              │
│     ├── Regular CVE database checks                     │
│     ├── Automated security alerts                       │
│     └── Rapid patching process                          │
│                                                         │
│  3. Minimal Surface                                     │
│     ├── Remove unused dependencies                      │
│     ├── Prefer standard library                         │
│     └── Audit transitive dependencies                   │
│                                                         │
│  4. Reproducible Builds                                 │
│     ├── Deterministic build process                     │
│     ├── Verifiable build outputs                        │
│     └── Signed releases                                 │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Update Process

```
┌─────────────────────────────────────────────────────────┐
│               ai-way Update Security                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  1. Update Discovery                                    │
│     ├── Check signed update manifest                    │
│     ├── Verify manifest signature                       │
│     └── Compare to current version                      │
│                                                         │
│  2. Download                                            │
│     ├── HTTPS with certificate pinning                  │
│     ├── Progress verification                           │
│     └── Resume capability                               │
│                                                         │
│  3. Verification                                        │
│     ├── SHA-256 hash check                              │
│     ├── Code signature verification                     │
│     └── Integrity validation                            │
│                                                         │
│  4. Installation                                        │
│     ├── Backup current version                          │
│     ├── Atomic installation                             │
│     └── Rollback on failure                             │
│                                                         │
│  5. Post-Install                                        │
│     ├── Integrity re-verification                       │
│     ├── Functionality testing                           │
│     └── Report status                                   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## Recommendations

### For ai-way Development

#### Must Have (Ship Blockers)

- [ ] **SafeTensors only**: No pickle model loading, ever
- [ ] **Hash verification**: SHA-256 for all models
- [ ] **Curated model list**: Pre-approved models only
- [ ] **Pinned dependencies**: Exact versions with hashes
- [ ] **Signed updates**: Cryptographic signatures required
- [ ] **Sandboxed loading**: Isolated model load process
- [ ] **Rollback capability**: Quick revert if issues found

#### Should Have

- [ ] **Reproducible builds**: Deterministic, verifiable
- [ ] **Vulnerability scanning**: Automated CVE checks
- [ ] **Certificate pinning**: For all network operations
- [ ] **Minimal dependencies**: Regular cleanup
- [ ] **Security testing**: Regular penetration testing

#### Could Have

- [ ] **Formal verification**: Mathematical proofs of security properties
- [ ] **Bug bounty program**: External security research
- [ ] **Threat intelligence**: Monitor for ai-way-specific attacks

### For AJ (User Guidance)

1. **Only use official ai-way models**: Don't load external models
2. **Keep ai-way updated**: Security patches are important
3. **Verify downloads**: Check that ai-way comes from official source
4. **Report anomalies**: Strange behavior might indicate compromise

### Incident Response

If supply chain compromise suspected:

1. **Isolate immediately**: Disconnect from network
2. **Preserve evidence**: Don't wipe system yet
3. **Check indicators**: Compare hashes, signatures
4. **Report**: Contact ai-way security team
5. **Rebuild**: Clean install from verified source

---

## Research Sources

- [JFrog Malicious Models Research](https://jfrog.com/blog/data-scientists-targeted-by-malicious-hugging-face-ml-models-with-silent-backdoor/)
- [Hugging Face 100 Malicious Models - Dark Reading](https://www.darkreading.com/application-security/hugging-face-ai-platform-100-malicious-code-execution-models)
- [CISO's Guide to AI Supply Chain - The Hacker News](https://thehackernews.com/2025/11/cisos-expert-guide-to-ai-supply-chain.html)
- [Backdoors in LLMs - Barracuda](https://blog.barracuda.com/2024/10/15/backdoors--supply-chain-attacks--and-other-threats-to-large-lang)
- [Hardware Trojans in ML - ACSAC 2024](https://mlsec.org/docs/2024-acsac.pdf)
- [Wiz/Hugging Face Security](https://www.wiz.io/blog/wiz-and-hugging-face-address-risks-to-ai-infrastructure)
- [AI Supply Chain Predictions - Extra Hop](https://www.extrahop.com/blog/amid-rising-genaI-hacking-hysteria-supply-chain-most-at-risk)

---

## Conclusion

### The Invisible Battlefield

Supply chain attacks are invisible until they succeed. AJ will never see the malicious code in a compromised model. They won't notice the backdoor in a dependency. The attack happens silently, completely.

### Our Responsibility

ai-way must be paranoid about supply chain:
- Trust nothing by default
- Verify everything possible
- Isolate what can't be verified
- Detect what can't be isolated
- Recover when detection fails

### The Ultimate Defense

Defense in depth:
1. **Prevent**: SafeTensors, verification, curation
2. **Detect**: Anomaly monitoring, integrity checks
3. **Contain**: Sandboxing, isolation, minimal privileges
4. **Recover**: Rollback, clean reinstall, incident response

### Final Thought

> "AJ trusts ai-way. ai-way must earn that trust by trusting nothing else. Every dependency is a liability. Every model is a risk. Every update is an attack surface. Build with paranoia."

---

**Document Version**: 1.0
**Last Updated**: 2025-12-19
**Author**: Security Research Team
**Classification**: Internal - For ai-way Development

---

*"The supply chain is only as strong as its weakest link. Find the links. Verify them all."*
