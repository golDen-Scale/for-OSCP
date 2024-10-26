---
description: Linux /
---

# ✔️ Editorial

## 建立立足点

### 信息收集

* 使用Nmap对目标系统开放端口进行信息收集，获得2个开放端口：22，80

<figure><img src="../../.gitbook/assets/1 (34).png" alt=""><figcaption></figcaption></figure>

* 将输出信息中获取到的域名加入到hosts文件中：

<figure><img src="../../.gitbook/assets/2 (32).png" alt=""><figcaption></figcaption></figure>

* 先查看80端口上的Web页面内容，使用dirsearch和简单的手动枚举没有什么发现：

<figure><img src="../../.gitbook/assets/3 (29).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6 (30).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (30).png" alt=""><figcaption></figcaption></figure>

* 逐一检查各个页面，发现了可以进行文件上传的地方，猜测是与文件上传漏洞相关：

<figure><img src="../../.gitbook/assets/4 (30).png" alt=""><figcaption></figcaption></figure>

* 还有一个邮箱地址，先收集下来：

<figure><img src="../../.gitbook/assets/7 (30).png" alt=""><figcaption></figcaption></figure>

* 在upload这个页面中，尝试上传反弹shell的脚本，需按实际情况修改自己的IP和监听端口：

<figure><img src="../../.gitbook/assets/8 (30).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/10 (30).png" alt=""><figcaption></figcaption></figure>

* 上传后，没有任何反应。





















### 漏洞利用





### GET SHELL







## 权限提升

### 本地信息收集







### 漏洞查阅







### 漏洞利用







### ROOT























{% hint style="info" %}

{% endhint %}

