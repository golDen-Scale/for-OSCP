---
description: 针对密码哈希值 / 针对Kerberos票据 / 获取域内目标账户的明文密码
---

# ✔️ 暴力破解域内账户密码

## 先决条件

* 已经在目标域内建立立足点
* 当前有足够权限通过工具、相关攻击技术能够提取到密码Hash / Kerberos票据

{% hint style="info" %}
原理：提取Hash / 票据——离线暴破——还原明文密码
{% endhint %}

## 常用工具

### John the Ripper

* 暴破Hash

```bash
# 字典文件实际情况选择
john hash.txt rockyou.txt
# 也可以先查查格式，再指定格式暴破
john --list=formats
john --formats=nt hash.txt rockyou.txt
```

* 暴破票据

```bash
// Some code
```

### Hashcat

* 暴破Hash

```bash
// Some code
```

* 暴破票据

```bash
// Some code
```

### Kerberoast

* 暴破Kerberos票据，常用脚本：**tgsrepcrack.py**

```bash
# 先用mimikatz提取出Kerberos票据（.kirbi文件）

```
