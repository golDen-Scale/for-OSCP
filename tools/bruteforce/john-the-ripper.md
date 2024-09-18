---
description: NTLM哈希值暴破 / Kerberos票据暴破
---

# ✔️ John The Ripper

## 参数

```
1.指定字典文件：--wordlist=rockyou.txt  
2.指定暴破的hash格式：--format=ntlm / --format=krb5tgs
3.查看支持的hash格式：--list=formats
4.暴破时提速：--fork=8
5.尝试更多的密码变种：--rules
```

## 常用命令

```bash
# 常规暴破NTLM哈希
john hash.txt rockyou.txt
john --format=ntlm --wordlist=rockyou.txt hash.txt
john --rules --format=ntlm --wordlist=rockyou.txt hash.txt
john --rules --format=ntlm --wordlist=rockyou.txt hash.txt --fork=8
# 
```
