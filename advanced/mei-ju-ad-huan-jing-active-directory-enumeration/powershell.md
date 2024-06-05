---
description: 适用于在AD环境中进行域内信息枚举
---

# ✔️ Powershell

## 使用ADSI搜索器

```powershell
# 列出所有域用户
$Searcher = New-Object DirectoryServices.DirectorySearcher
$Searcher.Filter = "(objectClass=user)"
$Results = $Searcher.FindAll()
Foreach ($Result in $Results) {
    $Result.Properties
}

# 列出特定组的成员
$Group = [ADSI]"LDAP://CN=Domain Admins,CN=Users,DC=example,DC=com"   # 按实际情况修改
$Group.Member
```

## 使用Get-ADUser

```powershell
# 列出所有域用户
Get-ADUser -Filter *
# 列出禁用的用户账户
Get-ADUser -Filter {Enabled -eq $false}
# 列出特定OU中的用户
Get-ADUser -Filter * -SearchBase "OU=Employees,DC=example,DC=com"   # 按实际情况修改
# 列出最近登录的用户
Get-ADUser -Filter {LastLogonDate -ne $null} | Sort-Object LastLogonDate -Descending
```

## 使用Get-ADGroup

```powershell
# 列出所有域组
Get-ADGroup -Filter *
# 列出特定组的成员
Get-ADGroupMember -Identity "Domain Admins"
# 列出特定用户所属的所有组
Get-ADUser -Identity "用户名" | Get-ADUserMemberOf
```

## 使用Get-ADComputer

```powershell
# 列出域内所有计算机
Get-ADComputer -Filter *
# 列出特定OU中的计算机
Get-ADComputer -Filter * -SearchBase "OU=Servers,DC=example,DC=com"    # 按实际情况修改
# 列出最近登录的计算机
Get-ADComputer -Filter {LastLogonDate -ne $null} | Sort-Object LastLogonDate -Descending
```

## 使用Get-ADOrganizationalUnit（组织单元）

```powershell
# 列出所有OU
Get-ADOrganizationalUnit -Filter *
# 列出特定OU中的所有计算机
Get-ADOrganizationalUnit -Filter * | Get-ADComputer
```

## 使用Get-ADGroupPolicyObject（组策略）

```powershell
# 列出所有GPO
Get-GPO -All
# 列出特定OU应用的GPO
Get-GPOReport -All -ReportType XML | Select-String -Pattern "OU=Servers"
```

## 使用Get-ACL查看ACL

```powershell
# 列出所有域对象的ACL
Get-ADObject -Filter * | Get-ACL
# 列出特定用户的ACL
Get-ADUser -Identity "jdoe" | Get-ACL
```

## 使用Get-ADTrust查看域信任

```powershell
# 列出所有域信任关系
Get-ADTrust -Filter *
```

## 枚举域内高权限用户账户

```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse | Get-DomainUser
```

## 枚举域内具有特定权限的用户账户

* `S-1-5-21` 是一个常见的域用户和组SID的前缀，通常用于标识特定域中的安全主体。

```powershell
Get-DomainObjectAcl -ResolveGUIDs | Where-Object { $_.SecurityIdentifier -like "*S-1-5-21*" } | Select-Object -Property ActiveDirectoryRights, ObjectDN, SecurityIdentifier
```

## 枚举域内所有有委派权限的对象

* 域内有委派（Delegate）权限的对象意味着它们能够将自己的身份验证信息转发给其他服务，允许这些服务以它们的身份进行操作。可以被用于扩大攻击范围，提升权限，甚至完全控制域内的资源。

```powershell
Find-Delegation -ResolveGUIDs
```

{% hint style="info" %}
* PowerShell是一个跨平台的任务自动化和配置管理框架，它包括一个命令行shell和一个相关的脚本语言，主要用于系统管理和自动化。
* 而PowerView是一个用于Active Directory信息收集和渗透测试的PowerShell脚本库。专门用于Active Directory环境的信息收集和渗透测试。它是PowerSploit工具套件的一部分，广泛用于渗透测试和红队操作。
* 它们相关，但不同。
{% endhint %}
