# Hermes-Agent MacBook Access Implementation Guide

## Overview
This guide covers implementing SSH-based access from Hermes-Agent (running in a Proxmox LXC) to your MacBook's `~/Developer` directory via TailScale.

---

## Prerequisites

- Hermes-Agent running in an LXC on Proxmox
- TailScale installed and configured on both LXC and MacBook
- MacBook with Remote Login (SSH) enabled

---

## Step-by-Step Implementation

### Step 1: Enable Remote Login on MacBook

1. Open **System Settings** → **General** → **Remote Login**
2. Toggle **Remote Login** to ON
3. Click "i" info button → Configure access
4. Add your TailScale user to allowed users

### Step 2: Configure TailScale SSH

On both LXC and MacBook, ensure TailScale SSH is enabled:

```bash
# Check current TailScale settings
tailscale status

# If needed, update /etc/tailscale/tailscaled.conf or use:
sudo tailscale up --ssh=true
```

### Step 3: Generate SSH Key for Agent

On the Hermes-Agent LXC:

```bash
# Generate dedicated key (no passphrase for automation)
ssh-keygen -t ed25519 -f ~/.ssh/hermes_macbook

# View public key to copy
cat ~/.ssh/hermes_macbook.pub
```

### Step 4: Configure Restricted Access on MacBook

On your MacBook, add the public key to `~/.ssh/authorized_keys` with directory restriction:

```bash
# Edit authorized_keys
nano ~/.ssh/authorized_keys

# Add the following line (replace with actual public key):
command="cd ~/Developer && /bin/bash" ssh-ed25519 AAAA... comment

# This forces:
# 1. Change to ~/Developer directory on connect
# 2. Drop to interactive shell
```

**Alternative: Use `authorized_keys` command restriction**

```bash
# For full Herms-Agent compatibility, use:
command="cd ~/Developer && /bin/bash -l" ssh-ed25519 AAAA... hermes-agent-key

# The -l flag makes it a login shell, loading profile
```

### Step 5: Configure Hermes-Agent

On the Hermes-Agent LXC, add to environment configuration:

```bash
# Option 1: Environment variables (~/.hermes/.env)
TERMINAL_SSH_HOST=<macbook-tailnet-ip>
TERMINAL_SSH_USER=<your-tailnet-user>
TERMINAL_SSH_KEY=~/.ssh/hermes_macbook
```

```bash
# Option 2: config.yaml
terminal:
  backend: ssh
  ssh_host: "<macbook-tailnet-ip>"
  ssh_user: "<your-tailnet-user>"
  ssh_key: "~/.ssh/hermes_macbook"
  persistent_shell: true
```

### Step 6: Find Your MacBook's TailScale IP

```bash
# From MacBook
tailscale ip -4

# Or from LXC
tailscale status
```

### Step 7: Test Connectivity

```bash
# Test SSH from LXC (using TailScale IP)
ssh -i ~/.ssh/hermes_macbook <user>@<macbook-tailnet-ip>

# Should drop you directly into ~/Developer
```

---

## Security Considerations

### Current Model (On-Demand)
- Agent accesses MacBook only when you're present
- Directory restricted via SSH forced command
- No internet-facing SSH ports

### SSH Key Restriction Details
The `command=` restriction in `authorized_keys` ensures:
- Agent starts in `~/Developer` directory
- Cannot access paths outside `~/Developer`
- Full shell access within that directory only

---

## Troubleshooting

### "Permission denied (publickey)"
1. Verify public key is in MacBook's `authorized_keys`
2. Check key permissions: `chmod 600 ~/.ssh/hermes_macbook`
3. Ensure MacBook has Remote Login enabled for your user

### "Connection refused"
1. Verify TailScale is running on both machines
2. Check `tailscale status` shows both machines
3. Ensure MacBook firewall allows SSH

### Agent can't access files
1. Verify directory exists: `ls -la ~/Developer`
2. Check SSH forced command is working: `pwd` after SSH connect
3. Verify file permissions on `~/Developer`

---

## Environment Variables Reference

| Variable | Description | Example |
|----------|-------------|---------|
| `TERMINAL_SSH_HOST` | Target SSH server | `100.x.x.x` |
| `TERMINAL_SSH_USER` | SSH username | `riley` |
| `TERMINAL_SSH_KEY` | Path to private key | `~/.ssh/hermes_macbook` |
| `TERMINAL_SSH_PORT` | SSH port (optional) | `22` |

---

## Alternative: OpenClaw (Future Consideration)

If you ever want to compare alternatives:
- OpenClaw supports Tailnet binding natively
- Better messaging integration (Discord, Telegram, WhatsApp)
- More complex setup for file operations
- Better suited if you want multi-platform agent access

Current recommendation: Stick with Hermes-Agent for this use case.

---

## Research Sources

- Context7 Documentation: /nousresearch/hermes-agent
- Hermes-Agent GitHub: Configuration & Environment Variables
- TailScale SSH Documentation

---
*Implementation guide created: April 2026*
