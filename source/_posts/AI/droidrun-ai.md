---
title: Droidrun 操作Android手机
date: 2025-04-19
categories: AI
icon: AI
desc: 人工智能和ChatGPT
---

DroidRun 是一个强大的框架，用于通过 LLM 代理控制 Android 设备。它允许您使用自然语言命令自动化 Android 设备交互。

### 能做什么？

- 使用自然语言命令控制 Android 设备
- 支持多个 LLM 提供商（OpenAI、Anthropic、Gemini）
- CLI
- 用于自定义自动化的可扩展 Python API
- 屏幕截图分析，直观了解设备

### 官方资源

- droidrun官网地址: [https://www.droidrun.ai/](https://www.droidrun.ai/)
- droidrun-portal: [https://github.com/droidrun/droidrun-portal](https://github.com/droidrun/droidrun-portal)
- Github地址: [https://github.com/droidrun/droidrun](https://github.com/droidrun/droidrun)

### 使用前提

1. 电脑本机已安装并配置了 ADB
2. 在安卓设备上已安装DroidRun Portal app
3. 一个受支持的大型语言模型提供商的 API 密钥
    - OpenAI
    - Anthropic
    - Google Gemini
    
### 安装

整个环境使用python来运行。

```python
pip install droidrun
```

其它安装配置参考: [https://github.com/droidrun/droidrun](https://github.com/droidrun/droidrun)