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

* 合适的字典文件：rockyou.txt
* 使用Medusa来爆破受到htaccess保护的Web目录(本例为：/admin)

```bash
medusa -h 192.168.xxx.xx -u admin -P /usr/share/wordlists/rockyou.txt -M http -m DIR:/admin
```

* 受到htaccess保护的Web目录的特征：
  * 常会弹出一个对话框，要求输入用户名和密码
  * 如果没有提供正确的凭据，服务器通常会返回一个 401 Unauthorized 的状态码
  * 受保护的目录会在响应头中包含 `WWW-Authenticate` 头部

```bash
# 先确认该目录是否受htaccess保护：
curl -I http://example.com/admin
# 检查是否包含以下内容：
HTTP/1.1 401 Unauthorized       # 说明需要身份验证
WWW-Authenticate: Basic realm="Restricted Area"     # 需要客户端提供用户名和密码
```

### 远程桌面协议攻击（Crowbar）

* 需要更多的攻击时间，但是回报更大
* Crowbar (Levye)：该工具常用于针对远程桌面协议(RDP)的身份验证进行的爆破

```bash
 crowbar -b rdp -s 192.168.xxx.xx -u admin -C ~/password-file.txt -n 1
```

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
