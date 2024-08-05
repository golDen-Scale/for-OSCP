---
description: Windows - Easy：HP Power Manager / Buffer Overflow
---

# ✔️ Kevin

## 建立立足点

### 信息枚举

使用Nmap进行开放端口扫描：

<figure><img src="../.gitbook/assets/1 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

检查目标系统80端口上的内容，发现是HP Power Manager的登录界面，尝试弱口令登录（<mark style="color:red;">**admin:admin**</mark>），也可以直接搜索相关软件的默认凭证：

<figure><img src="../.gitbook/assets/2 (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

登录后，检查各项内容获得其版本号：HP Power Manager 4.2

<figure><img src="../.gitbook/assets/3 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅&利用（Metasploit）

初步查到HP Power Manager相关漏洞为缓冲区溢出：

<figure><img src="../.gitbook/assets/5 (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

上工具！利用Metasploit搜索相关可利用脚本：

<figure><img src="../.gitbook/assets/4 (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

设置远程IP、本地IP、本地端口：

<figure><img src="../.gitbook/assets/6 (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### GET SEHLL & ROOT

发现直接获取到了目标系统的最高权限：

<figure><img src="../.gitbook/assets/7 (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/8 (3).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例中利用的是HP Power Manager的缓冲区溢出漏洞，利用成功就能直接获得最高权限。

* [https://www.youtube.com/watch?v=wwyoh2kEO9I](https://www.youtube.com/watch?v=wwyoh2kEO9I)
{% endhint %}
