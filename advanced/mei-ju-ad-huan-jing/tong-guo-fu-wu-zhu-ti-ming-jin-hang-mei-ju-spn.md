---
description: 目标：高价值的服务账户 /
---

# ✔️ 通过服务主体名进行枚举（SPN）

## 先决条件





## 方法

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

2. 使用nslookup解析枚举出的域名，从而获得一个内网的IP地址：

```bash
nslookup xxx.example.com
```
