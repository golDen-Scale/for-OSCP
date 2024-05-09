---
description: Easy - Linux：ExifTool 12.23
---

# ✔️ Exghost

## 建立立足点

### 信息枚举

使用Nmap对目标进行开放端口扫描，获得2个开放端口：

<figure><img src="../.gitbook/assets/1 (1).png" alt=""><figcaption></figcaption></figure>

首先对80端口的Web网页进行检查，无收获：

<figure><img src="../.gitbook/assets/2 (1).png" alt=""><figcaption></figcaption></figure>

然后尝试以Anonymous身份登录FTP服务，也没有收获。因此，尝试使用暴力破解，获得登录凭证：<mark style="color:red;">**user:system**</mark>

```bash
// 用Hydra对目标FTP进行暴力破解尝试
hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt ftp://192.168.160.183 -V
```

<figure><img src="../.gitbook/assets/3 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/4 (1).png" alt=""><figcaption></figcaption></figure>

登录FTP后获取到一个backup文件，将它下载到kali本地后，发现它是一个PCAP文件，用Wireshark打开并查看其HTTP流相关数据包：

{% hint style="info" %}
本例中关于目标FTP服务开启了被动模式，因此登录后并不能下载目标文件，需要关闭这个被动模式，才能成功下载。

<img src="../.gitbook/assets/6 (1).png" alt="" data-size="original">

```bash
// 登录FTP后，直接输入：
passive
```
{% endhint %}

<figure><img src="../.gitbook/assets/5 (1).png" alt=""><figcaption></figcaption></figure>















### 漏洞查找



### 漏洞利用



### GET SHELL





## 提升权限

### 本地信息收集





### 漏洞查找





### 漏洞利用





### ROOT





{% hint style="info" %}
MEMO.

* [https://github.com/ly4k/PwnKit/tree/main](https://github.com/ly4k/PwnKit/tree/main)
*
{% endhint %}
