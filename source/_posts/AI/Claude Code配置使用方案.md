---
title: Claude的安装与使用
date: 2026-01-02
categories: AI
icon: AI
desc: Claude Code的安装与使用
---

Claude Code，Anthropic 的代理编码工具，它位于您的终端中，帮助您比以往任何时候都更快地将想法转化为代码。

大家都知道Claude Code在编程领域好用，但是呢。

众所周知，由于在国内各种原因，开通Claude账号、Claude付费很难。

如果你不在国外，最易上手小白方案： `Claude Code`  + `claude-code-router` + `cc-switch`

### 安装Claude Code

Claude 官网: [https://code.claude.com/docs/zh-CN/overview](https://code.claude.com/docs/zh-CN/overview)

```
# Mac/linux 
curl -fsSL https://claude.ai/install.sh | bash

# 使用HomeBrew
brew install --cask claude-code

# windows
irm https://claude.ai/install.ps1 | iex

# npm (注意：需要 Node.js 18+）
npm install -g @anthropic-ai/claude-code
```

### 安装claude-code-router

官方Github仓库: [https://github.com/musistudio/claude-code-router](https://github.com/musistudio/claude-code-router)

claude-code-router， 可将 Claude Code 请求路由到不同的模型，并自定义任何请求。

具体功能：
- 模型路由: 根据您的需求将请求路由到不同的模型（例如，后台任务、思考、长上下文）。
- 多提供商支持： 支持 OpenRouter、DeepSeek、Ollama、Gemini、Volcengine 和 SiliconFlow 等各种模型提供商。
- 请求/响应转换: 使用转换器为不同的提供商自定义请求和响应。

```
npm install -g @musistudio/claude-code-router

# 使用 Router 运行 Claude Code (这一步非常重要, 必须执行。
ccr code
```

### 安装cc-switch

[cc-switch官方仓库](https://github.com/db-link/cc-switch)，它是一款Claude Code / Codex / Gemini CLI 全方位辅助可视化界面工具。

下载参考Github仓库说明。
