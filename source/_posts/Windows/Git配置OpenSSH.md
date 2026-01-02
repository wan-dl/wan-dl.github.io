---
title: Windows Git配置OpenSSH
date: 2026-01-02
categories: Windows
icon: Windows
---

### 安装openssh客户端

1. 用管理员权限打开 PowerShell/Windows Terminal：
2. 或者在开始菜单输入 “PowerShell” 或 “Windows Terminal”，右键选择 “以管理员身份运行”。

```
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

### 检查/启动 ssh-agent 并设置为开机自动启动

```
# 检查状态
Get-Service ssh-agent

# 启动服务
Start-Service ssh-agent

# 设置为开机自动启动
Set-Service -Name ssh-agent -StartupType Automatic
```

### 把你的私钥加入 agent

```
ssh-add $env:USERPROFILE\.ssh\id_ed25519
ssh-add -l   # 列出已加载的 key
```
