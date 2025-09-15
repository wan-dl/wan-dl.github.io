---
title: 使用undici通过ProxyAgent设置Node网络请求代理
date: 2025-09-15
categories: NodeJS
desc: 日常使用笔记
icon: node
---

undici 支持通过ProxyAgent设置Node网络请求代理。

## 1. 安装 Undici

```bash
npm install undici
```

## 2. 基本用法

Undici 提供了专门的类：`ProxyAgent`。你可以通过它让 undici 的所有请求经过你的代理服务器。

**示例：HTTP 代理**

```javascript
const { fetch, ProxyAgent } = require('undici');

// 假设你本地的 HTTP 代理地址是 http://127.0.0.1:7890
const agent = new ProxyAgent('http://127.0.0.1:7890');

fetch('https://api.github.com', { 
  dispatcher: agent 
})
  .then(res => res.text())
  .then(console.log);
```

**示例：HTTPS 代理**

如果你的代理支持 HTTPS，直接填代理地址即可：

```javascript
const agent = new ProxyAgent('https://你的代理地址:端口');
```

**示例：Socks 代理**

```javascript
const agent = new ProxyAgent('socks://127.0.0.1:1080');
```


## 3. 在 request 方法中使用代理

Undici 的 `request` 方法也可以通过 `dispatcher` 参数指定代理：

```javascript
const { request, ProxyAgent } = require('undici');
const agent = new ProxyAgent('http://127.0.0.1:7890');

request('https://api.github.com', {
  dispatcher: agent
}).then(({ body }) => body.text()).then(console.log);
```


## 4. 用于 Node 18+ 的原生 fetch

Node.js 18+ 的原生 fetch 也是用 undici 实现的，也支持 dispatcher 参数：

```javascript
const { ProxyAgent } = require('undici');
const agent = new ProxyAgent('http://127.0.0.1:7890');

fetch('https://api.github.com', { dispatcher: agent })
  .then(res => res.json())
  .then(console.log);
```


## 5. 多代理/条件代理

你可以根据目标 URL 动态选择代理：

```javascript
function getAgent(url) {
  if (url.includes('google')) return new ProxyAgent('http://127.0.0.1:7890');
  return undefined;
}

fetch('https://www.google.com', { dispatcher: getAgent('https://www.google.com') });
```

## 6. 示例

我想调用google AI服务，由于网络不通，所以需要proxy中转。

```
const { setGlobalDispatcher, ProxyAgent } = require('undici');
const { GoogleGenerativeAI } = require("@google/generative-ai");

// Set up the proxy agent
const proxyAgent = new ProxyAgent('http://127.0.0.1:7890'); // Use your proxy address
setGlobalDispatcher(proxyAgent);

// Initialize the Google AI client. It will automatically use the global dispatcher.
const genAI = new GoogleGenerativeAI("xxx");

async function google_main(send_content) {
  const model = genAI.getGenerativeModel({ model: "gemini-2.0-flash"});
  const prompt = `你是一名专业的计算机翻译人员，需要将用户提供的中文内容准确、流畅地翻译成英文。保持专业术语的准确性，同时确保英文表达自然符合目标语言习惯。示例：[修复], 翻译内容: [Fixed]

  需要翻译的内容: [${send_content}]
  `;

  const result = await model.generateContent(prompt);
  const response = await result.response;
  const text = response.text();
  return text;
}

module.exports = google_main;

```


## 官方文档

- [Undici ProxyAgent 文档](https://undici.nodejs.org/#/docs/api/ProxyAgent)
- [Undici fetch 文档](https://undici.nodejs.org/#/docs/api/fetch/)
