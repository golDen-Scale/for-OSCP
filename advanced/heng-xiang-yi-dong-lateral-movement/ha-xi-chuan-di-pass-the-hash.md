---
description: 基于NTLM认证 / 不需要知道明文密码 / 较旧的且使用NTLM协议的系统
---

# ✔️ 哈希传递 - Pass The Hash

## 先决条件

* 需要找到目标用户账户的NTLM哈希
* 目标域名
* 目标系统必须支持SMB / WMI / RPC 等远程认证协议

## 具体实现

### 技术原理

* 利用已知哈希值来进行身份验证，不需要知道账户的明文密码

## 常用工具

### mimikatz.exe

```powershell
mimikatz.exe
# 权限提升
privilege::debug

sekurlsa::logonpasswords full
```

### Impacket

* 几个常用脚本，按实际情况在该工具的/examples目录中选择合适脚本：

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

### Crackmapexec

```bash
# 
```



### Metasploit

```bash
# psexec模块
use exploit/windows/smb/psexec
# wmiexec模块
use auxiliary/admin/smb/wmiexec
# smb_login模块
use auxiliary/scanner/smb/smb_login
# smbexec模块
use exploit/windows/smb/smbexec
```



{% hint style="info" %}
哈希传递攻击分为针对本地用户账户和域用户账户，通过抓取目标用户的NTLM哈希来绕过明文密码登录，其本质就是冒充特定的用户账户。
{% endhint %}
