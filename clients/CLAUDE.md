# Claude Code Client - Sandboxed Execution Guide

Run Claude Code with full autonomy inside a hardened jail. No permission prompts. OS-level containment.

**Philosophy**: Don't rely on Claude's permission system for security. Use OS-level isolation and let Claude run freely within that sandbox.

## Table of Contents

- [Quick Start](#quick-start)
- [Architecture Overview](#architecture-overview)
- [Fedora Silverblue Setup (Recommended)](#fedora-silverblue-setup-recommended)
- [Isolation Layers](#isolation-layers)
- [Claude Code Configuration](#claude-code-configuration)
- [Alternatives](#alternatives)
- [Troubleshooting](#troubleshooting)

---

## Quick Start

```bash
# 1. Create a project sandbox container
distrobox create --name claude-jail \
  --image registry.fedoraproject.org/fedora-toolbox:43 \
  --home ~/src/my-project

# 2. Enter the container
distrobox enter claude-jail

# 3. Install Claude Code
npm install -g @anthropic-ai/claude-code

# 4. Configure permissive mode with deny-list
mkdir -p ~/.claude
cat > ~/.claude/settings.json << 'EOF'
{
  "permissions": {
    "defaultMode": "bypassPermissions",
    "deny": [
      "Bash(sudo:*)",
      "Bash(rpm-ostree:*)",
      "Bash(pkexec:*)",
      "Bash(dbus-send:*)",
      "Bash(flatpak:*)",
      "Bash(podman:*)",
      "Bash(systemctl --user:*)"
    ]
  }
}
EOF

# 5. Run Claude with zero prompts
claude
```

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                     HOST (Fedora Silverblue)                     │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                    IMMUTABLE ROOT (/)                      │  │
│  │   ostree-managed, read-only, rpm-ostree for system pkgs   │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                    HOME (/var/home)                        │  │
│  │  ┌─────────────────────────────────────────────────────┐  │  │
│  │  │              PROJECT DIR (bind-mounted)              │  │  │
│  │  │  ┌─────────────────────────────────────────────┐    │  │  │
│  │  │  │         DISTROBOX CONTAINER (jail)          │    │  │  │
│  │  │  │  ┌─────────────────────────────────────┐    │    │  │  │
│  │  │  │  │          CLAUDE CODE                 │    │    │  │  │
│  │  │  │  │   - bypassPermissions mode           │    │    │  │  │
│  │  │  │  │   - Deny-list active                 │    │    │  │  │
│  │  │  │  │   - Full R/W within project          │    │    │  │  │
│  │  │  │  └─────────────────────────────────────┘    │    │  │  │
│  │  │  └─────────────────────────────────────────────┘    │  │  │
│  │  └─────────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                    SELINUX (enforcing)                     │  │
│  │            Mandatory Access Control layer                  │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### Security Layers

| Layer | Protection | Claude's Access |
|-------|-----------|-----------------|
| **ostree immutability** | System can't be modified | None (read-only /) |
| **Container isolation** | Process namespace, filesystem | Project dir only |
| **SELinux MAC** | Kernel-level policy | Confined by type |
| **Claude deny-list** | Blocks dangerous commands | Application layer |
| **Home dir structure** | Personal data outside project | Not bind-mounted |

---

## Fedora Silverblue Setup (Recommended)

### Prerequisites

```bash
# Verify you're on Silverblue
cat /etc/os-release | grep VARIANT_ID
# Should show: VARIANT_ID=silverblue

# Verify SELinux is enforcing
getenforce
# Should show: Enforcing

# Ensure distrobox is installed (comes with Silverblue)
distrobox --version
```

### Step 1: Create Project Directory Structure

Keep your projects isolated from personal data:

```bash
# Recommended structure
mkdir -p ~/src           # All Claude-accessible projects live here
mkdir -p ~/personal      # Personal files - NEVER mount to containers
mkdir -p ~/secrets       # SSH keys, tokens - NEVER mount to containers
```

### Step 2: Create a Jailed Container

**Option A: Per-project jail (most secure)**

```bash
# Create container with ONLY project dir as home
distrobox create --name project-claude \
  --image registry.fedoraproject.org/fedora-toolbox:43 \
  --home ~/src/my-project \
  --additional-packages "nodejs npm git"
```

**Option B: Shared src jail (more convenient)**

```bash
# Create container with entire src dir
distrobox create --name claude-dev \
  --image registry.fedoraproject.org/fedora-toolbox:43 \
  --home ~/src \
  --additional-packages "nodejs npm git docker-cli"
```

### Step 3: Install Claude Code Inside Container

```bash
distrobox enter claude-dev

# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Verify installation
claude --version
```

### Step 4: Configure Docker Access (Optional)

If Claude needs to run docker commands:

```bash
# On HOST: ensure podman socket is running
systemctl --user enable --now podman.socket

# Inside container: alias docker to podman via socket
echo 'export DOCKER_HOST=unix:///run/user/$(id -u)/podman/podman.sock' >> ~/.bashrc
```

### Step 5: Create Claude Configuration

Inside the container, create the settings file:

```bash
mkdir -p ~/.claude

cat > ~/.claude/settings.json << 'EOF'
{
  "permissions": {
    "defaultMode": "bypassPermissions",

    "deny": [
      "Bash(sudo:*)",
      "Bash(su:*)",
      "Bash(pkexec:*)",
      "Bash(rpm-ostree:*)",
      "Bash(ostree:*)",
      "Bash(flatpak:*)",
      "Bash(snap:*)",
      "Bash(dbus-send:*)",
      "Bash(gdbus:*)",
      "Bash(qdbus:*)",
      "Bash(systemctl:*)",
      "Bash(journalctl --vacuum:*)",
      "Bash(loginctl:*)",
      "Bash(hostnamectl:*)",
      "Bash(timedatectl:*)",
      "Bash(localectl:*)",
      "Bash(machinectl:*)",
      "Bash(podman:*)",
      "Bash(buildah:*)",
      "Bash(skopeo:*)",
      "Bash(toolbox:*)",
      "Bash(distrobox:*)",
      "Bash(chroot:*)",
      "Bash(unshare:*)",
      "Bash(nsenter:*)",
      "Bash(mount:*)",
      "Bash(umount:*)",
      "Bash(dd:*)",
      "Bash(mkfs:*)",
      "Bash(fdisk:*)",
      "Bash(parted:*)",
      "Bash(cryptsetup:*)",
      "Bash(chmod 777:*)",
      "Bash(chown root:*)",
      "Bash(curl|sh:*)",
      "Bash(curl|bash:*)",
      "Bash(wget -O-|sh:*)",
      "Bash(wget -O-|bash:*)",
      "Bash(eval:*)",
      "Bash(ssh-keygen -R:*)",
      "Bash(rm -rf /:*)",
      "Bash(rm -rf /*:*)",
      "Bash(rm -rf ~:*)",
      "Bash(> /dev/sd:*)",
      "Bash(iptables:*)",
      "Bash(nft:*)",
      "Bash(firewall-cmd:*)",
      "Bash(setenforce:*)",
      "Bash(chcon:*)",
      "Bash(restorecon:*)",
      "Bash(setsebool:*)"
    ],

    "allow": [
      "Bash(git:*)",
      "Bash(npm:*)",
      "Bash(npx:*)",
      "Bash(node:*)",
      "Bash(yarn:*)",
      "Bash(pnpm:*)",
      "Bash(python:*)",
      "Bash(python3:*)",
      "Bash(pip:*)",
      "Bash(pip3:*)",
      "Bash(poetry:*)",
      "Bash(uv:*)",
      "Bash(cargo:*)",
      "Bash(rustc:*)",
      "Bash(go:*)",
      "Bash(docker:*)",
      "Bash(docker-compose:*)",
      "Bash(make:*)",
      "Bash(cmake:*)",
      "Bash(gcc:*)",
      "Bash(g++:*)",
      "Bash(clang:*)",
      "Bash(javac:*)",
      "Bash(java:*)",
      "Bash(mvn:*)",
      "Bash(gradle:*)",
      "Bash(ls:*)",
      "Bash(find:*)",
      "Bash(grep:*)",
      "Bash(rg:*)",
      "Bash(fd:*)",
      "Bash(cat:*)",
      "Bash(head:*)",
      "Bash(tail:*)",
      "Bash(less:*)",
      "Bash(wc:*)",
      "Bash(sort:*)",
      "Bash(uniq:*)",
      "Bash(diff:*)",
      "Bash(patch:*)",
      "Bash(sed:*)",
      "Bash(awk:*)",
      "Bash(jq:*)",
      "Bash(yq:*)",
      "Bash(xq:*)",
      "Bash(curl:*)",
      "Bash(wget:*)",
      "Bash(http:*)",
      "Bash(httpie:*)",
      "Bash(gh:*)",
      "Bash(hub:*)",
      "Bash(glab:*)",
      "Bash(mkdir:*)",
      "Bash(cp:*)",
      "Bash(mv:*)",
      "Bash(rm:*)",
      "Bash(touch:*)",
      "Bash(ln:*)",
      "Bash(tar:*)",
      "Bash(gzip:*)",
      "Bash(gunzip:*)",
      "Bash(zip:*)",
      "Bash(unzip:*)",
      "Bash(7z:*)",
      "Bash(chmod:*)",
      "Bash(chown:*)",
      "Bash(ps:*)",
      "Bash(top:*)",
      "Bash(htop:*)",
      "Bash(kill:*)",
      "Bash(pkill:*)",
      "Bash(pgrep:*)",
      "Bash(lsof:*)",
      "Bash(netstat:*)",
      "Bash(ss:*)",
      "Bash(nc:*)",
      "Bash(ncat:*)",
      "Bash(socat:*)",
      "Bash(tcpdump:*)",
      "Bash(ping:*)",
      "Bash(traceroute:*)",
      "Bash(dig:*)",
      "Bash(nslookup:*)",
      "Bash(host:*)",
      "Bash(ssh:*)",
      "Bash(scp:*)",
      "Bash(rsync:*)",
      "Bash(sftp:*)",
      "Bash(openssl:*)",
      "Bash(gpg:*)",
      "Bash(base64:*)",
      "Bash(sha256sum:*)",
      "Bash(md5sum:*)",
      "Bash(date:*)",
      "Bash(cal:*)",
      "Bash(time:*)",
      "Bash(timeout:*)",
      "Bash(watch:*)",
      "Bash(tee:*)",
      "Bash(xargs:*)",
      "Bash(env:*)",
      "Bash(printenv:*)",
      "Bash(export:*)",
      "Bash(source:*)",
      "Bash(which:*)",
      "Bash(whereis:*)",
      "Bash(type:*)",
      "Bash(file:*)",
      "Bash(stat:*)",
      "Bash(du:*)",
      "Bash(df:*)",
      "Bash(free:*)",
      "Bash(uname:*)",
      "Bash(id:*)",
      "Bash(whoami:*)",
      "Bash(groups:*)",
      "Bash(echo:*)",
      "Bash(printf:*)",
      "Bash(seq:*)",
      "Bash(yes:*)",
      "Bash(true:*)",
      "Bash(false:*)",
      "Bash(test:*)",
      "Bash([:*)",
      "Bash([[:*)",
      "Bash(read:*)",
      "Bash(sleep:*)",
      "Bash(wait:*)",
      "Bash(jobs:*)",
      "Bash(bg:*)",
      "Bash(fg:*)",
      "Bash(nohup:*)",
      "Bash(screen:*)",
      "Bash(tmux:*)",
      "Bash(script:*)",
      "Bash(strace:*)",
      "Bash(ltrace:*)",
      "Bash(gdb:*)",
      "Bash(lldb:*)",
      "Bash(valgrind:*)",
      "Bash(perf:*)",
      "Bash(journalctl:*)",
      "Bash(dmesg:*)",
      "Bash(tree:*)",
      "Bash(exa:*)",
      "Bash(eza:*)",
      "Bash(bat:*)",
      "Bash(fzf:*)",
      "Bash(ag:*)",
      "Bash(ack:*)",
      "Bash(sqlite3:*)",
      "Bash(psql:*)",
      "Bash(mysql:*)",
      "Bash(mongosh:*)",
      "Bash(redis-cli:*)",
      "Bash(ollama:*)",
      "Bash(kubectl:*)",
      "Bash(helm:*)",
      "Bash(k9s:*)",
      "Bash(terraform:*)",
      "Bash(ansible:*)",
      "Bash(vagrant:*)",
      "Bash(packer:*)",
      "Bash(aws:*)",
      "Bash(az:*)",
      "Bash(gcloud:*)",
      "Bash(doctl:*)",
      "Bash(flyctl:*)",
      "Bash(vercel:*)",
      "Bash(netlify:*)",
      "Bash(wrangler:*)",
      "WebFetch",
      "WebSearch"
    ]
  }
}
EOF
```

---

## Isolation Layers

### Layer 1: Filesystem Isolation (Container)

The distrobox container provides:

| Isolation | Effect |
|-----------|--------|
| **Home bind-mount** | Only `~/src/project` is visible as `~` |
| **No access to real home** | `~/personal`, `~/.ssh`, etc. are invisible |
| **Separate /tmp** | Container has its own temp space |
| **Read-only host binaries** | Can use host tools but can't modify them |

**What Claude CAN'T access from the container:**
- `~/.ssh/` (SSH keys)
- `~/.gnupg/` (GPG keys)
- `~/.config/` (desktop app configs)
- `~/.local/share/` (application data)
- `~/Documents/`, `~/Downloads/` (personal files)
- Browser profiles, password managers, etc.

### Layer 2: Process Isolation (Namespaces)

Distrobox uses Linux namespaces:

```bash
# View namespace isolation
ls -la /proc/self/ns/

# Key namespaces:
# - mnt:  Separate mount table
# - pid:  Process IDs isolated
# - net:  Can be isolated (optional)
# - user: UID mapping
```

### Layer 3: Immutable System (ostree)

Silverblue's ostree provides:

```bash
# System is read-only by design
touch /usr/bin/test-write  # Permission denied

# Only rpm-ostree can modify system (blocked in Claude's deny-list)
rpm-ostree status

# Rollback is always available if something goes wrong
rpm-ostree rollback
```

### Layer 4: Mandatory Access Control (SELinux)

SELinux provides kernel-level enforcement:

```bash
# Check current mode
getenforce  # Should be "Enforcing"

# View container SELinux context
ps -eZ | grep -E "(distrobox|toolbox)"

# Check for denials
sudo ausearch -m avc -ts recent

# Common context for containers
# system_u:system_r:container_t:s0:c123,c456
```

### Layer 5: Claude Deny-List (Application)

Even if all other layers fail, Claude's deny-list prevents:

- Privilege escalation (`sudo`, `su`, `pkexec`)
- System modification (`rpm-ostree`, `flatpak`)
- Container escape (`nsenter`, `unshare`, `chroot`)
- Dangerous operations (`dd`, `mkfs`, `rm -rf /`)

---

## Claude Code Configuration

### Settings File Locations

| Scope | Path | Use Case |
|-------|------|----------|
| **User** | `~/.claude/settings.json` | Personal defaults |
| **Project** | `.claude/settings.json` | Team-shared settings |
| **Local** | `.claude/settings.local.json` | Personal overrides (gitignored) |

### Minimal Permissive Config

For maximum freedom with basic safety:

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions",
    "deny": [
      "Bash(sudo:*)",
      "Bash(rpm-ostree:*)"
    ]
  }
}
```

### Paranoid Config

For sensitive projects:

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions",
    "deny": [
      "Bash(sudo:*)",
      "Bash(su:*)",
      "Bash(pkexec:*)",
      "Bash(rpm-ostree:*)",
      "Bash(curl|sh:*)",
      "Bash(curl|bash:*)",
      "Bash(wget -O-|sh:*)",
      "Bash(rm -rf:*)",
      "Bash(chmod 777:*)",
      "Bash(ssh:*)",
      "Bash(scp:*)",
      "Bash(nc:*)",
      "Bash(ncat:*)",
      "WebFetch",
      "WebSearch"
    ]
  }
}
```

### Project-Specific Config

For a project at `~/src/my-project/.claude/settings.json`:

```json
{
  "permissions": {
    "defaultMode": "bypassPermissions",
    "allow": [
      "Bash(npm run:*)",
      "Bash(docker compose:*)"
    ],
    "deny": [
      "Bash(npm publish:*)",
      "Bash(docker push:*)"
    ]
  }
}
```

---

## Alternatives

### More Secure: Podman with Strict Isolation

For maximum security, use podman directly with explicit mounts:

```bash
# Create a minimal, ephemeral container
podman run -it --rm \
  --name claude-secure \
  --userns=keep-id \
  --security-opt=no-new-privileges \
  --cap-drop=ALL \
  --cap-add=CHOWN \
  --cap-add=DAC_OVERRIDE \
  --cap-add=FOWNER \
  --cap-add=SETGID \
  --cap-add=SETUID \
  --read-only \
  --tmpfs /tmp:rw,noexec,nosuid,size=1g \
  --tmpfs /home/user/.npm:rw,noexec,nosuid,size=500m \
  -v ~/src/my-project:/workspace:Z \
  -w /workspace \
  -e HOME=/home/user \
  registry.fedoraproject.org/fedora:43 \
  bash -c "
    useradd -m user &&
    dnf install -y nodejs npm git &&
    su - user -c 'npm install -g @anthropic-ai/claude-code && cd /workspace && claude'
  "
```

**Additional hardening:**

```bash
# Network isolation (no internet access)
podman run ... --network=none ...

# CPU/Memory limits
podman run ... --cpus=2 --memory=4g ...

# Seccomp profile
podman run ... --security-opt seccomp=/path/to/profile.json ...
```

### Simpler: Distrobox with Minimal Config

For ease of use with minimal security trade-off:

```bash
# Create standard distrobox
distrobox create --name claude-simple \
  --image registry.fedoraproject.org/fedora-toolbox:43

# Enter and install
distrobox enter claude-simple
npm install -g @anthropic-ai/claude-code

# Simple deny-list config
mkdir -p ~/.claude
echo '{
  "permissions": {
    "defaultMode": "bypassPermissions",
    "deny": ["Bash(sudo:*)", "Bash(rpm-ostree:*)"]
  }
}' > ~/.claude/settings.json
```

**Trade-offs:**
- Full home directory is accessible
- Less isolation than per-project containers
- Still protected by SELinux and ostree immutability

### Alternative OS: Alpine Linux (Minimal Attack Surface)

For paranoid setups, consider Alpine:

```bash
# Alpine-based container
distrobox create --name claude-alpine \
  --image alpine:3.19 \
  --home ~/src/my-project \
  --additional-packages "nodejs npm git"

# Alpine has:
# - Musl libc (smaller attack surface)
# - No systemd
# - Minimal base packages
# - Hardened by default
```

### Alternative OS: Qubes OS (Maximum Isolation)

For extreme security requirements:

- Run Claude in a disposable AppVM
- Network-isolated qube
- Separate qubes for different trust levels
- Hardware-level isolation via Xen hypervisor

---

## Troubleshooting

### Container Can't Access Docker

```bash
# On host: ensure podman socket exists
systemctl --user status podman.socket

# If not running:
systemctl --user enable --now podman.socket

# Inside container: verify socket access
ls -la /run/user/$(id -u)/podman/podman.sock
```

### SELinux Denials

```bash
# Check for recent denials
sudo ausearch -m avc -ts recent

# Generate policy module for specific denial
sudo ausearch -m avc -ts recent | audit2allow -M mypolicy
sudo semodule -i mypolicy.pp

# Temporary permissive (NOT recommended for production)
sudo setenforce 0
```

### Claude Deny-List Not Working

```bash
# Verify settings location
cat ~/.claude/settings.json

# Check for syntax errors
python3 -m json.tool ~/.claude/settings.json

# Deny rules use prefix matching, not regex
# "Bash(sudo:*)" matches "sudo anything"
# But won't match "echo | sudo" - be careful with pipes
```

### Container Home Directory Issues

```bash
# Verify home mount
distrobox enter claude-dev
echo $HOME
ls -la ~

# If wrong, recreate container with correct --home
distrobox rm claude-dev
distrobox create --name claude-dev --home ~/src ...
```

---

## Best Practices Summary

1. **Never mount real home** - Use `--home ~/src/project` to limit access
2. **Deny before allow** - Start with deny-list, add allows as needed
3. **Per-project containers** - More isolation, cleaner separation
4. **Keep SELinux enforcing** - It's your last line of defense
5. **Use ostree rollback** - If something goes wrong, rollback is instant
6. **Audit regularly** - Check `ausearch` for security events
7. **Update containers** - `distrobox upgrade` keeps tools current
8. **Separate secrets** - SSH keys, tokens stay outside containers

---

## See Also

- [AI-WAY Client](AI-WAY.md) - AI-WAY orchestration for curated agents
- [Docker Client](DOCKER.md) - Local inference with ollama/docker
- [Hypervisor Specialist](../specialists/hypervisor-specialist.md) - Virtualization expertise
- [Privacy Researcher](../research/privacy-researcher.md) - Agent fingerprinting research

---

**Maintained by**: 8007342
**Last Updated**: 2025-12-19
**Target OS**: Fedora Silverblue 43+
