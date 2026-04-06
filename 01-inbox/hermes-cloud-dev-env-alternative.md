# Alternative Provider: Cloud Development Environments

## Overview

Instead of a Proxmox LXC, use a cloud-based development environment where Hermes-Agent SSHs in to execute tasks.

---

## Option 1: GitHub Codespaces (Recommended for Your Setup)

### Why This Option
- You're on **GitHub Enterprise** - Codespaces is included
- Native SSH access via `gh codespace ssh`
- No additional cost (uses your Enterprise allocation)
- Fully managed - no infrastructure to maintain

### Pricing (GitHub Enterprise)
- Included with Enterprise (check your specific plan for allocation)
- Overage: pay-per-minute for compute time
- See: [GitHub Pricing Calculator](https://github.com/pricing/calculator)

### SSH Access
```bash
# SSH into codespace via GitHub CLI
gh codespace ssh -c <codespace-name>

# Or use full SSH connection
ssh -T github-codespace-abc123
```

### Configuration for Hermes-Agent
```yaml
# Hermes-Agent config
terminal:
  backend: ssh
  ssh_host: "github-codespace-abc123.users.github.com"
  ssh_user: "git"
  ssh_key: "~/.ssh/hermes_codespaces"
  persistent_shell: true
```

### Setup Steps

1. **Create Codespace** (via GitHub UI or CLI)
   ```bash
   gh codespace create --repo your-org/your-repo
   ```

2. **Configure SSH Access**
   - Ensure SSH key is added to GitHub
   - Or configure custom SSH in devcontainer.json:
   ```json
   {
     "features": {
       "ghcr.io/devcontainers/features/sshd:1": {}
     }
   }
   ```

3. **Get Codespace Endpoint**
   ```bash
   gh codespace list
   ```

4. **Add SSH key to agent**
   ```bash
   # Generate key in Hermes-Agent LXC
   ssh-keygen -t ed25519 -f ~/.ssh/hermes_codespaces
   
   # Add public key to GitHub SSH keys
   ```

### Pros
- No additional cost (Enterprise included)
- Managed by GitHub
- Geographic redundancy
- No Proxmox resource usage

### Cons
- Requires internet connection
- Dependent on GitHub
- Compute time limits (check Enterprise plan)

---

## Option 2: Gitpod

### Pricing
| Plan | Price | Hours |
|------|-------|-------|
| Free | $0/mo | 50 hrs/mo |
| Starter | $9/mo | 100 hrs/mo |
| Professional | $23/mo | Unlimited |

### SSH Access
```bash
# SSH into Gitpod workspace
gp ssh workspace
```

### Configuration for Hermes-Agent
```yaml
terminal:
  backend: ssh
  ssh_host: "<workspace-id>.ssh.ws-eu45.gitpod.io"
  ssh_user: "gitpod"
  ssh_key: "~/.ssh/hermes_gitpod"
```

### Pros
- Good free tier (50 hrs/mo)
- More flexible than Codespaces
- Customizable dev environments

### Cons
- Additional cost beyond free tier
- Less integrated with GitHub workflow

---

## Option 3: Hetzner Cloud / VPS (Self-Hosted Alternative)

### Pricing
- From ~€5/month for adequate specs
- Full control over environment

### Use Case
- If you want self-hosted but not on Proxmox
- More traditional VPS approach

### Pros
- Full control
- Cheapest for 24/7
- No vendor lock-in

### Cons
- More setup/maintenance
- Need to manage security

---

## Comparison Matrix

| Factor | Codespaces | Gitpod | VPS |
|--------|------------|--------|-----|
| Cost | Free (Enterprise) | $0-23/mo | ~$5-20/mo |
| SSH Access | Yes | Yes | Yes |
| Setup Complexity | Low | Low | Medium |
| 24/7 Availability | No (on-demand) | With paid plan | Yes |
| Maintenance | None | None | Self-managed |
| Geographic Redundancy | Yes | Yes | No |

---

## Recommendation for Your Setup

**Primary: GitHub Codespaces**
- Best cost (included in Enterprise)
- Good SSH support
- Integrated with your workflow

**Secondary: Gitpod**
- If you need more hours or flexibility

**Fallback: VPS**
- If you need 24/7 always-on

---

## Implementation for Hermes-Agent

### Step 1: Create Cloud Dev Environment

**GitHub Codespaces:**
```bash
# Install GitHub CLI if not present
brew install gh

# Authenticate
gh auth login

# Create codespace
gh codespace create --repo your-org/project
```

**Gitpod:**
```bash
# Install Gitpod CLI
brew install gitpod-cli

# Or use browser extension
```

### Step 2: Configure SSH Access

1. Generate SSH key in Hermes-Agent LXC
2. Add to GitHub/Gitpod SSH keys
3. Test connection

### Step 3: Update Hermes-Agent Config

```yaml
# ~/.hermes/config.yaml
terminal:
  backend: ssh
  ssh_host: "<endpoint>"
  ssh_user: "<user>"
  ssh_key: "~/.ssh/hermes_dev_env"
```

### Step 4: Test and Verify

---

## Research Sources

- GitHub Docs: Codespaces SSH access
- GitHub Enterprise billing documentation
- Gitpod pricing (2026)
- Context7: GitHub documentation

---

*Created: April 2026*
