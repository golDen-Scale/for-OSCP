# ✔️ Windows权限提升 - Windows Privilege Escalation

## 本地信息枚举

### 手动枚举

```powershell
// 枚举系统信息
systeminfo
hostname
wmic qfe
wmic qfe get caption,Description,HotFixID,InstalledOn
wmic logicaldisk
wmic logicaldisk get caption,description,providername

// 用户枚举
whoami
whoami /priv
whoami /groups
net user
net user administrator
net localgroup
net localgroup administrators

```



### 工具枚举





## 提权方法

### 利用内核漏洞（Kernel Exploits）



### 密码搜索（Password Hunting）





### 冒充攻击（Impersonation Attacks）







### 注册表攻击（Registry Attacks）





### 可执行文件（Executable Files）





### 计划任务（Schedule Tasks）





### DLL劫持（DLL Hijacking）





### 服务权限（Service Permissions）





### WSL





### CVE-2019-1388
