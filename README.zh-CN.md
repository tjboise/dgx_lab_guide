**[English](README.md)** | **中文**

# NVIDIA DGX — 远程连接指南

通过 SSH 连接实验室 DGX 服务器的分步指南。

> 刚加入实验室?请先阅读[新用户账号申请指南](NEW_USER_SETUP.zh-CN.md),获取个人账号并配置好 SSH 密钥。

---

## 服务器信息

| 项目 | 内容 | 备注 |
| --- | --- | --- |
| 服务器 IP | `100.88.61.40` | Tailscale IP — 必须先连接实验室的 Tailscale 网络 |
| 端口 | `22` | 标准 SSH 端口 |
| 登录用户名 | `your-username` | 你的个人账号 — 如果还没有,请见[新用户账号申请指南](NEW_USER_SETUP.zh-CN.md) |
| 管理员联系方式 | tjzhang37@gmail.com | 账号申请与技术支持 |

---

## 第 1 步 — 生成 SSH 密钥

### Windows 用户

以**管理员身份打开 PowerShell**(右键 → 以管理员身份运行):

```powershell
# 创建专用的密钥目录
New-Item -ItemType Directory -Path "C:\ssh_keys" -Force

# 生成 ed25519 密钥对
# 所有提示直接按回车 — passphrase 留空
ssh-keygen -t ed25519 -f "C:\ssh_keys\id_ed25519"
```

### Mac / Linux 用户

```bash
# 所有提示直接按回车 — 密钥保存在 ~/.ssh/id_ed25519
ssh-keygen -t ed25519
```

---

## 第 2 步 — 连接 Tailscale

服务器只能通过实验室的 Tailscale 网络访问,公网和局域网都无法直接连接。

- 安装 Tailscale:https://tailscale.com/download
- 用你的 Google 或 GitHub 账号登录
- 请管理员把你加入实验室的 Tailscale 网络

---

## 第 3 步 — 测试连接

**Windows:**

```powershell
ssh -i "C:\ssh_keys\id_ed25519" your-username@100.88.61.40
```

**Mac / Linux:**

```bash
ssh your-username@100.88.61.40
```

> **首次连接提示:** 询问主机真实性时输入 `yes`。  
> 个人账号使用 SSH 密钥登录,不需要密码。  
> 登录后运行 `nvidia-smi` 检查 GPU 状态。

---

## ⚠️ 实验室守则

| 规则 | 说明 |
| --- | --- |
| **不要在宿主机上安装软件** | 不要在宿主机环境中运行 `pip install`。请使用 Miniconda 虚拟环境或 Docker 容器。 |
| **开始前检查 GPU** | 任何训练任务开始前先运行 `nvidia-smi`,确认资源空闲。 |
| **使用 tmux 或 screen** | 不要直接在裸 SSH 终端里跑长时间任务 — 网络一断进程就会被杀。请用 `tmux` 或 `screen` 让会话在后台保持运行。 |
