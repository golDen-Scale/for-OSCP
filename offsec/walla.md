---
description: Linux / 中等难度 / RaspAP v2.5 / CVE-2021-4034
---

# ✔️ Walla

## 建立立足点

### 信息枚举

* 使用Nmap对目标系统开放端口进行扫描，获得如下开放端口及其版本信息：

<figure><img src="../.gitbook/assets/1 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 发现一个8091的开放端口运行着HTTP服务，查看后是一个登录页面，但是目前没有任何有用凭证和该页面运行的是什么应用的信息：

<figure><img src="../.gitbook/assets/2 (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 先对该端口进行扫描，看看是否还有什么隐藏目录/文件：

```bash
dirsearch dir -u http://192.168.180.97:8091 -i 200,300-399
```

<figure><img src="../.gitbook/assets/3 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 发现一些配置文件可读，查看后得知都指向了一个应用程序RaspAP：

<figure><img src="../.gitbook/assets/4 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 尝试搜索该应用的默认凭证，发现是<mark style="color:red;">**admin:secret**</mark>

<figure><img src="../.gitbook/assets/6 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 登录成功，并且发现其版本号信息：

<figure><img src="../.gitbook/assets/7 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/8 (6).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 初步搜索发现没有合适的漏洞可以利用：

<figure><img src="../.gitbook/assets/9 (4).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 又回到应用程序本身，发现其system下有一个console的终端控制台，直接有了一个shell：

<figure><img src="../.gitbook/assets/10 (4).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 回连Kali，并获取一个完整交互式shell，方便后续操作：

<figure><img src="../.gitbook/assets/11 (3).png" alt=""><figcaption></figcaption></figure>

* 获得第一个flag：

<figure><img src="../.gitbook/assets/12 (3).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 先简单的手工收集一下本机可能的可利用的弱点，发现有几个可以不用密码就能执行ROOT权限的文件，其中有一个wifi\_reset.py就在当前我所处的/walter目录下，并且当前我对该目录有完整的执行权限：

<figure><img src="../.gitbook/assets/13 (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/14 (3).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 本想着直接写一个相同文件名的脚本直接覆盖原文件，但是发现无法覆盖，因此此方法行不通：

<figure><img src="../.gitbook/assets/15 (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/16 (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/17 (3).png" alt=""><figcaption></figcaption></figure>

* 上工具，用linpeas对目标系统进行信息收集：

<figure><img src="../.gitbook/assets/18 (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/19 (3).png" alt=""><figcaption></figcaption></figure>

* 发现了几个可以利用的漏洞，从最近的开始尝试（CVE-2021-4034）：

<figure><img src="../.gitbook/assets/20 (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/21 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 阅读该脚本后发现不需要修改什么东西，将该漏洞的利用脚本上传到目标机器中直接执行即可获得shell：

<figure><img src="../.gitbook/assets/22 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### ROOT

* root，获得flag：

<figure><img src="../.gitbook/assets/23 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/24 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例getshell阶段并非利用的漏洞，而是应用程序本身的功能；权限提升阶段需要详细枚举本机信息，查找有写入权限的目录或文件。

CVE-2021-4034
{% endhint %}
