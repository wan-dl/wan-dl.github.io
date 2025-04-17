---
title: 使用packaging判断版本号大小
date: 2025-4-17
categories: Python
icon: Python
desc: 日常使用笔记
---

# packaging

```python
from packaging.version import Version

data = [
    {"version": "5.0.0"},
    {"version": "8.0.0"},
    {"version": "10.0.0"},
    {"version": "9.0.0"},
    {"version": "15.0.0"}
]

sorted_data = sorted(data, key=lambda x: Version(x["version"]), reverse=True)
print(sorted_data)
```