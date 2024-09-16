---
icon: check
description: Hard / Linux /
---

# ✔️ Nibbles

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA nibbles 192.168.218.47 --open
```

* 扫描的同时可尝试直接使用IP地址登录查看是否有Web页面，很幸运有，但是好像没有什么特别的信息。

<figure><img src="../.gitbook/assets/1 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/2 (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/3 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/6 (12).png" alt=""><figcaption></figcaption></figure>

* 使用dirsearch检查是否有隐藏文件/目录，没有：

```bash
dirsearch -u  http://192.168.218.47 -x 403,404,400
```

<figure><img src="../.gitbook/assets/4 (12).png" alt=""><figcaption></figcaption></figure>

*

































### 漏洞查阅









### 漏洞利用











### GET SHELL













## 权限提升

### 本地信息收集







### 漏洞利用









### ROOT















{% hint style="info" %}

{% endhint %}
