---
title: MacOSX 使用codesign签名
date: 2024-5-23
categories: MacOSX
icon: apple
---

#### codesign常用的签名命令

```shell
codesign -vvv --deep --force --sign "证书名称" <file_path>
```

#### 查看本地所有可用签名证书

```shell
security find-identity -v -p codesigning
```

输出结果:

```shell
  1) 86C79BE9894C96EE8D6B7288DC4ADC638454CF56 "xxxxxxx"
     1 valid identities found
```


#### 签名验证

```
codesign --verify --deep --verbose=2 <file_path>

输出:

<file_name>: valid on disk
<file_name>: satisfies its Designated Requirement
```

#### 清除签名

```shell
codesign --remove-signature <file_path>
```