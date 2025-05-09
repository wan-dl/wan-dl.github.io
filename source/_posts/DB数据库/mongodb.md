---
title: MongoDB常用语句
date: 2025-4-25
categories: mongodb
icon: mongodb
---

### 批量更新集合中的文档

```sql
db.collectuib.updateMany(
  {},
  { $set: { status: 1 } }
)
```