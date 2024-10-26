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

<figure><img src="../../.gitbook/assets/5 (2).png" alt=""><figcaption></figcaption></figure>

* 扫描同时尝试查看80端口上是否有web页面，发现只有一张图片，扫描隐藏文件/目录也没有收获：

```bash
dirsearch -u  http://10.129.8.225 -x 403,404,400
```

<figure><img src="../../.gitbook/assets/1 (30).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (29).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/3 (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (28).png" alt=""><figcaption></figcaption></figure>

* 从Nmap的输出结果，端口21开放了FTP服务尝试匿名登录成功：

<figure><img src="../../.gitbook/assets/6 (28).png" alt=""><figcaption></figcaption></figure>

* 分别查看这两个目录中的内容，并且把它们下载到本地：

<figure><img src="../../.gitbook/assets/7 (28).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (28).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/9 (27).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
FTP中的文件始终下载不成功，不知道什么原因，暂时搁置...
{% endhint %}



***

















### 漏洞查阅









### 漏洞利用











### GET SHELL











## 权限提升

### 本地信息收集









### 漏洞利用











### ROOT





{% hint style="info" %}

{% endhint %}
