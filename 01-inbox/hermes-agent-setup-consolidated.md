# Hermes-Agent Development Setup
## Consolidated Research, Findings & Implementation Plans

**Created:** April 2026  
**Status:** Ready for implementation  
**Implementation:** Deferred to future date

---

## Executive Summary

This document outlines two approaches to enable Hermes-Agent to assist with coding tasks:

1. **Approach A**: Agent SSHs into MacBook (simpler, on-demand only)
2. **Approach B**: Dedicated dev environment LXC on Proxmox (24/7 capability)

Both approaches use Hermes-Agent running in an LXC on Proxmox, connected via Discord for instructions.

---

## Research Findings

### Hermes-Agent Capabilities (Verified)

- **SSH Backend**: Native support via environment variables (`TERMINAL_SSH_HOST`, `TERMINAL_SSH_USER`, `TERMINAL_SSH_KEY`)
- Works with TailScale's SSH proxy (`ssh=true`)
- File tools (read, write, patch, search) work through SSH backend
- Supports persistent shell sessions
- MIT Licensed, open-source (Nous Research)

### Alternative Agents Evaluated

| Agent | Verdict |
|-------|---------|
| OpenClaw | More messaging-focused, complex setup for file ops - not recommended for this use case |
| Other OSS agents | Less mature or overkill for requirements |

### Hardware Requirements

| Environment | Memory | CPU | Disk |
|-------------|--------|-----|------|
| Hermes-Agent base | 1 GB (min), 2-4 GB (recommended) | 1-2 cores | 10 GB |
| Dev environment | 4 GB (min), 8 GB (recommended) | 2-4 cores | 50-100 GB |

---

## Approach A: MacBook SSH Access

### Architecture

```
┌──────────────────┐      TailScale       ┌──────────────────┐
│  Hermes-Agent    │ ◄──────────────────► │      MacBook     │
│  (LXC on         │    (SSH via          │  (Dev Machine)   │
│   Proxmox)       │     TailScale)       │                  │
│                  │                      │  - VS Code       │
│  Discord command │                      │  - Git repos     │
└──────────────────┘                      │  - K8s cluster   │
                                           └──────────────────┘
```

### How It Works

1. User sends instruction via Discord
2. Hermes-Agent (LXC) receives message
3. Agent SSHs into MacBook via TailScale
4. Executes task in `~/Developer` directory
5. Reports results via Discord

### Directory Access
- Restricted to `~/Developer` only
- Enforced via SSH forced command in `authorized_keys`

### Prerequisites

- [ ] TailScale installed on both LXC and MacBook
- [ ] MacBook Remote Login enabled
- [ ] TailScale SSH enabled (`ssh=true`)

### Implementation Steps

1. **Enable Remote Login on MacBook**
   - System Settings → General → Remote Login → Enable

2. **Configure TailScale SSH**
   - Ensure `ssh` flag enabled on both machines

3. **Generate SSH Key**
   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/hermes_macbook
   ```

4. **Add Public Key to MacBook** (with restriction)
   ```bash
   # In ~/.ssh/authorized_keys on MacBook:
   command="cd ~/Developer && /bin/bash -l" ssh-ed25519 AAAA... hermes-agent-key
   ```

5. **Configure Hermes-Agent**
   ```yaml
   # ~/.hermes/config.yaml
   terminal:
     backend: ssh
     ssh_host: "<macbook-tailnet-ip>"
     ssh_user: "<tailnet-user>"
     ssh_key: "~/.ssh/hermes_macbook"
     persistent_shell: true
   ```

6. **Test Connectivity**

### Pros
- Uses existing dev environment
- No environment duplication
- Simpler setup

### Cons
- MacBook must be awake
- Access only when user is present
- Directory restricted

---

## Approach B: Dedicated Dev Environment (Multiple Options)

### Option B1: Proxmox LXC
- See `hermes-dedicated-dev-env-alternative.md`

### Option B2: GitHub Codespaces (Recommended for Enterprise)
- See `hermes-codespaces-implementation.md`

### Option B3: Gitpod
- See `hermes-gitpod-implementation.md`

### Option B4: Self-hosted VPS (Hetzner, etc.)
- Traditional VPS approach

### Architecture

```
┌──────────────────┐      ┌────────────────────┐
│  Hermes-Agent    │ ───► │  Dev Environment   │
│  (LXC on Proxmox)│      │  (Dedicated LXC)   │
│                  │      │                    │
│  Discord command │      │  - git, node, etc. │
│                  │      │  - Full filesystem│
│                  │      │  - K8s cluster     │
└──────────────────┘      └────────────────────┘
```

### How It Works

1. User sends instruction via Discord
2. Hermes-Agent receives message
3. Executes in its own LXC environment
4. Reports results via Discord

### Implementation Options

#### Option B1: Additional LXC (Recommended)

1. Create new LXC (2 CPU, 4GB RAM, 50GB disk)
2. Install dev tools (git, node, python, etc.)
3. Clone projects to `/workspace`
4. Configure Hermes-Agent Docker backend

#### Option B2: Hermes-Agent Docker Backend

```yaml
# ~/.hermes/config.yaml
terminal:
  backend: docker
  docker_image: "nikolaik/python-nodejs:python3.11-nodejs20"
  container_cpu: 2
  container_memory: 4096
  container_disk: 51200
  container_persistent: true
```

### Sync Strategy

Since agent has its own environment:
- Work in `/workspace` within LXC
- Push changes via git to your repos
- Or use TailScale to mount/access files from MacBook

### Pros
- 24/7 availability
- Independent from MacBook
- Full filesystem access
- Can rebuild K8s cluster in this environment

### Cons
- Duplicates dev environment
- Requires git workflow for sync
- Additional resource usage on Proxmox

---

## Comparison Matrix

| Factor | Approach A (MacBook) | Approach B (Dedicated LXC) |
|--------|----------------------|---------------------------|
| 24/7 availability | No | Yes |
| MacBook dependency | Required | Not needed |
| Your workflow impact | Must be present | None |
| Directory access | ~/Developer only | Full |
| Kubernetes access | Uses your existing config | Rebuild cluster in LXC |
| Resource usage | Uses MacBook | Uses Proxmox |
| Implementation complexity | Lower | Higher |

---

## Decision Factors

### Choose Approach A If:
- You prefer simplicity
- Agent only needs to work when you're present
- Want to leverage existing MacBook dev environment
- Don't need 24/7 agent availability

### Choose Approach B If:
- Need 24/7 agent capability
- Want independent K8s environment for agent
- Willing to manage git sync workflow
- Have Proxmox resources to spare

---

## Communication Channel

- **Platform**: Discord
- Agent receives instructions and reports back via Discord

---

## Future Considerations

- Access to container registries
- Integration with specific APIs
- Additional messaging platforms

---

## Reference Files

| File | Contents |
|------|----------|
| `ai-agent-macbook-access-plan.md` | Original plan overview |
| `hermes-macbook-access-implementation.md` | Detailed SSH implementation |
| `hermes-dedicated-dev-env-alternative.md` | Dedicated LXC alternative |
| `hermes-cloud-dev-env-alternative.md` | Cloud dev env overview |
| `hermes-codespaces-implementation.md` | GitHub Codespaces detailed guide |
| `hermes-gitpod-implementation.md` | Gitpod detailed guide |

---

## Implementation Priority

When ready to implement:

1. **Start with Approach A** (simpler, ~30 mins)
2. Evaluate if 24/7 needed
3. If yes, implement Approach B

---

*Consolidated document: April 2026*
*Research sources: Context7 docs, web search, Hermes-Agent GitHub*
