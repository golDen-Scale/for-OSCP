---
description: 是为了找到有公开已知漏洞的服务，作为进入目标的切入点。
---

# ✔️ 信息收集和枚举 - Information Gathering

## 主动信息收集

利用工具或手工收集，会与目标直接交互，可能会被目标发现。

```bash
// 开放端口
nmap -Pn -sC -sV -p- -oN full 10.10.xx.xxx
nmap -sV -sC -p- -oA full 192.168.xxx.xxx --open
// 
```





## 被动信息收集

收集互联网上的各种公开信息，不与目标直接交互，不会被目标发现。



