---
description: 适用于对目标系统的活动目录基础结构的了解与收集
---

# ✔️ 枚举AD环境

## 先决条件

* 已建立至少一个属于目标域内的服务器/客户端机器的立足点
* 然后再对目标AD环境进行枚举

## 目的

* 成功建立立足点后，就是提高当前在目标域内的权限级别：
  * 查找目标域内的高价值组（如：域管理组 Domain Admin Group），针对该组中的成员账户进行下一步行动，因为它们在目标域内拥有更高的权限
  * 针对目标域中的域控制器进行下一步行动，因为它可能被用来修改所有加入目标域的计算机，或在这些机器上执行应用程序，并且域控制器包含每个域用户帐户的所有密码散列

## 常规枚举方法

* 主要使用的是内置的net.exe进行本地账户枚举：

```powershell
net user
net user /domain
net user 用户名 /domain
net group /domain
```

## 高效枚举方法

* Powershell的Get-ADUer很好用，不过它通常只默认安装在域控上，也有可能安装在Win 7及以上的Windows机器中，但是需要管理权限才能执行。
* 用Powershell编写一个可枚举域内AD用户以及这些账户所有属性的脚本：
  * 依赖的组件：LDAP / ADSI搜索器 / 目标域控制器名称 / 目标域名

```powershell
$domainObj = [System.DirectoryService.ActiveDirectory.Domain]::GetCurrentDomian()
$PDC = ($domainObj.PdcRoleOwner).Name
$SearchString = "LDAP://"
$SearchString += $PDC + "/"
$DistinguishedName = "DC=$($domainObj.Name.Replace('.',',DC='))"
$SearchString += $DistinguishedName
$Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
$objDomain = New-Object System.DirectoryServices.DirectoryEntry
$Searcher.SearchRoot = $objDomain
$Searcher.filter = "samAccountType=805306368"     # 可根据实际需求修改
$Searcher.FindAll()
Foreach($obj in $Result)
{
    Foreach($prop in $obj.Properties)
    {
        $prop                                     # 可根据实际需求修改
    }
    Write-Host "----------------------"
}
```

* 过滤嵌套组：

```powershell
$Searcher.filter = "(objectClass=Group)"
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
    $obj.Properties.name
}
```

* 过滤Secret\_Group组成员：

```powershell
$Searcher.filter = "(name=Secret_Group)"
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
    $obj.Properties.member
}
```

* 过滤Nested\_Group组成员：

```powershell
$Searcher.filter = "(name=Nested_Group)"
```

* 以此类推。





{% hint style="info" %}
* 从技术上讲，该属性称为PdcRoleOwner，具有此属性的域控制器总是拥有关于用户登录和身份验证的最新信息
* 该脚本将在目标网络中查询主域控制器模拟器和域的名称，搜索活动目录并过滤输出以显示用户帐户，然后清理输出以获得更高的可读性
* 该脚本非常灵活，可根据需要添加特性和函数
* 管理员通常倾向于在用户名中添加前缀或后缀，从而通过其功能来识别帐户
{% endhint %}







