# AI Agent Development Setup Plan

## Overview
Enable Hermes-Agent (running in Proxmox LXC) to assist with coding tasks by accessing the MacBook's file system via TailScale SSH.

## Architecture

```
┌──────────────────┐      TailScale       ┌──────────────────┐
│  Hermes-Agent    │ ◄──────────────────► │      MacBook     │
│  (LXC on         │    (SSH via          │  (Dev Machine)   │
│   Proxmox)       │     TailScale)       │                  │
│                  │                      │  - VS Code       │
│  Env vars:       │                      │  - Git repos     │
│  TERMINAL_SSH_*  │                      │  - Dev tools     │
└──────────────────┘                      └──────────────────┘
```

## Research Summary

### Hermes-Agent Capabilities (Verified)
- **SSH Backend**: Native support via environment variables:
  ```
  TERMINAL_SSH_HOST=<macbook-tailnet-ip>
  TERMINAL_SSH_USER=<your-user>
  TERMINAL_SSH_KEY=~/.ssh/id_rsa
  ```
- Works with TailScale's SSH proxy (`ssh=true`)
- File tools (read, write, patch, search) work through the SSH backend
- Supports persistent shell sessions
- MIT Licensed, open-source (Nous Research)

### Alternative Agents Considered
- **OpenClaw**: More messaging-focused, complex setup for file access
- Conclusion: Hermes-Agent is the better fit for this use case

### Security Model
- Agent only accesses files on-demand (while user is present)
- SSH key authentication via TailScale
- No exposed ports to internet

## Implementation Steps

### Phase 1: MacBook Configuration
1. Enable Remote Login (SSH)
   - System Settings → General → Remote Login → Enable
   - Allow access for TailScale user

2. Configure TailScale SSH
   - Ensure `ssh` flag enabled in TailScale config

3. Create dedicated SSH key for agent
   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/hermes_macbook
   ```

### Phase 2: LXC Configuration
4. Add public key to MacBook
   - Append `~/.ssh/hermes_macbook.pub` to MacBook's `~/.ssh/authorized_keys`

5. Configure Hermes-Agent environment
   ```bash
   # Add to ~/.hermes/.env or configuration
   TERMINAL_SSH_HOST=<macbook-tailnet-ip>
   TERMINAL_SSH_USER=<your-tailnet-user>
   TERMINAL_SSH_KEY=~/.ssh/hermes_macbook
   
   # In config.yaml
   terminal:
     backend: ssh
   ```

### Phase 3: Testing
6. Test SSH connectivity from LXC to MacBook
7. Verify file operations through Hermes-Agent
8. Restrict access to specific directories

## Directory Access Restrictions
- Access limited to: `~/Developer`
- Will be implemented via SSH forced command restriction in `authorized_keys`

## Future Considerations
- Option to create standalone dev environment for agent
- Potential for background access when MacBook is awake
- Integration with existing Kubernetes workflows (future phase)

---
*Plan created: April 2026*
*Research conducted using Context7 docs and web search*
