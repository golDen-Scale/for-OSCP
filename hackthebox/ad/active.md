---
description: Easy / SMB共享 / 枚举
---

# ✔️ Active

## 建立立足点

### 信息收集&枚举

* 使用Nmap对目标系统进行开放端口扫描：

<figure><img src="../../.gitbook/assets/1 (5).png" alt=""><figcaption></figcaption></figure>

* 顺便把IP地址和域名写进/etc/hosts中，以便后续使用：

<figure><img src="../../.gitbook/assets/2 (5).png" alt=""><figcaption></figcaption></figure>

* 使用Nmap查找一下相关的SMB漏洞：

```bash
# 过滤出本机上关于smb漏洞扫描的脚本：
local -r '\.nse$' | xargs grep categories | grep 'default\|version\|safe' | grep smb
```

<figure><img src="../../.gitbook/assets/3 (5).png" alt=""><figcaption></figcaption></figure>

```bash
nmap --script safe -p 445 10.129.85.245
```

<figure><img src="../../.gitbook/assets/4 (6).png" alt=""><figcaption></figcaption></figure>

没有什么收获：

<figure><img src="../../.gitbook/assets/6 (6).png" alt=""><figcaption></figcaption></figure>

查找到当前目标系统中SMB服务是2.0版本，使用smb-enum-services脚本也无收获，该脚本貌似是作用于SMB 1.0版本的：

<figure><img src="../../.gitbook/assets/7 (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (8).png" alt=""><figcaption></figcaption></figure>

* 尝试匿名登录SMB服务，并枚举其所有共享目录：

```bash
smbclient -L 10.129.85.245
```

<figure><img src="../../.gitbook/assets/9 (6).png" alt=""><figcaption></figcaption></figure>

分别用enum4linux和smbmap这两种工具对这些共享目录进行枚举以及确认当前权限：

```bash
# enum4linux：
enum4linux 10.129.85.245
# smbmap:
smbmap -H 10.129.85.245
```

<figure><img src="../../.gitbook/assets/10 (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/11 (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12 (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/13 (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
smbmap比enum4linux好用
{% endhint %}

### GET SHELL





## 权限提升

### 本机信息收集&枚举







### ROOT





{% hint style="info" %}
本例的考察重点是对目标SMB共享资源的枚举，属于简单级别的机器。
{% endhint %}
