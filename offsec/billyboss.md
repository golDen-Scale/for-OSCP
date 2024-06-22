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

* 因为此时已经获得了应用程序的版本号，查阅公开漏洞库发现了可能可以利用的漏洞脚本：

<figure><img src="../.gitbook/assets/10.png" alt=""><figcaption></figcaption></figure>

* 阅读该脚本后，发现几个需要修改的地方：

<figure><img src="../.gitbook/assets/11.png" alt=""><figcaption></figcaption></figure>

* 但是在之前的信息枚举的过程中完全没有发现任何可利用用户凭证信息，所以决定使用Hydra进行爆破：

1. 先用BurpSuite拦截一下POST表单查看路径和用户名、密码处的格式：

<figure><img src="../.gitbook/assets/12.png" alt=""><figcaption></figcaption></figure>

2. 用户名密码处用Base64编码了，在构造hydra命令时应注意用:username=<mark style="color:red;">**^USER64^**</mark>spassw0rd=<mark style="color:red;">**^PASS64^**</mark>：

```bash
# 使用Hydra的爆破时间实在是太长，需要准备一些更好的username和password字典文件
hydra -L /usr/share/secLists/Usernames/Names/names.txt -P /usr/share/SecLists/Passwords/common-credentials/10k-most-common.txt
192.168.250.61 -s 8081 http-post-form '/service/rapture/session:username=^USER64^spassw0rd=^PASS64^:Forbidden‘
```

* 因考虑到Hydra爆破耗时漫长，所以与此同时决定猜测弱口令，简单猜测几次后发现了有效凭证_<mark style="color:red;">**nexus:nexus  😄**</mark>_

<figure><img src="../.gitbook/assets/13.png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 此时已具备了可以修改漏洞脚本的基本项，还





### GET SHELL









## 权限提升

### 本机信息枚举









### 漏洞查阅







### 漏洞利用









### ROOT





{% hint style="info" %}
本例算中等偏难的机器，锁定可利用的漏洞和提权时都不难，但是其实现过程中需要根据实际情况进行修改和变通。
{% endhint %}
