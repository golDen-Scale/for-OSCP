---
description: Easy /
---

# ✔️ Access

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA access 10.129.8.225 --open
```









* 扫描同时尝试查看80端口上是否有web页面，发现只有一张图片，扫描隐藏文件/目录也没有收获：

```bash
dirsearch -u  http://10.129.8.225 -x 403,404,400
```

<figure><img src="../../.gitbook/assets/1 (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/3 (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (15).png" alt=""><figcaption></figcaption></figure>

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
