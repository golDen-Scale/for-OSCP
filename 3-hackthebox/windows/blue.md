---
description: Easy / 枚举 / 永恒之蓝
---

# Blue

## 建立立足点

### 信息收集

* 使用Nmap对目标进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA blue 10.129.223.191 --open
```

<figure><img src="../../.gitbook/assets/1 (10).png" alt=""><figcaption></figcaption></figure>

* 由于本例并没有开放的Web应用的端口，所以直接无密码查看SMB共享：

<figure><img src="../../.gitbook/assets/2 (8).png" alt=""><figcaption></figcaption></figure>

* 比较值得注意的是/Share和/Users，空密码登录到/Users和/Share中，并没有什么值得关注的内容：

```bash
smbclient  //10.129.223.191/Users
smbclient  //10.129.223.191/Share
```

<figure><img src="../../.gitbook/assets/3 (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6 (10).png" alt=""><figcaption></figcaption></figure>

* 因为本例既没有开放的80端口，检查smb服务时也没有什么特别的发现，所以重新进行信息枚举（HTB的机器名字通常能起到提示作用）：

```bash
nmap -p 445 --script smb-vuln-* 10.129.223.191
```

<figure><img src="../../.gitbook/assets/7 (13).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 果然重新扫描后发现存在永恒之蓝（MS17-010）漏洞，查找相关漏洞利用：

```
searchsploit ms17-010
```

<figure><img src="../../.gitbook/assets/8 (14).png" alt=""><figcaption></figcaption></figure>







### 漏洞利用









### GET SHELL













## 权限提升

### 本地信息收集









### 漏洞查阅









### 漏洞利用









### ROOT









{% hint style="info" %}

{% endhint %}
