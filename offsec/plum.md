---
description: Linux / 中等难度 / PluXml 5.8.7
---

# ✔️ Plum

## 建立立足点

### 信息收集

* 使用Nmap进行开放端口扫描，获得一个80端口：

```bash
nmap -sC -sV -p- -oA plum 192.168.185.28 --open
```

<figure><img src="../.gitbook/assets/1 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 查看其Web页面发现是一个CMS系统，并且在页面底部发现了管理员登录面板的入口：

<figure><img src="../.gitbook/assets/2 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 简单猜测弱口令，成功登录：

<figure><img src="../.gitbook/assets/3 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 发现了目标系统正在运行的是PluXml 5.8.7的应用程序，下一步查找是否有公开已知漏洞：

<figure><img src="../.gitbook/assets/4 (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 搜索发现当前的pluxml 5.8.7有两个可利用的已知公开漏洞：CVE-2022-25018和CVE-2021-38603，先从最近的漏洞开始尝试：

<figure><img src="../.gitbook/assets/5 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 从CVE-2022-25018的漏洞利用步骤描述得知，可在管理员面板的Static Pages这一项直接编辑页面，插入php代码即可：

<figure><img src="../.gitbook/assets/6 (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/7 (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 将反弹shell的php脚本修改好后，把完整的代码复制粘贴到目标系统的编辑框里：

<figure><img src="../.gitbook/assets/8 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 保存已编辑好的内容，监听好端口，触发页面等待回连：

<figure><img src="../.gitbook/assets/9 (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/10 (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 成功获得反弹shell：

<figure><img src="../.gitbook/assets/11 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/12 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 本例并没有常规的利用系统漏洞进行提权的操作，而是在查找local.txt的过程中意外发现了root账户的明文密码，因此直接切换到root账户，获取2个flag:

<figure><img src="../.gitbook/assets/13 (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/14 (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### ROOT

<figure><img src="../.gitbook/assets/15 (3).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例虽然被划分为中等难度机器，但其实很简单，感觉像是考察对机器内容枚举的细粒度。

CVE-2022-25018
{% endhint %}
