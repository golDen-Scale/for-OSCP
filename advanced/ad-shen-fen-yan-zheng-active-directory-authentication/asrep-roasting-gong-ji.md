---
description: 基于Kerberos认证 / 离线爆破攻击 / 未开启Kerberos预身份验证
---

# ✔️ AS-REP Roasting攻击

## 先决条件

* 目标用户账户勾选了“不要求Kerberos预身份验证”这个选项
*

## 常用工具

### Impacket

* 常用脚本：**GetNPUsers.py**

```bash
// Some code
```

### Powershell

* 常用脚本：**ASREPRoast.ps1**

```powershell
// Some code
```





{% hint style="info" %}
AS-REP Roasting攻击有些局限，因为它主要是针对没有开启Kerberos预身份验证的用户账户的攻击，然后通过离线爆破的方式获取到目标用户的密码。
{% endhint %}
