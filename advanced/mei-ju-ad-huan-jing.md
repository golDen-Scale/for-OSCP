---
description: 适用于对目标系统的活动目录基础结构的了解与收集
---

# ✔️ 枚举AD环境

## 先决条件

* 已建立至少一个属于目标域内的服务器/客户端的立足点
* 然后在对目标AD环境进行枚举

## 常规枚举方法

* 主要使用的是内置的net.exe进行本地账户枚举：

```bash
net user
net user /domain
net user 用户名 /domain
net group /domain
```

## 高效枚举方法

* Powershell的Get-ADUer很好用，不过它通常只默认安装在域控上，也有可能安装在Win 7及以上的Windows机器中，但是需要管理权限才能执行。
* 用Powershell编写一个可枚举AD用户以及这些账户所有属性的脚本：

```powershell
// Some code
```



{% hint style="info" %}
* 从技术上讲，该属性称为PdcRoleOwner，具有此属性的域控制器总是拥有关于用户登录和身份验证的最新信息
* 该脚本将在网络中查询主域控制器模拟器和域的名称，搜索活动目录并过滤输出以显示用户帐户，然后清理输出以获得可读性
* 该脚本非常灵活，可根据需要添加特性和函数
{% endhint %}







