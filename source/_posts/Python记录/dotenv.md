---
title: 在 Python 中使用dotenv加载环境变量
date: 2025-4-17
categories: Python
icon: Python
desc: 日常使用笔记
---

`python-dotenv` 是一个用于在 Python 项目中加载环境变量的库，它可以从 `.env` 文件中读取键值对并将其添加到环境变量中。这对于管理敏感信息（如 API 密钥、数据库凭据等）和配置项目环境非常有用。

### 核心功能
- **加载环境变量**：从 `.env` 文件中加载环境变量到 `os.environ` 中。
- **管理敏感信息**：将敏感信息存储在 `.env` 文件中，避免硬编码到代码中。
- **跨环境支持**：可以为开发、测试和生产环境设置不同的 `.env` 文件。

---

### 安装
可以通过 `pip` 安装：
```bash
pip install python-dotenv
```

---

### 使用方法

#### 1. 创建一个 `.env` 文件
在项目根目录创建一个 `.env` 文件，并定义一些键值对：
```
SECRET_KEY=your_secret_key
DATABASE_URL=postgres://user:password@localhost:5432/dbname
DEBUG=True
```

#### 2. 加载 `.env` 文件
在 Python 文件中加载 `.env` 文件：
```python
from dotenv import load_dotenv
import os

# 加载 .env 文件
load_dotenv()

# 获取环境变量
secret_key = os.getenv('SECRET_KEY')
database_url = os.getenv('DATABASE_URL')
debug_mode = os.getenv('DEBUG')

print(f"SECRET_KEY: {secret_key}")
print(f"DATABASE_URL: {database_url}")
print(f"DEBUG: {debug_mode}")
```

---

### 高级功能
- **指定文件路径**  
  如果 `.env` 文件不在当前目录，可以指定文件路径：
  
```python
load_dotenv(dotenv_path='/path/to/.env')
```

- **覆盖系统环境变量**  
  默认情况下，`load_dotenv()` 不会覆盖已经存在的系统环境变量。可以通过设置 `override=True` 来覆盖：
  
```python
load_dotenv(override=True)
```

- **查找 `.env` 文件**  
  如果 `.env` 文件可能位于不同的目录，可以使用 `find_dotenv` 自动查找：
  
```python
from dotenv import find_dotenv, load_dotenv
load_dotenv(find_dotenv())
```

---

### 使用场景
1. **本地开发环境配置**：将开发环境的配置项存储在 `.env` 文件中。
2. **敏感信息管理**：避免将 API 密钥等敏感信息直接暴露在代码仓库中。
3. **跨环境配置**：为不同的环境（如开发、测试、生产）使用不同的 `.env` 文件。

---

### 注意事项
1. **不要将 `.env` 文件提交到版本控制系统**：可以在 `.gitignore` 文件中添加 `.env`。
2. **安全性**：尽量在生产环境中使用更安全的方式（如密钥管理服务）来存储敏感信息，而不是依赖 `.env` 文件。

通过 `python-dotenv`，可以更方便地管理环境变量，从而提升代码的安全性和可维护性。