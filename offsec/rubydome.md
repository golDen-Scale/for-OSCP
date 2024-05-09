---
description: Linux - Easy：PDFKit
---

# ✔️ RubyDome

## 建立立足点

### 信息枚举

使用Nmap进行开放端口扫描，获得2个开放端口：22和3000

<figure><img src="../.gitbook/assets/1 (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例的“兔子洞”：

<img src="../.gitbook/assets/3 (2).png" alt="" data-size="original">
{% endhint %}

查看3000端口上的Web页面，发现目标是一个可将HTML页面转换为PDF格式的应用：

<figure><img src="../.gitbook/assets/2 (2).png" alt=""><figcaption></figcaption></figure>

尝试随意输入一个URL（http://127.0.0.1），来触发目标应用抛出错误信息，以进一步收集信息：

<figure><img src="../.gitbook/assets/4 (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5 (2).png" alt=""><figcaption></figcaption></figure>

### 漏洞查找

查找关于PDFKit相关的已知、公开漏洞，得到：

<figure><img src="../.gitbook/assets/6 (2).png" alt=""><figcaption></figcaption></figure>

本例中，虽为从错误信息中看到任何版本号，但是从初步查找到的公开漏洞的描述信息可得知CVE-2022-25765是针对PDFKit 0.0.0到0.8.7.2版本都适用的，因此可以尝试。

<figure><img src="../.gitbook/assets/7 (2).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

```bash
// 在ExploitDB中查找利用脚本：
searchsploit PDFKit
// 下载利用脚本到Kali本地：
searchsploit -m 51293.py .
```

<figure><img src="../.gitbook/assets/8 (2).png" alt=""><figcaption></figcaption></figure>

查看该脚本的使用方法，供后续构造命令：

<figure><img src="../.gitbook/assets/10 (2).png" alt=""><figcaption></figcaption></figure>

* 参数 -s：设置Kali本机IP和监听的端口
* 参数 -w：设置目标地址和路径（查看目标页面的HTML源码）
* 参数 -p：设置POST的具体参数（查看目标页面的HTML源码）

<figure><img src="../.gitbook/assets/9 (2).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

```bash
// 在Kali上监听端口
nc -lvnp 4444
// 
```



## 提升权限

### 本地信息枚举





### 漏洞查找





### 漏洞利用





### ROOT







