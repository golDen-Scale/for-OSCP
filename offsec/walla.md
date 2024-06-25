---
description: Linux / 中等难度 / RaspAP v2.5 / CVE-2021-4034
---

# ✔️ Walla

## 建立立足点

### 信息枚举

* 使用Nmap对目标系统开放端口进行扫描，获得如下开放端口及其版本信息：

<figure><img src="../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

* 发现一个8091的开放端口运行着HTTP服务，查看后是一个登录页面，但是目前没有任何有用凭证和该页面运行的是什么应用的信息：

<figure><img src="../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

* 先对该端口进行扫描，看看是否还有什么隐藏目录/文件：

```bash
dirsearch dir -u http://192.168.180.97:8091 -i 200,300-399
```

<figure><img src="../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

* 发现一些配置文件可读，查看后得知都指向了一个应用程序RaspAP：

<figure><img src="../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

* 尝试搜索该应用的默认凭证，发现是<mark style="color:red;">**admin:secret**</mark>

<figure><img src="../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

* 登录成功，并且发现其版本号信息：

<figure><img src="../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/8 (6).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅







### 漏洞利用







### GET SHELL









## 权限提升

### 本地信息收集







### 漏洞查阅







### 漏洞利用







### ROOT









{% hint style="info" %}
本例getshell阶段并非利用的漏洞，而是应用程序本身的功能；权限提升阶段需要详细枚举本机信息，查找有写入权限的目录或文件。

CVE-2021-4034
{% endhint %}
