---
description: Kerberos身份验证 / NTLM身份验证
---

# ✔️ AD身份验证 - Active Directory Authentication

## 先决条件

* 已收集到了目标域内的用户账户信息（低权限/高权限）、组成员信息、SPN信息等

## 方法

### NTLM身份验证协议

* NTLM认证是通过加密质询-响应机制
* 用于当客户端通过IP地址进行身份验证（不是通过hostname）
* 当用户尝试对未在Active Directory集成DNS服务器上注册的主机名进行身份验证时
* 第三方应用程序可能会选择使用NTLM认证而不是Kerberos认证

### Kerberos身份验证协议

* Kerberos认证是通过票据系统（Ticket）
* 自Windows Server 2003以来一直作为微软的主要认证机制
*

### 存储和检索已缓存的凭证

* 是作为权限提升和横向移动时的重要切入点，也是作为完成一整个攻击链的起点
*
* 常用工具：
  * Mimikatz
  * Invoke-Mimikatz
  * Impacket
  * SharpHound/BloodHound
