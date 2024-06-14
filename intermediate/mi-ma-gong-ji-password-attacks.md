---
description: 用于发现和使用有效凭证
---

# ✔️ 密码攻击 - Password Attacks

## 字典

* 优先考虑速度，会舍弃密码的有效覆盖率为代价
* 在创建和目标相关的字典文件时，精确度比覆盖率更重要（可创建与目标相关性更高的字典文件）
* /usr/share/wordlists/
* [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)
* 工具：John the Ripper（根据实际需求修改规则/etc/john/john.conf）

## 暴力破解

* 优先考虑密码的覆盖率，以牺牲速度为代价
* 直接跑现成的字典文件的话简单，耗时，但效率低下
* 创建更符合目标需求的字典列表：
  * 与特定模式匹配的所有可能密码的字典文件
  * crunch + 占位符

## 常见的网络服务攻击方式

{% hint style="info" %}
多次失败的登录尝试通常会在目标系统中产生风险：

* 日志和告警
* 锁定账户
* 在系统管理员重启该账户之前，无法再访问目标系统
{% endhint %}

* 提高密码攻击的效率：
  * 根据对应的**协议（HTTP/RDP/SSH/VNC/FTP/POP3/SNMP）**
  * 使用相关的**工具（Medusa/THC-Hydra/Crowbar/spray）**
  * 增加登录的线程数量（可能会因为协议而受限）
  * 在实施密码攻击之前，选择更适合特定目标的**用户列表**和**密码文件**

### HTTP htaccess攻击（Medusa）

*



### 远程桌面协议攻击（Crowbar）

* 需要更多的攻击时间，但是回报更大





### SSH攻击（Hydra）

* 利用Hydra对各种协议的身份验证进行暴力破解
* 其他各类协议以此类推

```bash
 hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.xxx.xx
```

### HTTP POST攻击（Hydra）

* 利用Hydra对Web应用程序的登录表单进行暴力破解
* 表单为POST请求

```bash
hydra 192.168.xxx.xx http-form-post "/form/frontpage.php:user=admin&pass=^PAS
S^:INVALID LOGIN" -l admin -P /usr/share/wordlists/rockyou.txt -vV -f
```

## 利用密码散列

* 一旦获取对目标系统的访问权限，能够提取密码hash时，就可以通过破解哈希来获取其明文密码
* 大多数使用密码身份验证的系统都需要将密码以散列形式存储在本地（非明文）
* 这意味着在身份验证的过程中，密码验证将以散列的形式和之前存储的消息摘要进行对比

### 检索&识别密码哈希

* 工具：hashid

```bash
// Some code
```

### 在Windows中进行哈希传递







### 哈希爆破

* 工具：John the Ripper

```bash
// Some code
```

* 工具：hashcat

```bash
// Some code
```

