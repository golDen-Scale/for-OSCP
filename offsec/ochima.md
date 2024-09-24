---
description: 中等
---

# ✔️ Ochima

## 建立立足点

### 信息收集

* 使用Nmap对目标系统开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA ochima 192.168.228.32 --open
```

{% hint style="info" %}
扫描太慢了........
{% endhint %}

* 扫描同时可尝试查看目标80端口是否开放，发现是一个Apache的默认界面：

<figure><img src="../.gitbook/assets/1 (15).png" alt=""><figcaption></figcaption></figure>

* 可检查一下80端口上是否有隐藏文件/目录，啥也没有：

```bash
dirsearch -u  http://192.168.228.32 -x 403,404,400
```

<figure><img src="../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

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
