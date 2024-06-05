---
description: 目标：高价值的服务账户 /
---

# ✔️ 通过服务主体名进行枚举（SPN）

## Tips

* 独立的应用程序可以使用一组预定义的服务账户：
  * LocalSystem
  * LocalService
  * NetworkService
* 对于更复杂的应用程序，可能会使用域用户账户来提供所需的上下文，同时仍然能够访问域内的资源
* 像 Exchange、SQL 或 Internet Information Services (IIS) 这样的大型应用程序集成到 Active Directory 中时，使用称为服务主体名称 (Service Principal Name, SPN) 的唯一服务实例标识符，将特定服务器上的服务与 Active Directory 中的服务账户相关联
* Windows Server 2008 R2 中引入了托管服务账户（Managed Service Accounts）
* Windows Server 2012 中引入了组托管服务账户（Group Managed Service Accounts）

## 先决条件

* 已获取到一个目标域内的用户账户（不用非得是高权限账户，只要有能够对Active Directory进行查询的基本权限的账户就行）
* 与目标域处于相同的网络环境中，能够访问域控制器来进行查询

## 方法

### 方法1

1. Powershell脚本枚举：

```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$PDC = ($domainObj.PdcRoleOwner).Name
$SearchString = "LDAP://"
$SearchString += $PDC + "/"
$DistinguishedName = "DC=$($domainObj.Name.Replace('.',',DC='))"
$SearchString += $DistinguishedName
$Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
$objDomain = New-Object System.DirectoryServices.DirectoryEntry
$Searcher.SearchRoot = $objDomain
$Searcher.filter = "serviceprincipalname=*http*"       # 可根据实际情况修改
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
    Foreach($prop in $obj.Properties)
    {
        $prop
    }
}
```

* [x] samaccountname
* [x] serviceprinciplename

2. 使用nslookup解析枚举出的域名，从而获得一个内网的IP地址：

```bash
nslookup xxx.example.com
```

### 方法2

#### PowerView

```powershell
Import-Module PowerView
Get-SPN -Domain 目标域名
```

#### Impacket

```powershell
python GetUserSPNs.py -dc-ip <DomainControllerIP> <DOMAIN>/<USER>:<PASSWORD>
```
