---
description: 已经确认当前所在的主机在域环境之内
---

# ✔️ 域内信息收集 - Domain Information Gathering

## 基础信息收集

```bash
// 列出当前域中所有的主机
net view /domain
// 通过上一命令查询到的主机名，查询指定主机的进一步的信息
net view /domain:主机名
// 列出当前域中所有用户组列表
net group /domain
// 
```

## 查找域控制器

```bash
// Some code

```



## 获取域内账户&管理员信息

```bash
// 查看域中系统账户krbtgt的属性和信息
net user krbtgt
// 获取域中所有用户账户的基本信息（普通账户、内置账户、服务账户）
net user /domain
// 列出本地计算机上的管理员组中的成员
net localgroup administrators
// 查询域中Domain Admins组的成员信息（具有在特定域内管理和维护域的权限）
net group "domain admins" /domain
// 查询域中Enterprise Admins组的成员信息（具有在整个企业（跨域）范围内管理和维护域的权限）
net group "Enterprise Admins" /domain
// 

```

## 定位域管理员

```bash
// Some code

```

## 查找域管理进程

```bash
// Some code

```
