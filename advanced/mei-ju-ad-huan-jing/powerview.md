---
description: 适用于：信息收集、域枚举、权限提升、权限维持、分析目标域中的攻击路径等。
---

# ✔️ PowerView

## 常规方法

### 信息收集

收集目标系统上的用户、组、组成员、主机相关信息：

```powershell
Get-NetGroup -AdminCount
Get-NetGroup -UserName admin123
Get-NetGroupMember -GroupName "Administrators"
Get-NetComputer -Ping
Get-NetComputer -fulldata
Get-NetComputer -fulldata | select cn operatingsystem
Get-NetUser
Get-NetUser | select cn 
```

### 域枚举

枚举目标域内各类信息，以及查找自己当前在目标域内拥有哪些主机的管理员权限：

```powershell
Get-NetDomain
Get-DomainSID
Get-DomainPolicy
Get-NetDomainController
Find-LocalAdminAccess
Invoke-EnumerateLocalAdmin
Get-NetGPO
Get-NetLoggedon -ComputerName Domain-controller.CONTROLLER.local
Get-LastLoggedon -ComputerName Domain-controller.CONTROLLER.local
Get-NetRDPSession -ComputerName Domain-controller.CONTROLLER.local
Invoke-ShareFinder
```

#### 枚举域用户

```powershell
# 列出目标域中所有的用户
Get-DomainUser
# 列出禁用的账户
Get-DomainUser -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=2)"
```

#### 枚举域组

```powershell
# 列出所有域组
Get-DomainGroup
# 列出特定组Domain Admins的成员
Get-DomainGroupMember -Identity "Domain Admins"
# 列出指定用户所属的所有组
Get-DomainUser -Identity "用户名" | Get-DomainGroup
```

#### 枚举计算机

```powershell
# 列出所有的域内计算机
Get-DomainComputer
# 列出最近登录的计算机
Get-DomainComputer | ? { $_.lastlogontimestamp -ne $null } | Sort-Object lastlogontimestamp -Descending
```

#### 枚举组织单元（OU）

```powershell
# 列出所有的组织单元
Get-DomainOU
# 列出特定OU中的所有计算机
Get-DomainOU | Get-DomainComputer
```

#### 枚举组策略对象（GPO）

```powershell
# 列出所有的GPO
Get-DomainGPO
# 列出特定OU所应用的GPO
Get-DomainOU | Get-DomainGPO
```

#### 枚举ACL权限

```powershell
# 列出所有的域对象的ACL
Get-DomainObjectAcl
# 列出指定用户的ACL
Get-DomainObjectAcl -Identity "用户名"
```

#### 枚举会话和登录信息

```powershell
# 列出指定计算机上的会话
Get-NetSession -ComputerName "comp1.example.com"
# 列出指定计算机的登录信息
Get-NetLoggedon -ComputerName "comp1.example.com"
```

#### 枚举域信任关系

```powershell
# 列出所有域信任关系
Get-DomainTrust
```

### 分析目标域中的攻击路径











### 常用命令

```powershell
// 常用命令
Find-LocalAdminAccess ：命令可以查找在域中具有本地管理员权限的系统。
Get-NetDomain：获取当前域的信息，包括域名、域SID、域控制器等。
Get-NetDomainController：获取域控制器的信息，包括域控制器的名称、IP 地址、操作系统等。
Get-NetGroup：获取域中的组信息，包括组名、组 SID、成员等。
Get-NetUser：获取域中的用户信息，包括用户名、用户 SID、用户组等。
Get-NetComputer：获取域中的计算机信息，包括计算机名、IP 地址、操作系统等。
Get-NetShare：获取域中共享的信息，包括共享名称、共享路径、权限等。
Get-NetSession：获取域中的会话信息，包括用户会话、计算机会话、会话状态等。
Get-NetLoggedon：获取域中当前已登录用户的信息，包括用户名、计算机名、登录时间等。
Get-NetLocalGroup：获取目标系统的本地组信息，包括组名、组成员等
Get-NetLocalGroupMember：获取目标系统本地组的成员信息。
Invoke-ShareFinder：搜索目标系统上的共享资源。
Invoke-UserHunter：搜索域中的高权限用户，包括域管理员、企业管理员等。
Invoke-Mimikatz：执行 Mimikatz 工具进行内存中凭证的提取，用于横向移动和权限维持。
Invoke-ACLScanner：扫描目标系统的访问控制列表（ACL），查找潜在的权限问题。
Find-LocalAdminAccess：搜索域中具有本地管理员权限的系统。
```

{% hint style="info" %}
* PowerShell 是一个任务自动化和配置管理框架，它包括一个命令行shell和一个相关的脚本语言，主要用于系统管理和自动化。
* 而PowerView是一个用于Active Directory信息收集和渗透测试的PowerShell脚本库。专门用于Active Directory环境的信息收集和渗透测试。它是PowerSploit工具套件的一部分，广泛用于渗透测试和红队操作。
* 它们相关，但不同。
{% endhint %}
