---
description: Windows / 中等难度 / Nexus Repository Manager OSS 3.21.0-05
---

# ✔️ Billyboss

## 建立立足点

### 信息枚举

* 使用Nmap针对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA billyboss 192.168.250.61
```

<figure><img src="../.gitbook/assets/Snipaste_2024-06-22_14-02-58.png" alt=""><figcaption></figcaption></figure>

* 同时也可直接尝试看看目标系统是否有web页面，本例的80端口正运行着一个叫BaGet的应用程序，可做一些常规的信息收集：

<figure><img src="../.gitbook/assets/Snipaste_2024-06-22_14-01-00.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Snipaste_2024-06-22_14-01-28.png" alt=""><figcaption></figcaption></figure>

* 通过Nmap的扫描结果，发现目标还有一个8081的开放端口，也正在运行着HTTP服务，依次查看相关网页内容，确定当前正运行的是Nexus Repository Manager OSS 3.21.0-05的应用程序：

<figure><img src="../.gitbook/assets/4 (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5 (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/6 (4).png" alt=""><figcaption></figcaption></figure>

* 使用dirsearch扫描一下该端口上还有没有其他的隐藏目录/文件：

<figure><img src="../.gitbook/assets/7 (5).png" alt=""><figcaption></figcaption></figure>

* 发现一个/swagger-ui，看它大小不抱什么希望，果然没什么东西：

<figure><img src="../.gitbook/assets/8 (5).png" alt=""><figcaption></figcaption></figure>











### 漏洞查阅







### 漏洞利用







### GET SHELL









## 权限提升

### 本机信息枚举









### 漏洞查阅







### 漏洞利用









### ROOT





{% hint style="info" %}
本例算中等偏难的机器，锁定可利用的漏洞和提权时都不难，但是其实现过程中需要根据实际情况进行修改和变通。
{% endhint %}
