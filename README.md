# NVIDIA DGX — Remote Connection Guide

A step-by-step guide for connecting to the lab DGX server via **VS Code Remote - SSH**.

---

## Server Information

| Item | Detail | Note |
| --- | --- | --- |
| Server IP | `172.25.231.3` | Internal LAN |
| Port | `22` | Standard SSH |
| Shared account | `hwlab` | Ask admin for password · **Do NOT share outside lab** |

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

## Step 2 — Test the Connection

Verify the connection works before setting up VS Code.

**Windows:**

```powershell
ssh -i "C:\ssh_keys\id_rsa" hwlab@172.25.231.3
```

**Mac / Linux:**

```bash
ssh hwlab@172.25.231.3
```

> **First-time prompt:** Type `yes` when asked about host authenticity.  
> Enter the `hwlab` password — characters are hidden while typing, that's normal.  
> Once in, run `nvidia-smi` to verify GPU status.

---

## Step 3 — Configure VS Code

VS Code lets you edit remote files, use integrated terminals, and drag-and-drop uploads just like working locally.

### 3.1 Install the extension

In VS Code's Extensions panel, search for **Remote - SSH** (publisher: Microsoft) and install it.

### 3.2 Create an SSH config file

Create a plain text file at `C:\ssh_keys\vscode_config` (no extension) with the following content:

```
Host 172.25.231.3
    HostName 172.25.231.3
    User hwlab
    Port 22
    IdentityFile C:\ssh_keys\id_rsa
```

> **Mac/Linux:** Edit `~/.ssh/config` directly. Omit the `IdentityFile` line if you're using the default key path.

### 3.3 Link the config to VS Code

1. Open Settings (gear icon → Settings)
2. Search for `ssh config`
3. Set **Remote.SSH: Config File** to `C:\ssh_keys\vscode_config`

### 3.4 Connect

1. Open **Remote Explorer** (monitor icon in the sidebar)
2. Click the connect icon next to `172.25.231.3`
3. Choose **Linux** → Continue → enter the `hwlab` password

The bottom-left corner turns green: `SSH: 172.25.231.3` — you're connected.

---

## ⚠️ Lab Code of Conduct

| Rule | Details |
| --- | --- |
| **No host installs** | Never run `pip install` on the host environment. Use Miniconda virtual environments or Docker containers. |
| **Check GPU before starting** | Run `nvidia-smi` before any training job to confirm resources are free. |
| **Use tmux or screen** | Never run long jobs directly in the VS Code terminal — a network drop kills the process. Use `tmux` or `screen` to keep sessions alive in the background. |
