# Gitpod + Hermes-Agent Implementation Guide

## Deep Dive Analysis

### Overview

Gitpod provides cloud-based developer environments with SSH access. Alternative to GitHub Codespaces with different pricing model and features.

---

## Pricing (2026)

| Plan | Price | Hours | Features |
|------|-------|-------|----------|
| **Free** | $0/mo | 50 hrs/mo | Community repo only |
| **Starter** | $9/mo | 100 hrs/mo | Private repos |
| **Professional** | $23/mo | Unlimited | Larger workspaces |
| **Unlimited** | $39/mo | Unlimited | Priority, 8+ cores |

### Cost Comparison

| Provider | Free Tier | Paid (Unlimited) |
|----------|-----------|------------------|
| GitHub Codespaces | Enterprise | ~$10-20/mo |
| Gitpod | 50 hrs | $23-39/mo |
| Hetzner VPS | N/A | ~€5/mo |

---

## SSH Access Methods

### Method 1: Gitpod CLI (Recommended)
```bash
# Install Gitpod CLI
npm install -g gitpod-cli

# Or use standalone
brew install gitpod-cli

# SSH into workspace
gitpod ssh <workspace-id>
```

### Method 2: Direct SSH
```bash
# Get workspace SSH endpoint
gitpod workspace list

# Connect directly
ssh -p <port> <workspace-id>@<region>.ssh.ws-eu45.gitpod.io
```

---

## Implementation Steps

### Step 1: Create Gitpod Account

```bash
# Login via GitHub (recommended)
gitpod login --host github.com
```

### Step 2: Install Gitpod CLI (in Hermes-Agent LXC)

```bash
# Option A: npm (if node installed)
npm install -g gitpod-cli

# Option B: Direct download
curl -L https://github.com/gitpod-io/gitpod/releases/latest/download/gitpod-cli-linux-amd64 -o /usr/local/bin/gitpod
chmod +x /usr/local/bin/gitpod
```

### Step 3: Authenticate

```bash
# Interactive login
gitpod login --host github.com

# Or use access token
export GITPOD_TOKEN=<your-token>
```

### Step 4: Create/Start Workspace

```bash
# Create workspace from repo
gitpod workspace create https://github.com/your-org/your-project

# Or use shorthand
gp workspace create your-org/your-project

# List workspaces
gitpod workspace list

# Start existing workspace
gitpod workspace start <workspace-id>
```

### Step 5: Configure SSH Access for Agent

```bash
# Generate SSH key in Hermes-Agent LXC
ssh-keygen -t ed25519 -f ~/.ssh/hermes_gitpod

# Add SSH key to Gitpod
# Via web UI: Gitpod Settings → SSH Keys → Add public key
```

### Step 6: Get Workspace SSH Details

```bash
# Get workspace connection info
gitpod workspace info <workspace-id>

# Output includes:
# - SSH host: <id>.ssh.ws-eu45.gitpod.io
# - SSH port: 22
# - User: gitpod
```

### Step 7: Configure Hermes-Agent

```yaml
# ~/.hermes/config.yaml
terminal:
  backend: ssh
  ssh_host: "<workspace-id>.ssh.ws-eu45.gitpod.io"
  ssh_user: "gitpod"
  ssh_key: "~/.ssh/hermes_gitpod"
  persistent_shell: true
```

---

## Workspace Lifecycle

### States
1. **Provisioning** - creating workspace
2. **Running** - active development
3. **Stopped** - after inactivity (configurable)
4. **Archived** - after retention period

### Retention
- **Free tier**: Workspaces archived after 14 days
- **Paid plans**: Up to 30 days retention
- **Strategy**: Use git for persistence

### Keeping Workspace Running
- Periodic activity keeps workspace alive
- Or use prebuilds for faster startup

---

## Gitpod vs Codespaces Comparison

| Factor | Gitpod | Codespaces |
|--------|--------|------------|
| Free tier | 50 hrs/mo | Enterprise included |
| SSH access | Native | Via gh CLI |
| Custom images | Yes | Limited |
| 24/7 workspace | Paid only | On-demand only |
| GitHub integration | Strong | Native |
| Self-hosted option | Yes | No |

### When to Choose Gitpod
- Need custom workspace images
- Want self-hosted option
- Better for long-running sessions (paid plans)
- More flexible environment config

### When to Choose Codespaces
- Already on GitHub Enterprise
- Prefer native integration
- Don't want additional cost

---

## Alternative: Gitpod Self-Hosted

If you want full control:

```bash
# Gitpod can be self-hosted on Kubernetes
# Requires:
# - Kubernetes cluster
# - Docker registry
# - Domain/DNS
```

Not recommended for your current setup (Proxmox already handles this).

---

## Hermes-Agent Integration Details

### Environment Variables

```bash
TERMINAL_SSH_HOST=<workspace-id>.ssh.ws-eu45.gitpod.io
TERMINAL_SSH_USER=gitpod
TERMINAL_SSH_KEY=~/.ssh/hermes_gitpod
```

### Full Configuration Example

```yaml
# ~/.hermes/config.yaml
terminal:
  backend: ssh
  ssh_host: "abc123def456.ssh.ws-eu45.gitpod.io"
  ssh_user: "gitpod"
  ssh_key: "~/.ssh/hermes_gitpod"
  persistent_shell: true
  ssh_port: 22
```

### Workspace Management Script

```bash
#!/bin/bash
# workspace-manager.sh

WORKSPACE_ID="abc123def456"

case "$1" in
  start)
    gitpod workspace start $WORKSPACE_ID
    ;;
  stop)
    gitpod workspace stop $WORKSPACE_ID
    ;;
  status)
    gitpod workspace info $WORKSPACE_ID
    ;;
esac
```

---

## Limitations

| Limitation | Impact | Mitigation |
|------------|--------|------------|
| Free tier time limit | 50 hrs/month | Paid plan for more |
| Auto-stop | Inactivity timeout | Periodic pings |
| Network required | Must be online | Not for offline |
| Port forwarding | Limited to app ports | Configure in .gitpod.yml |

---

## Testing Checklist

- [ ] Gitpod CLI installed in LXC
- [ ] Account created and authenticated
- [ ] Workspace created
- [ ] SSH key generated and added
- [ ] SSH connection tested
- [ ] Hermes-Agent configured
- [ ] File operations verified
- [ ] Git workflow tested

---

## Troubleshooting

### "gitpod: command not found"
```bash
# Install CLI
npm install -g gitpod-cli
# Or download directly
curl -L .../gitpod-cli-linux-amd64 -o /usr/local/bin/gitpod
```

### "Authentication failed"
```bash
# Re-authenticate
gitpod login --host github.com

# Or check token
echo $GITPOD_TOKEN
```

### "Workspace not found"
```bash
# List workspaces
gitpod workspace list

# Create new workspace
gitpod workspace create <repo-url>
```

### "Connection refused"
```bash
# Start workspace first
gitpod workspace start <workspace-id>
```

---

## Research Sources

- Gitpod Official Documentation
- Context7: /gitpod-io/gitpod
- Gitpod CLI Reference
- Pricing comparison sites (2026)

---

*Implementation guide: April 2026*
