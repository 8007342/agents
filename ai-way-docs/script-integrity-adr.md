# ADR: Script Integrity Verification

**Status**: Proposed
**Author**: Solutions Architect
**Date**: 2024-12
**Deciders**: Core team

## Context

Yollayah's bootstrap script (`yollayah.sh`) and its modules (`lib/*.sh`) are bash scripts that AJ clones from GitHub and runs locally. We need to consider whether and how to verify these scripts haven't been tampered with.

### The Question

> Should yollayah.sh verify its own integrity before running?

This ADR explores the threat model, available approaches, and recommends a path forward that aligns with our Constitution.

## Threat Model

### What We're Protecting Against

| Threat | Likelihood | Impact | Notes |
|--------|------------|--------|-------|
| Accidental corruption | Medium | Low | Disk errors, bad copy |
| Local tampering post-download | Low | High | Malware modifies scripts |
| Supply chain attack | Very Low | Critical | Compromised dependency |
| Repository compromise | Very Low | Critical | Attacker gains GitHub write access |
| MITM during clone | Extremely Low | Critical | Git uses TLS; requires advanced attacker |
| Nation-state targeting | Extremely Low | Critical | If this applies, integrity checks won't help |

### What We're NOT Protecting Against

- **Root-level compromise**: If attacker has root, game over
- **Sophisticated supply chain attacks**: Ollama, Python, system libraries
- **Physical access attacks**: Attacker can bypass any local check
- **Repository compromise**: Attacker modifies code AND verification

### Honest Assessment

Most integrity verification for bash scripts is **security theater** against sophisticated attackers. The real value is:

1. **Detecting accidental corruption** (disk errors, incomplete downloads)
2. **Catching unsophisticated tampering** (script kiddie modifies file)
3. **Providing audit trail** (proving what code ran)
4. **Building user trust** (signals we care about security)

## Approaches Evaluated

### 1. No Verification (Status Quo)

**How it works**: Trust git clone + GitHub TLS.

**Pros**:
- Zero complexity
- Zero maintenance burden
- Git already provides some integrity (SHA-1 object hashes)

**Cons**:
- No protection against post-download tampering
- No detection of corruption
- No audit trail

**Constitution alignment**: Violates Law of Care ("First, do no harm") - we should at least detect corruption.

### 2. SHA-256 Checksums

**How it works**:
- Store expected hashes in manifest file
- Verify at runtime: `sha256sum --check`

**Pros**:
- Simple, universal tooling
- Detects any modification
- Millisecond overhead
- No dependencies

**Cons**:
- Chicken-and-egg: if attacker modifies script, they modify hash too
- Maintenance burden (update hashes on every change)
- Provides false sense of security against sophisticated attacks

**Constitution alignment**: Law of Truth ("Be honest") - we must be clear this only catches accidental/unsophisticated tampering.

### 3. GPG Signatures

**How it works**:
- Developer signs releases with GPG key
- User verifies: `gpg --verify script.sh.sig`

