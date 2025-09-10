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


### 字段批量复制与重命名

abc表，复制字段type为device_type，并将type的值赋值给device_type

```shell
db.abc.updateMany(
  { type: { $exists: true } }, // 只操作有type字段的文档
  [
    { $set: { device_type: "$type" } }
  ]
)
```

第一参数 `{ type: { $exists: true } }` 只匹配含有 type 字段的文档。
第二参数为聚合管道数组，`$set` 操作把 `type` 字段的值赋给新字段 `device_type`