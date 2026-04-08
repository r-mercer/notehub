# Alternative: Dedicated Developer Environment for Hermes-Agent

## Overview

Instead of accessing your MacBook via SSH, run a dedicated dev environment (LXC) on Proxmox where Hermes-Agent can work independently. This provides 24/7 agent capability without depending on your MacBook.

---

## Architecture Comparison

### Current Plan (MacBook Access)
```
┌────────────────────┐      TailScale       ┌────────────────┐
│  Hermes-Agent      │ ◄──────────────────► │     MacBook    │
│  (LXC on Proxmox) │      SSH             │ ~/Developer    │
│                    │                      │ (restricted)   │
└────────────────────┘                      └────────────────┘
- MacBook must be awake
- Access to ~/Developer only
- You must be present
```

### Alternative (Dedicated Dev Environment)
```
┌────────────────────┐      ┌────────────────────┐
│  Hermes-Agent     │ ───► │  Dev Environment   │
│  (LXC on Proxmox) │      │  (Dedicated LXC)   │
│                    │      │                    │
│  Discord command  │      │  Full filesystem   │
└────────────────────┘      │  24/7 availability  │
                            └────────────────────┘
- Agent runs independently
- Full dev capabilities
- Sync changes via git
```

---

## Hardware Requirements (Hermes-Agent)

Based on research from official docs and community guides:

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| **Memory** | 1 GB | 2-4 GB |
| **CPU** | 1 core | 2 cores |
| **Disk** | 10 GB | 50 GB+ (for code/projects) |

**Note:** Browser automation (Playwright/Chromium) requires **at least 2 GB** memory.

### For Dev Environment (Full Development Stack)

Add requirements for running code, tests, builds:

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| **Memory** | 4 GB | 8 GB |
| **CPU** | 2 cores | 4 cores |
| **Disk** | 50 GB | 100 GB+ |

---

## Implementation Options

### Option A: Additional LXC (Recommended for Your Setup)

Since you already run LXCs on Proxmox:

1. Create new LXC with Ubuntu/Debian
2. Install dev tools (git, node, python, etc.)
3. Configure Hermes-Agent to use local Docker backend
4. Sync work via git or TailScale file sharing

**Pros:**
- Uses existing Proxmox infrastructure
- Lightweight compared to full VM
- Easy to provision

**Cons:**
- Shares resources with other LXCs

### Option B: Docker Container (Within Existing LXC)

Run Hermes-Agent's built-in Docker backend:

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

**Pros:**
- Already supported by Hermes-Agent
- Isolated filesystem
- Resource limits configurable

**Cons:**
- Requires Docker in LXC
- Less flexible than full LXC

### Option C: Separate VM

Full virtual machine for agent:

**Pros:**
- Complete isolation
- Can run full desktop env if needed
- Independent resource scheduling

**Cons:**
- Higher resource overhead
- More complex setup

---

## Recommended Approach for Your Setup

### Step 1: Create Dev LXC on Proxmox

```bash
# Create LXC (example parameters)
pct create 102 local:vztmpl/ubuntu-22.04.tar.gz \
  --cores 2 \
  --memory 4096 \
  --rootfs local:50 \
  --hostname hermes-dev-env \
  --net0 name=eth0,bridge=vmbr0,ip=dhcp
```

### Step 2: Install Development Tools

```bash
# Update and install essentials
apt update && apt upgrade -y
apt install -y git curl wget build-essential
apt install -y python3 python3-pip nodejs npm
# Add tools specific to your workflow
```

### Step 3: Configure Hermes-Agent Docker Backend

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

### Step 4: Set Up Git Sync

```bash
# In dev environment, clone your projects
cd /workspace
git clone https://github.com/yourusername/your-project.git

# Workflow:
# 1. Agent works in /workspace/your-project
# 2. You pull changes to MacBook
# 3. Or use TailScale to mount/access files
```

---

## Comparison Summary

| Factor | MacBook Access | Dedicated LXC |
|--------|----------------|---------------|
| 24/7 availability | No | Yes |
| MacBook dependency | Required | Not needed |
| Your workflow impact | Must be present | None |
| Directory access | ~/Developer only | Full |
| Dev environment | Your MacBook | New environment |
| Resource usage | Uses MacBook resources | Uses Proxmox resources |
| Sync mechanism | Direct file access | Git or TailScale |

---

## Research Sources

- Hermes-Agent official docs: Resource limits (2-4GB memory, 2 CPUs recommended)
- Proxmox LXC guides for AI agents
- Community tutorials on running Hermes on VPS/containers
- Context7: /nousresearch/hermes-agent

---

## Decision Questions

Before implementing, consider:

1. **Do you need 24/7 agent availability?** If yes, dedicated environment recommended.
2. **Should the agent work on work projects?** May need separate environment/security controls.
3. **How will you sync changes?** Git workflow vs TailScale file sharing.
4. **What dev tools does the agent need?** Affects LXC provisioning.

---
*Created: April 2026*
*Research conducted for alternative implementation planning*
