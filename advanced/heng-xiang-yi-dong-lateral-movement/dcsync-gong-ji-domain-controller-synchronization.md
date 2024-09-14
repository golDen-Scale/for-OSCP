---
description: 横向移动 / 滥用身份验证 /
---

# ✔️ DCSync攻击 - Domain Controller Synchronization

## 先决条件

* 需要获取到目标域内一个有效的域账户
* 该账户需要有足够高的权限，因为需要模拟域控之间的数据同步行为
  * Administrators组成员
  * 域管理员组成员
* 需要能够与目标域内的域控建立通信

## 常用工具

### mimikatz.exe

```powershell
mimikatz.exe
# 指定要同步的目标账户
lsadump::dcsync /user:administrator
lsadump::dcsync /user:krbtgt
lsadump::dcsync / 
# 获取Hash NTLM字段的值

# 
```

### Impacket

* 常用脚本：**secretsdump.py**

```bash
python3 secretsdump.py 
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