**Pros**:
- Cryptographically proves authorship
- Survives repository compromise (attacker can't sign without key)
- Industry standard

**Cons**:
- GPG UX is notoriously hostile (AJ won't use it)
- Key management complexity (backup, revocation, password)
- Requires user action (AJ must verify manually)
- "Trusting trust" problem (how does AJ get our key safely?)

**Constitution alignment**: Law of Elevation ("Lift others higher") - expecting AJ to use GPG is unrealistic and patronizing.

### 4. Sigstore/Cosign (Modern Signing)

**How it works**:
- Keyless signing via GitHub identity
- Signatures recorded on Certificate Transparency log
- Verify: `cosign verify-blob`

**Pros**:
- No key management
- Better UX than GPG
- Audit trail on public ledger
- CI/CD friendly

**Cons**:
- Requires internet for verification (CT log check)
- Still requires user to run verification
- Newer technology (less familiar)
- Depends on cloud services

**Constitution alignment**: Better than GPG, but still requires user education that may not stick.

### 5. Environment Sanitization

**How it works**:
- Reset PATH to known-safe directories
- Unset LD_PRELOAD, LD_LIBRARY_PATH
- Reject suspicious execution contexts

```bash
export PATH="/usr/local/bin:/usr/bin:/bin"
unset LD_PRELOAD LD_LIBRARY_PATH
[[ "$0" == /tmp/* ]] && exit 1
```

**Pros**:
- Prevents common attack vectors
- Zero user interaction required
- Negligible overhead
- Doesn't require external verification

**Cons**:
- Doesn't verify script content itself
- Only prevents specific attack patterns

**Constitution alignment**: Law of Care - this is basic hygiene we should always do.

### 6. Hybrid Self-Verification

**How it works**:
- Scripts contain their own expected hash
- On startup, verify hash matches
- Offer "skip verification" for power users

```bash
INTEGRITY_CHECK="${INTEGRITY_CHECK:-enabled}"

if [[ "$INTEGRITY_CHECK" == "enabled" ]]; then
    verify_integrity || exit 1
fi
```

**Pros**:
- Catches post-download tampering
- No user action required (automatic)
- Power users (PJ) can disable
- Simple implementation

**Cons**:
- Chicken-and-egg still exists
- Requires hash updates on changes
- Can be disabled (attacker sets INTEGRITY_CHECK=disabled)

**Constitution alignment**:
- Law of Service ("Serve genuine interests") - automatic protection
- Four Protections ("AJ from ai-way") - can be disabled by informed user

## The Chicken-and-Egg Problem

All self-contained verification has this fundamental limitation:

> If an attacker can modify the script, they can modify the verification code.

**There is no purely local solution to this.** Real security requires:
1. Out-of-band hash verification (phone call, separate website)
2. Cryptographic signatures with pre-established trust
3. Hardware security modules (TPM, Secure Boot)

For ai-way's threat model (AJ running locally, not enterprise security), we accept this limitation and focus on:
- **Defense in depth**: Multiple layers, each catching different attacks
- **Honest communication**: Tell AJ what this does and doesn't protect against
- **Graceful degradation**: If verification fails, explain clearly

## Recommendation

### Implement Tiered Integrity Module

Create `lib/integrity/` module with three tiers:

```
lib/integrity/
├── README.md        # Honest documentation of limitations
├── init.sh          # Module entry point
├── environment.sh   # Tier 1: Environment sanitization (always on)
├── checksums.sh     # Tier 2: Self-verification (default on)
└── signing.sh       # Tier 3: Signature verification (opt-in)
```

#### Tier 1: Environment Sanitization (Always On)

```bash
# Always runs, cannot be disabled
export PATH="/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
unset LD_PRELOAD LD_LIBRARY_PATH PYTHONHOME
unset LD_AOUT_LIBRARY_PATH LD_DEBUG_OUTPUT

# Reject suspicious contexts
[[ "$0" == /tmp/* ]] && die "Refusing to run from /tmp"
[[ -w "$0" && ! -O "$0" ]] && die "Script is world-writable"
```

**Rationale**: Basic hygiene. No reason to ever skip this.

#### Tier 2: Self-Verification (Default On, Can Disable)

```bash
# Can be disabled by setting YOLLAYAH_SKIP_INTEGRITY=1
if [[ "${YOLLAYAH_SKIP_INTEGRITY:-}" != "1" ]]; then
    verify_module_hashes || {
        warn "Integrity check failed"
        warn "Set YOLLAYAH_SKIP_INTEGRITY=1 to bypass (not recommended)"
        exit 1
    }
fi
```

**Rationale**: Catches accidental corruption and unsophisticated tampering. Power users (PJ) can disable if they're modifying scripts intentionally.

#### Tier 3: Signature Verification (Opt-In)

```bash
# Only runs if explicitly requested
if [[ "${YOLLAYAH_VERIFY_SIGNATURES:-}" == "1" ]]; then
    if command -v cosign &>/dev/null; then
        verify_cosign_signatures || exit 1
    elif command -v gpg &>/dev/null; then
        verify_gpg_signatures || exit 1
    else
        warn "No signature verification tool found"
        warn "Install cosign or gpg for signature verification"
    fi
fi
```

**Rationale**: Available for paranoid users, but not required. We sign releases, document how to verify, but don't force it.

### Power User (PJ) Escape Hatch

The Constitution's Four Protections include "Protect the mission from corruption" - but also recognize that informed users should have control.

**PJ Definition**: A Power Joe is an AJ who:
- Understands what they're disabling
- Has a legitimate reason (developing, debugging, auditing)
- Accepts the consequences

**Implementation**:
```bash
# Environment variable to skip integrity checks
export YOLLAYAH_SKIP_INTEGRITY=1

# Or command-line flag
./yollayah.sh --no-integrity-check

# Both trigger a warning
warn "Integrity checks disabled by user request"
warn "This is not recommended for normal use"
```

**Documentation requirement**: README must explain:
- What integrity checks do
- What they don't protect against
- When it's appropriate to disable
- How to re-enable

## Implementation Plan

### Phase 1: Environment Sanitization
1. Add `lib/integrity/environment.sh`
2. Source at bootstrap before anything else
3. Always on, no disable option

### Phase 2: Self-Verification
1. Add `lib/integrity/checksums.sh`
2. Generate `.integrity` manifest with module hashes
3. Verify at startup
4. Add `YOLLAYAH_SKIP_INTEGRITY` escape hatch
5. Update hashes via `make integrity` or pre-commit hook

### Phase 3: Signing Infrastructure
1. Set up GPG key for releases
2. Configure GitHub Actions for Sigstore
3. Add `lib/integrity/signing.sh` (opt-in)
4. Document verification process in README

### Phase 4: Documentation
1. Create `lib/integrity/README.md` with honest threat model
2. Update main README with integrity section
3. Add to `dangers/` threat research

## Decision

**Accepted**: Implement Tiered Integrity Module as described above.

**Rationale**:
- Tier 1 (environment) is essential hygiene with zero downside
- Tier 2 (checksums) catches 90% of real-world issues with minimal overhead
- Tier 3 (signatures) available for those who want it
- PJ escape hatch respects user autonomy while defaulting to safety

## Consequences

### Positive
- Detects accidental corruption automatically
- Catches unsophisticated tampering
- Provides audit trail for debugging
- Builds user trust
- Respects power user autonomy

### Negative
- Maintenance burden (update hashes on changes)
- False sense of security if not communicated honestly
- Complexity in bootstrap sequence
- Edge cases with modified scripts (intentional forks)

### Risks
- Users may think this protects against all attacks (it doesn't)
- Hash updates may be forgotten (use pre-commit hooks)
- Environment sanitization may break edge cases (document exceptions)

## References

- [CONSTITUTION.md](../CONSTITUTION.md) - Law of Care, Law of Truth, Four Protections
- [dangers/](../dangers/) - Threat research
- [Sigstore Documentation](https://docs.sigstore.dev/)
- [GPG Signing Guide](https://www.gnupg.org/gph/en/manual/x135.html)
- [OWASP Supply Chain Security](https://owasp.org/www-project-software-supply-chain-security/)

---

## Appendix: Honest Limitations

### What This Protects Against

- Disk corruption (bit rot, bad sectors)
- Incomplete downloads
- Accidental modifications (editing wrong file)
- Unsophisticated malware (script kiddie replaces file)
- PATH/LD_PRELOAD injection attacks

### What This Does NOT Protect Against

- **Repository compromise**: If attacker has GitHub write access, they modify code AND hashes
- **Sophisticated supply chain attacks**: Compromised Ollama, Python, libraries
- **Physical access**: Attacker can bypass anything local
- **Root-level compromise**: Root wins, always
- **Key compromise**: If signing key is stolen, signatures are useless
- **Determined nation-state**: If they're targeting you specifically, integrity checks won't help

### The Honest Truth

> "We cannot prevent all attacks. We can make attacks more expensive and detect unsophisticated ones. Most importantly, we can be honest about what we're protecting and what we're not."

This aligns with our Constitution's Law of Truth: "Be honest, always. Admit uncertainty."
