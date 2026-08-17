# New User Account Setup Guide

A guide for lab members to get access to the NVIDIA DGX Spark server.

---

## Overview

Each lab member gets their own account on the DGX server. Your account is isolated to your own home directory (`/home/your-username`). You will not have access to other users' files, and they will not have access to yours.

---

## Step 1 — Generate an SSH Key Pair

You will use SSH keys to log in securely without a password.

**Windows (PowerShell):**
```powershell
# Create a dedicated key directory
New-Item -ItemType Directory -Path "C:\ssh_keys" -Force

# Generate a 4096-bit RSA key pair
# Press ENTER for all prompts — leave the passphrase empty
ssh-keygen -t rsa -b 4096 -f "C:\ssh_keys\id_rsa"
```

**Mac / Linux:**
```bash
# Press ENTER for all prompts
ssh-keygen -t rsa -b 4096
# Key will be saved to ~/.ssh/id_rsa
```

---

## Step 2 — Send Your Public Key to the Admin

Copy your public key and send it to the lab admin (tjzhang).

**Windows:**
```powershell
type C:\ssh_keys\id_rsa.pub
```

**Mac / Linux:**
```bash
cat ~/.ssh/id_rsa.pub
```

The output will look like:
```
ssh-rsa AAAAB3NzaC1yc2EAAA... your-name@your-computer
```

Send this entire string to the admin. **Never share your private key (`id_rsa`).**

---

## Step 3 — Wait for Account Creation

The admin will:
1. Create your account on the DGX
2. Set up your SSH key
3. Send you your username and the server's Tailscale IP

---

## Step 4 — Connect to the Server

Once you receive your username and the server IP, install **Tailscale** first:

- Download: https://tailscale.com/download
- Log in with your Google or GitHub account
- Ask the admin to add you to the lab's Tailscale network

Then connect via SSH:

**Windows:**
```powershell
ssh -i "C:\ssh_keys\id_rsa" your-username@SERVER_IP
```

**Mac / Linux:**
```bash
ssh your-username@SERVER_IP
```

---

## Step 5 — Set Up VS Code Remote SSH (Recommended)

1. Install the **Remote - SSH** extension in VS Code
2. Press `Ctrl+Shift+P` → `Remote-SSH: Open SSH Configuration File`
3. Add the following entry:

```
Host DGX
    HostName SERVER_IP
    User your-username
    IdentityFile C:\ssh_keys\id_rsa        # Windows
    # IdentityFile ~/.ssh/id_rsa           # Mac / Linux
```

4. Press `Ctrl+Shift+P` → `Remote-SSH: Connect to Host` → select `DGX`

---

## Notes

- Your files live in `/home/your-username/` — you have full read/write access there
- You do **not** have `sudo` (admin) privileges by default
- If you need additional software installed, contact the admin
- Do not share your SSH private key with anyone
