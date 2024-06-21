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



#### mimikatz

```powershell
// Some code
```



## 白银票据传递攻击

### 先决条件

* 已获得了目标域内服务账户的密钥值
* 已获得了目标域的SID值
* 目标域名
* 要伪造的域用户（高权限账户：域管理员）

### 工具

#### Impacket



#### mimikatz



{% hint style="info" %}
票据传递攻击的本质就是冒充其他账户。
{% endhint %}
