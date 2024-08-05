---
description: Easy / 枚举 /
---

# ✔️ Greenhorn

## 建立立足点

### 信息收集

* 使用Nmap对目标进行开放端口扫描，获得3个开放端口：

```bash
nmap -sC -sV -p- -oA greenhorn 10.129.61.36 --open
```

<figure><img src="../../.gitbook/assets/1 (9).png" alt=""><figcaption></figcaption></figure>











### 漏洞查阅







### 漏洞利用







### GET SHELL





## 权限提升

### 本地信息收集





### 漏洞查阅&#x20;







### 漏洞利用







### ROOT















{% hint style="info" %}
本例Get Shell阶段枚举、阅读源码和对各个上传点的尝试是重点，提权部分不算典型的OSCP机型，图像解析的部分更偏向于CTF。

(本例机器中途重置过，IP地址有改变，但不影响其利用过程和实现结果)
{% endhint %}
