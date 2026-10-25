**[English](NEW_USER_SETUP.md)** | **中文**

# 新用户账号申请指南

帮助实验室成员获取 NVIDIA DGX Spark 服务器访问权限的指南。

---

## 概述

每位实验室成员在 DGX 服务器上都有自己的账号。你的账号只能访问自己的主目录(`/home/your-username`),无法访问其他用户的文件,其他人也无法访问你的文件。

---

## 第 1 步 — 生成 SSH 密钥对

你将使用 SSH 密钥安全登录,无需密码。

**Windows (PowerShell — 以管理员身份打开,右键 → 以管理员身份运行):**
```powershell
# 创建专用的密钥目录
New-Item -ItemType Directory -Path "C:\ssh_keys" -Force

# 生成 ed25519 密钥对
# 所有提示直接按回车 — passphrase 留空
ssh-keygen -t ed25519 -f "C:\ssh_keys\id_ed25519"
```

**Mac / Linux:**
```bash
# 所有提示直接按回车
ssh-keygen -t ed25519
# 密钥将保存在 ~/.ssh/id_ed25519
```

---

## 第 2 步 — 把公钥发给管理员

复制你的公钥:

**Windows:**
```powershell
type C:\ssh_keys\id_ed25519.pub
```

**Mac / Linux:**
```bash
cat ~/.ssh/id_ed25519.pub
```

输出类似这样:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5... your-name@your-computer
```

请按以下格式发邮件给实验室管理员 **tjzhang37@gmail.com**:

```
To: tjzhang37@gmail.com
Subject: DGX Account Request — <你的全名>

Hi,

I'd like to request an account on the lab DGX server.

Name: <你的全名>
Preferred username: <例如:名字首字母 + 姓>
Lab / advisor: <所在实验室或导师>
Public key:
<在此粘贴 id_ed25519.pub 的完整内容>

Thanks,
<你的名字>
```

**切勿分享你的私钥(`id_ed25519`)。** 邮件里只能包含 `.pub` 文件的内容。

---

## 第 3 步 — 等待账号创建

管理员会:
1. 在 DGX 上创建你的账号
2. 配置你的 SSH 公钥
3. 把你的用户名和服务器的 Tailscale IP 发给你

---

## 第 4 步 — 连接服务器

收到用户名和服务器 IP 后,先安装 **Tailscale**:

- 下载:https://tailscale.com/download
- 用你的 Google 或 GitHub 账号登录
- 请管理员把你加入实验室的 Tailscale 网络

然后通过 SSH 连接:

**Windows:**
```powershell
ssh -i "C:\ssh_keys\id_ed25519" your-username@100.88.61.40
```

**Mac / Linux:**
```bash
ssh your-username@100.88.61.40
```

---

## 第 5 步 — 配置 VS Code Remote SSH(推荐)

1. 在 VS Code 中安装 **Remote - SSH** 扩展
2. 按 `Ctrl+Shift+P` → `Remote-SSH: Open SSH Configuration File`
3. 添加以下配置:

```
Host DGX
    HostName 100.88.61.40
    User your-username
    IdentityFile C:\ssh_keys\id_ed25519        # Windows
    # IdentityFile ~/.ssh/id_ed25519           # Mac / Linux
```

4. 按 `Ctrl+Shift+P` → `Remote-SSH: Connect to Host` → 选择 `DGX`

---

## 注意事项

- 你的文件位于 `/home/your-username/` — 你对该目录有完整的读写权限
- 默认**没有** `sudo`(管理员)权限
- 如果需要安装额外软件,请联系管理员
- 不要把你的 SSH 私钥分享给任何人
