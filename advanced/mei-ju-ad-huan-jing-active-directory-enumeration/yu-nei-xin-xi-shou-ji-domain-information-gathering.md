---
description: 已经确认当前所在的主机在域环境之内
---

# ✔️ 域内信息收集 - Domain Information Gathering

## 基础信息收集

```powershell
# 列出当前域中所有的主机
net view /domain
# 通过上一命令查询到的主机名，查询指定主机的进一步的信息
net view /domain:主机名
# 列出当前域中所有用户组列表
net group /domain 
```

## 查找域控制器

```powershell
# 列出指定域或工作组中的域控制器的名称和 IP 地址列表
nltest /DCLIST:xxx
# 获取域控制器的当前时间（因为主域控制器通常也会充当时间服务器的角色）
net time /domain
# 查询域中名为Domain Controllers的组的成员信息（包含了所有的域控制器的计算机账户）
net group "Domain Controllers" /domain
# 查询 DNS 中的 SRV 记录，以获取 LDAP 服务在域中的位置信息
nslookup -type=SRV _ldap._tcp
# 于查询当前域的主域控制器（PDC）的名称
netdom query pdc
```



## 获取域内账户&管理员信息

```powershell
# 查看域中系统账户krbtgt的属性和信息
net user krbtgt
# 获取域中所有用户账户的基本信息（普通账户、内置账户、服务账户）
net user /domain
# 列出本地计算机上的管理员组中的成员
net localgroup administrators
# 查询域中Domain Admins组的成员信息（具有在特定域内管理和维护域的权限）
net group "domain admins" /domain
# 查询域中Enterprise Admins组的成员信息（具有在整个企业（跨域）范围内管理和维护域的权限）
net group "Enterprise Admins" /domain

```

## 定位域管理员

### 方法

1. **日志**：本地机器的管理员日志
2. **会话**：域内每台机器的登录会话

### 工具

#### psloggedon.exe

```bash
PsLoggedon.exe \\DC
```















## 查找域管理进程

```powershell
# Some code

```







