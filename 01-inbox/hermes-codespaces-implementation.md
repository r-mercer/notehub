# GitHub Codespaces + Hermes-Agent Implementation Guide

## Deep Dive Analysis

### Overview

GitHub Codespaces provides cloud-based development environments with SSH access. This guide covers using it as a dev environment for Hermes-Agent.

---

## Pricing & Quotas (GitHub Enterprise)

### Enterprise Cloud Inclusion
- **Included with Enterprise** - check your organization settings for specific allocations
- Overage pricing: pay-per-minute for compute time
- **Machine types available:**
  - 2-core (4GB RAM)
  - 4-core (8GB RAM) 
  - 8-core (16GB RAM)

### Cost Estimation
| Machine Type | Price/minute | Hourly (est.) |
|--------------|--------------|---------------|
| 2-core | ~$0.08 | ~$4.80 |
| 4-core | ~$0.16 | ~$9.60 |
| 8-core | ~$0.32 | ~$19.20 |

*Check your Enterprise agreement for exact pricing*

---

## SSH Access Methods

### Method 1: GitHub CLI (Recommended)
```bash
# Install GitHub CLI
brew install gh

# Authenticate
gh auth login

# SSH into codespace
gh codespace ssh -c <codespace-name>
```

### Method 2: Direct SSH
```bash
# Get codespace SSH endpoint
gh codespace list

# Connect directly
ssh -T <codespace-id>@<endpoint>.users.github.com
```

### Method 3: Custom SSH (devcontainer.json)
```json
{
  "features": {
    "ghcr.io/devcontainers/features/sshd:1": {}
  }
}
```

---

## Implementation Steps

### Step 1: Verify Enterprise Access

```bash
# Check your Enterprise settings
gh enterprise view

# Or check org settings
gh api orgs/<your-org>/settings/codespaces
```

### Step 2: Install and Configure GitHub CLI (in Hermes-Agent LXC)

```bash
# Install gh
curl -fsSL https://cli.github.com/packages/github_cli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/github_cli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/github_cli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github_cli.list > /dev/null
sudo apt update && sudo apt install gh
```

### Step 3: Authenticate

```bash
# Interactive authentication
gh auth login

# Required scopes:
# - repo
# - codespace
# - read:org
```

### Step 4: Create Codespace

```bash
# Create codespace for a specific repo
gh codespace create --repo your-org/your-project --branch main

# Or specify machine type
gh codespace create --repo your-org/your-project --machine 4cores

# List existing codespaces
gh codespace list
```

### Step 5: Configure SSH Access for Agent

```bash
# Generate SSH key in Hermes-Agent LXC
ssh-keygen -t ed25519 -f ~/.ssh/hermes_codespaces

# Add to GitHub SSH keys
# Option A: Via web UI
# Settings → SSH and GPG keys → New SSH key

# Option B: Via gh CLI (if supported)
gh ssh-key add ~/.ssh/hermes_codespaces.pub
```

### Step 6: Configure Hermes-Agent

```yaml
# ~/.hermes/config.yaml
terminal:
  backend: ssh
  ssh_host: "vscode.github-abc123.users.github.com"
  ssh_user: "git"
  ssh_key: "~/.ssh/hermes_codespaces"
  persistent_shell: true
```

### Step 7: Test Connection

```bash
# Test SSH from LXC
ssh -i ~/.ssh/hermes_codespaces git@<codespace-host>

# Or use gh proxy
gh codespace ssh -c <codespace-name> --command "ls -la"
```

---

## Codespace Lifecycle Management

### Understanding Lifecycle
1. **Creating** - builds from devcontainer
2. **Running** - active development
3. **Stopped** - after 30 min inactivity (configurable)
4. **Deleted** - after retention period (max 30 days)

### Keeping Codespace Running
- Codespaces auto-stop after inactivity
- For agent use, consider:
  - Periodic "heartbeat" calls
  - Using prebuild configurations
  - API-based lifecycle management

### API Management

```bash
# Start codespace
gh api -X POST /user/codespaces/<codespace-name>/start

# Stop codespace
gh api -X POST /user/codespaces/<codespace-name>/stop

# Delete codespace
gh api -X DELETE /user/codespaces/<codespace-name>
```

---

## Hermes-Agent Integration Details

### Environment Variables

```bash
# Required for Hermes-Agent
TERMINAL_SSH_HOST=<codespace-endpoint>
TERMINAL_SSH_USER=git
TERMINAL_SSH_KEY=~/.ssh/hermes_codespaces

# Optional: Use gh as proxy
# gh codespace ssh handles auth automatically
```

### Configuration Example

```yaml
# ~/.hermes/config.yaml
terminal:
  backend: ssh
  ssh_host: "octocat-literate-space-parakeet-7gwrqp9q9jcx4vq.users.github.com"
  ssh_user: "git"
  ssh_key: "~/.ssh/hermes_codespaces"
  persistent_shell: true
  ssh_port: 22
```

---

## Limitations & Considerations

### Known Limitations

| Limitation | Impact | Mitigation |
|------------|--------|------------|
| Auto-stop after inactivity | Agent can't work 24/7 | Use API to keep alive or recreate |
| No root access | Some tools limited | Use prebuilt configs |
| Ephemeral storage | Data lost on stop | Use volumes or git sync |
| Network dependent | Needs internet | Not suitable for offline work |

### Codespace Retention
- Stopped codespaces retained up to **30 days**
- After deletion, environment is fresh each time
- **Strategy:** Use git workflow for persistence

### Recommended Workflow

```
1. Agent receives task via Discord
2. Create/start codespace (via API)
3. SSH in, clone relevant repos
4. Execute task
5. Push changes to git
6. Codespace auto-stops or manually stop
```

---

## Alternative: Using gh as Proxy

For simpler authentication, use GitHub CLI as SSH proxy:

```bash
# Configure gh to use SSH
gh config set git_protocol ssh

# Then in config:
# ssh_host: localhost
# Use gh codespace ssh as wrapper
```

---

## Testing Checklist

- [ ] GitHub CLI installed in LXC
- [ ] Authenticated with Enterprise account
- [ ] Codespace created and accessible
- [ ] SSH key generated and added
- [ ] SSH connection tested
- [ ] Hermes-Agent configured
- [ ] File operations tested
- [ ] Git operations working

---

## Troubleshooting

### "Permission denied (publickey)"
- Verify SSH key added to GitHub
- Check key permissions: `chmod 600 ~/.ssh/hermes_codespaces`

### "Codespace not found"
- Check codespace name: `gh codespace list`
- Ensure codespace is started: `gh api -X POST .../start`

### "Connection timeout"
- Codespace may be stopped
- Start via API: `gh api -X POST .../start`

### "gh: command not found"
- Install GitHub CLI for your OS
- Verify installation: `gh --version`

---

## Research Sources

- GitHub Docs: REST API for Codespaces
- GitHub CLI Manual: gh codespace
- Enterprise Cloud Billing Documentation
- Context7: GitHub documentation

---

*Implementation guide: April 2026*
