---
icon: check
description: Easy / Linux / Redis
---

# Wombo

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA wombo 192.168.197.69 --open
```

<figure><img src="../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

* 通过扫描结果发现了2个运行web的端口：80和8080，80端口运行的是一个nginx的web服务器，8080端口运行的是一个名为NodeBB的CMS Web应用：

<figure><img src="../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

* 分别针对80和8080端口进行隐藏文件/目录扫描，发现8080端口扫出了很多东西：

```bash
dirsearch -u  http://192.168.197.69:8080 -x 403,404,400
```

<figure><img src="../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

* 同时，8080端口上的robots.txt文件也指向了3个文件/目录：

























### 漏洞查阅









### 漏洞利用









### GET SHELL











## 权限提升

### 本地信息收集







### 漏洞利用







### ROOT









{% hint style="info" %}
（二刷机器，补笔记。本例机器中途重置过，IP地址有变化，不影响漏洞利用及其实现结果）
{% endhint %}
