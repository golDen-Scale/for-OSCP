---
description: Easy / 枚举 / 阅读源码 / 图像解析
---

# ✔️ Greenhorn

## 建立立足点

### 信息收集

* 使用Nmap对目标进行开放端口扫描，获得3个开放端口：

```bash
nmap -sC -sV -p- -oA greenhorn 10.129.61.36 --open
```

<figure><img src="../../.gitbook/assets/1 (9).png" alt=""><figcaption></figcaption></figure>

* 将IP和域名添加到/etc/hosts文件后，开始检查80端口上的内容，发现了目标正在运行的软件pluck和admin登录的入口：

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

* 进入admin面板后发现了pluck的版本（4.7.18），随后尝试几个弱口令均失败：

<figure><img src="../../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

* 转到3000端口依次对其内容进行信息收集，发现其正在运行的是Gitea 1.21.11并且还有目标系统的源码：

<figure><img src="../../.gitbook/assets/8 (12).png" alt=""><figcaption></figcaption></figure>

* 其中有几个文件可以重点关注：admin.php / login.php

<figure><img src="../../.gitbook/assets/9 (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/10 (10).png" alt=""><figcaption></figcaption></figure>

* 在`login.php`这个文件中，其源码描述了关于用户登陆验证的过程。其中使用了SHA-512算法加密了密码(未加盐)，这意味着可以使用暴力破解。并且也指明了密码文件是pass.php：

<figure><img src="../../.gitbook/assets/11 (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12 (10).png" alt=""><figcaption></figcaption></figure>

* `pass.php`位于/data/settings/pass.php该路径下，里面有一个hash密码，把它保存为一个hashes.txt文件用于后续暴破：

<figure><img src="../../.gitbook/assets/13 (11).png" alt=""><figcaption></figcaption></figure>

* 在hash example上查找对应的模式，用于hashcat暴破（不确定是哪种）





### 漏洞查阅

* 在信息枚举阶段获取到的Web页面运行的pluck 4.7.18确实是有一个远程代码执行的漏洞，但是在本例中的漏洞利用阶段仅起到了辅助作用，并未直接利用：

<figure><img src="../../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

* 从该漏洞脚本中可以确认其上传文件的类型是zip类型，这为后续上传反弹shell做了提示：

<figure><img src="../../.gitbook/assets/7 (12).png" alt=""><figcaption></figcaption></figure>

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
