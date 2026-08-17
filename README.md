# NVIDIA DGX — Remote Connection Guide

A step-by-step guide for connecting to the lab DGX server over SSH.

> New to the lab? See the [New User Account Setup Guide](NEW_USER_SETUP.md) for getting your own account and SSH key set up first.

---

## Server Information

| Item | Detail | Note |
| --- | --- | --- |
| Server IP | `100.88.61.40` | Tailscale IP — you must be connected to the lab's Tailscale network first |
| Port | `22` | Standard SSH |
| Login | `your-username` | Your personal account — see [New User Account Setup Guide](NEW_USER_SETUP.md) if you don't have one yet |
| Admin contact | tjzhang37@gmail.com | Account requests & support |

---

## Step 1 — Generate SSH Keys

### Windows users

Open **PowerShell as Administrator** (right-click → Run as Administrator):

```powershell
# Create a dedicated key directory
New-Item -ItemType Directory -Path "C:\ssh_keys" -Force

# Generate a 4096-bit RSA key pair
# Press ENTER for all prompts — leave the passphrase empty
ssh-keygen -t rsa -b 4096 -f "C:\ssh_keys\id_rsa"
```

### Mac / Linux users

```bash
# Press ENTER for all prompts — saved to ~/.ssh/id_rsa
ssh-keygen -t rsa -b 4096
```

---

## Step 2 — Connect to Tailscale

The server is only reachable over the lab's Tailscale network, not the open internet or LAN.

- Install Tailscale: https://tailscale.com/download
- Log in with your Google or GitHub account
- Ask the admin to add you to the lab's Tailscale network

---

## Step 3 — Test the Connection

**Windows:**

```powershell
ssh -i "C:\ssh_keys\id_rsa" your-username@100.88.61.40
```

**Mac / Linux:**

```bash
ssh your-username@100.88.61.40
```

> **First-time prompt:** Type `yes` when asked about host authenticity.  
> Personal accounts log in with your SSH key, no password needed.  
> Once in, run `nvidia-smi` to verify GPU status.

---

## ⚠️ Lab Code of Conduct

| Rule | Details |
| --- | --- |
| **No host installs** | Never run `pip install` on the host environment. Use Miniconda virtual environments or Docker containers. |
| **Check GPU before starting** | Run `nvidia-smi` before any training job to confirm resources are free. |
| **Use tmux or screen** | Never run long jobs directly in the VS Code terminal — a network drop kills the process. Use `tmux` or `screen` to keep sessions alive in the background. |
