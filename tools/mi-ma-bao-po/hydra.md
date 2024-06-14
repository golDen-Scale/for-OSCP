---
description: 密码爆破
---

# ✔️ Hydra

## 通用格式

* **hydra \[-l 用户名/-L 用户名列表] \[-p 密码/-P 密码列表] -u -f \[目标IP地址/域名] \[-s 端口号] \[模块] "/login.php:username=^USER^\&password=^PASS^:F=\<form name='login' "**
  * \-u：要尝试每一个密码的每一个用户名（遍历用户名列表&密码列表）
  * \-f：找到第一个匹配项就立刻停止
  * 模块：视具体情况而定
  * "/login.php:username=^USER^\&password=^PASS^:F=\<form name='login' "：此处常用于针对Web应用的登录界面的具体规则指定，需要结合BurpSuite以及查看源码来指定

## &#x20;单独使用进行爆破

*









### 组合使用进行爆破（Hydra + BurpSuite）

* 常针对Web应用程序的登录界面 (HTTP / HTTPS)
* 常用模块：
  * **http-form-post**：用于破解基于 HTTP POST 表单的认证。通常用于网站登录页面的暴力破解。
  * **https-form-post**：与 http-form-post 类似，但针对 HTTPS 协议的表单认证。
  * **http-form-get**：用于破解基于 HTTP GET 表单的认证。
  * **https-form-get**：与 http-form-get 类似，但针对 HTTPS 协议的表单认证。
  * **https-post-form**：与 http-post-form 类似，但针对 HTTPS 协议

#### **http-post-form**：

* 用于执行更复杂的 HTTP POST 表单暴力破解，常用于需要多步验证或自定义表单参数的情况。

```bash
# 实例
hydra -L /usr/share/SecLists/Usernames/top-usernames-shortlist.txt -P /usr/share/SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f 192.168.xxx.xx -s 8080 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```

* \`/login.php:username=^USER^\&password=^PASS^\`：此处需要使用BurpSuite拦截表单请求，根据实际情况进行修改
* \`F=\<form name='login' \`：此处需要查看源码中表达实际的名称进行修改
