---
title: macOS 使用LaunchAgent设置开机自动启动服务
date: 2026-02-06
categories: MacOSX
icon: apple
---

# macOS 开机自动启动服务配置文档

本文档介绍如何在 macOS 上使用 LaunchAgent 实现开机自动启动服务（uwsgi 和 nginx）。

---

## 目录

1. [基础知识](#基础知识)
2. [uwsgi 自动启动配置](#uwsgi-自动启动配置)
3. [nginx 自动启动配置](#nginx-自动启动配置)
4. [常用管理命令](#常用管理命令)
5. [故障排查](#故障排查)
6. [附录](#附录)

---

## 基础知识

### LaunchAgent vs LaunchDaemon

| 特性 | LaunchAgent | LaunchDaemon |
|------|-------------|--------------|
| **启动时机** | 用户登录后 | 系统启动时（无需登录） |
| **权限** | 用户权限 | root 权限 |
| **配置路径** | `~/Library/LaunchAgents/` | `/Library/LaunchDaemons/` |
| **适用场景** | 个人服务、开发环境 | 系统服务、生产环境 |

### plist 文件命名规范

采用**反向域名**格式：`com.公司名.项目名.服务名.plist`

例如：
- `com.apple.nginx.plist`
- `com.apple.hx.nginx.plist`

---

## nginx 自动启动配置

### 方案一：直接启动（推荐）

#### 1. 创建 LaunchAgent 配置文件

```bash
nano ~/Library/LaunchAgents/com.apple.hx.nginx.plist
```

**配置内容：**

```xml name=com.apple.hx.nginx.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.apple.hx.nginx</string>
    
    <key>ProgramArguments</key>
    <array>
        <string>/opt/homebrew/bin/nginx</string>
        <string>-g</string>
        <string>daemon off;</string>
    </array>
    
    <key>RunAtLoad</key>
    <true/>
    
    <key>KeepAlive</key>
    <true/>
    
    <key>StandardOutPath</key>
    <string>/opt/homebrew/var/log/nginx/launch.out.log</string>
    
    <key>StandardErrorPath</key>
    <string>/opt/homebrew/var/log/nginx/launch.err.log</string>
    
    <key>WorkingDirectory</key>
    <string>/opt/homebrew</string>
</dict>
</plist>
```

**重要说明：**
- **`-g "daemon off;"`**：让 nginx 在前台运行（LaunchAgent 要求）
- **`KeepAlive: true`**：nginx 崩溃时自动重启

---

#### 2. 加载和启动服务

```bash
# 确保日志目录存在
mkdir -p /opt/homebrew/var/log/nginx

# 设置文件权限
chmod 644 ~/Library/LaunchAgents/com.apple.hx.nginx.plist

# 验证 plist 格式
plutil -lint ~/Library/LaunchAgents/com.apple.hx.nginx.plist

# 加载服务
launchctl load ~/Library/LaunchAgents/com.apple.hx.nginx.plist

# 验证是否加载成功
launchctl list | grep com.apple.hx.nginx

# 查看进程
ps aux | grep nginx

# 测试访问
curl http://localhost
```

---

### 方案二：使用 shell 脚本启动

#### 1. 创建启动脚本

```bash
nano /opt/hx_analyze/server/start_nginx.sh
```

**脚本内容：**

```bash name=start_nginx.sh
#!/bin/bash

# 可选：延迟启动
# sleep 10

# 启动 nginx（前台运行）
exec /opt/homebrew/bin/nginx -g "daemon off;"
```

**设置执行权限：**

```bash
chmod +x /opt/hx_analyze/server/start_nginx.sh
```

---

### 方案二：使用 Homebrew Services（最简单）

如果 nginx 是通过 Homebrew 安装的：

```bash
# 启动并设置开机自启
brew services start nginx

# 查看所有服务状态
brew services list

# 停止服务
brew services stop nginx

# 重启服务
brew services restart nginx
```

Homebrew 会自动创建和管理 LaunchAgent，无需手动配置！

---

## 常用管理命令
---

### nginx 服务管理

```bash
# 启动服务
launchctl start com.apple.hx.nginx

# 停止服务
launchctl stop com.apple.hx.nginx

# 卸载服务
launchctl unload ~/Library/LaunchAgents/com.apple.hx.nginx.plist

# 重新加载服务
launchctl unload ~/Library/LaunchAgents/com.apple.hx.nginx.plist
launchctl load ~/Library/LaunchAgents/com.apple.hx.nginx.plist

# 查看服务状态
launchctl list | grep com.apple.hx.nginx

# 查看进程
ps aux | grep nginx

# 查看日志
tail -f /opt/homebrew/var/log/nginx/launch.err.log

# 测试 nginx 配置
/opt/homebrew/bin/nginx -t

# 重新加载 nginx 配置（不重启）
/opt/homebrew/bin/nginx -s reload
```

---

### macOS 新版命令（bootstrap/bootout）

macOS Big Sur 及更高版本推荐使用：

```bash
# 加载服务
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.apple.nginx.plist

# 卸载服务
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.apple.nginx.plist

# 查看服务详情
launchctl print gui/$(id -u)/com.apple.hx.analyze
```

---

## 故障排查

### 1. 验证 plist 文件格式

```bash
plutil -lint ~/Library/LaunchAgents/com.apple.nginx.plist
```

应该输出：`OK`

---

### 2. 检查文件权限

```bash
# 检查 plist 文件权限
ls -l ~/Library/LaunchAgents/com.apple.nginx.plist
# 应该是：-rw-r--r--

# 检查脚本权限
ls -l /opt/hx_analyze/server/start.sh
# 应该是：-rwxr-xr-x

# 修正权限
chmod 644 ~/Library/LaunchAgents/com.apple.nginx.plist
chmod +x /opt/hx_analyze/server/start.sh
```

---

### 3. 查看系统日志

```bash
# 查看 launchd 相关日志
log show --predicate 'subsystem == "com.apple.xpc.launchd"' --last 5m

# 查看特定服务日志
log show --predicate 'subsystem == "com.apple.xpc.launchd"' --last 5m | grep com.apple.xpc

# 实时监控系统日志
log stream --predicate 'subsystem == "com.apple.xpc.launchd"'
```

---

### 4. 查看服务日志

```bash
# nginx 日志
cat /opt/homebrew/var/log/nginx/launch.err.log
cat /opt/homebrew/var/log/nginx/error.log
cat /opt/homebrew/var/log/nginx/access.log
```

---

### 5. 手动测试脚本

在配置 LaunchAgent 之前，先手动测试脚本是否正常：

```bash
# 测试 nginx
/opt/homebrew/bin/nginx -t  # 测试配置
/opt/homebrew/bin/nginx -g "daemon off;"  # 前台运行测试
```

---

### 6. 常见错误及解决方案

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `Load failed: 5: Input/output error` | plist 格式错误或权限问题 | 1. 运行 `plutil -lint` 检查<br>2. 删除 `UserName` 字段（LaunchAgent 不需要）<br>3. 检查文件权限 |
| 服务加载成功但不运行 | 脚本路径错误或无执行权限 | 1. 检查路径是否正确<br>2. 运行 `chmod +x` 添加执行权限 |
| nginx 启动后立即退出 | 缺少 `daemon off;` 参数 | 在 ProgramArguments 中添加 `-g "daemon off;"` |
| 日志文件未生成 | 日志目录不存在 | 运行 `mkdir -p` 创建日志目录 |

---

## 附录

---

### B. plist 配置完整参考

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <!-- 服务唯一标识（必须） -->
    <key>Label</key>
    <string>com.apple.hx.example</string>
    
    <!-- 要执行的命令（必须） -->
    <key>ProgramArguments</key>
    <array>
        <string>/bin/bash</string>
        <string>/path/to/script.sh</string>
    </array>
    
    <!-- 加载时自动运行（开机自启） -->
    <key>RunAtLoad</key>
    <true/>
    
    <!-- 进程退出后自动重启 -->
    <key>KeepAlive</key>
    <true/>
    
    <!-- 标准输出日志 -->
    <key>StandardOutPath</key>
    <string>/path/to/out.log</string>
    
    <!-- 错误输出日志 -->
    <key>StandardErrorPath</key>
    <string>/path/to/err.log</string>
    
    <!-- 工作目录 -->
    <key>WorkingDirectory</key>
    <string>/path/to/working/dir</string>
    
    <!-- 环境变量 -->
    <key>EnvironmentVariables</key>
    <dict>
        <key>PATH</key>
        <string>/usr/local/bin:/usr/bin:/bin</string>
    </dict>
    
    <!-- 定时执行（每隔 N 秒） -->
    <!-- 注意：与 RunAtLoad 配合使用会导致重复执行 -->
    <!-- <key>StartInterval</key>
    <integer>3600</integer> -->
    
    <!-- 定时执行（指定时间） -->
    <!-- <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>2</integer>
        <key>Minute</key>
        <integer>30</integer>
    </dict> -->
</dict>
</plist>
```

---

### C. 文件和目录结构

```
/opt/hx_analyze/server/
├── start.sh              # uwsgi 启动脚本
├── start_nginx.sh        # nginx 启动脚本
├── uwsgi.ini            # uwsgi 配置文件
└── log/
    ├── analyze.log      # uwsgi 运行日志
    ├── launch.out.log   # LaunchAgent 标准输出
    ├── launch.err.log   # LaunchAgent 错误输出
    ├── nginx.out.log    # nginx 标准输出
    └── nginx.err.log    # nginx 错误输出

~/Library/LaunchAgents/
├── com.apple.nginx.plist   # uwsgi LaunchAgent
└── com.apple.hx.nginx.plist     # nginx LaunchAgent

/opt/homebrew/var/log/nginx/
├── access.log           # nginx 访问日志
├── error.log            # nginx 错误日志
├── launch.out.log       # LaunchAgent 标准输出
└── launch.err.log       # LaunchAgent 错误输出
```

---

## 总结

### 最佳实践

1. ✅ 先手动测试脚本，确保能正常运行
2. ✅ 使用 `plutil -lint` 验证 plist 格式
3. ✅ 查看日志文件排查问题
4. ✅ 在 LaunchAgent 中**不要**使用 `UserName` 字段
5. ✅ 重启电脑验证开机自启是否生效

---

**文档版本：** v1.0  
**最后更新：** 2026-02-06  
**适用系统：** macOS Big Sur 及更高版本