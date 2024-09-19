---
description: 信息收集 / 信息枚举
---

# ✔️ Dirsearch

## 参数

```
-u   指定目标URL
-x   过滤掉不需要返回信息的状态码
```

## 常用命令

```bash
# 枚举隐藏文件/目录，并过滤掉不需要返回信息的状态码
dirsearch -u http://目标IP -x 403,404,400
```
