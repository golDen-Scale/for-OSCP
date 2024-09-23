---
icon: check
description: Easy / Linux / Redis
---

# ✔️ Wombo

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA wombo 192.168.197.69 --open
```

<figure><img src="../.gitbook/assets/2 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

* 通过扫描结果发现了2个运行web的端口：80和8080，80端口运行的是一个nginx的web服务器，8080端口运行的是一个名为NodeBB的CMS Web应用：

<figure><img src="../.gitbook/assets/4 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5 (1).png" alt=""><figcaption></figcaption></figure>

* 分别针对80和8080端口进行隐藏文件/目录扫描，发现8080端口扫出了很多东西：

```bash
dirsearch -u  http://192.168.197.69:8080 -x 403,404,400
```

<figure><img src="../.gitbook/assets/6 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/7 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 同时，8080端口上的robots.txt文件也指向了3个文件/目录：

<figure><img src="../.gitbook/assets/8 (16).png" alt=""><figcaption></figcaption></figure>

* 分别查看后，并没有什么特别的发现。

<figure><img src="../.gitbook/assets/9 (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/10 (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/14 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/15.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/16 (1).png" alt=""><figcaption></figcaption></figure>

* 尝试注册一个新用户，查看其是否有可以被利用的功能，也没有任何收获：

<figure><img src="../.gitbook/assets/12 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/13 (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 查看运行的NodeBB的相关漏洞，因为目前还不知道该应用的具体版本号，所以暂时搁置：

<figure><img src="../.gitbook/assets/11 (13).png" alt=""><figcaption></figcaption></figure>

* 目前看来，两个Web相关的端口没有任何发现。6379端口运行的是Redis 5.0.9，通过查找得到一个远程执行的公开漏洞：

<figure><img src="../.gitbook/assets/17 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/18 (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
因为从exploitDB中搜索出的相关漏洞是使用的Metasploit，本例决定手动执行漏洞利用。
{% endhint %}

### 漏洞利用

* 将redis-rce从Github上下载下来：

```bash
git clone https://github.com/Ridter/redis-rce.git
```

<figure><img src="../.gitbook/assets/19 (1).png" alt=""><figcaption></figcaption></figure>

* 直接执行该脚本，查看其利用方法，得知目前还差一个exp.so文件，该文件是redis的一个恶意模块，下载下来作为参数执行该脚本：

```bash
wget https://github.com/n0b0dyCN/redis-rogue-server/blame/master/exp.so
```

<figure><img src="../.gitbook/assets/20 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/21 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/23 (1).png" alt=""><figcaption></figcaption></figure>

*

















### ROOT









{% hint style="info" %}
本例没有提权过程，Get Shell后即可拿到最高权限，属于简单机器。

（二刷机器，补笔记。本例机器中途重置过，IP地址有变化，不影响漏洞利用及其实现结果）
{% endhint %}
