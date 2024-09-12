---
description: 基于NTLM认证 / 不需要知道明文密码 / 较旧的系统
---

# ✔️ 哈希传递 - Pass The Hash

## 先决条件

* 先要找到与账号相关的密码散列 (NTLM Hash)
*



## 具体实现

### 技术原理

* 利用已知哈希值来进行身份验证，不需要知道账户的明文密码
*



### 实现步骤





## 常用工具

### Mimikatz

```powershell
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords full
```



### Impacket

* 几个常用脚本，按实际情况在该工具的/examples目录中选择合适脚本：

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

### Crackmapexec

*

### Metasploit

*





{% hint style="info" %}
哈希传递攻击的本质就是冒充其他用户。
{% endhint %}
