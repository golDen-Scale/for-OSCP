---
icon: check
description: Hard /
---

# Mantis

## 建立立足点

### 信息收集

* 使用nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA mantis 10.129.40.77 --open
```

<figure><img src="../../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/3 (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (11).png" alt=""><figcaption></figcaption></figure>

* 根据上面扫描获得的信息，将域名添加到/etc/hosts文件中：

<figure><img src="../../.gitbook/assets/5 (11).png" alt=""><figcaption></figcaption></figure>

* 因为扫描出了8080端口，先查看其内容发现是一个运行着名为Orchard的CMS系统：

<figure><img src="../../.gitbook/assets/6 (11).png" alt=""><figcaption></figcaption></figure>

























### GET SHELL











## 权限提升

### 本地信息收集













### ROOT











{% hint style="info" %}
(本例机器中途重置过，IP地址有所改变，不影响其利用过程和实现结果)
{% endhint %}
