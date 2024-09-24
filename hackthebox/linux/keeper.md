---
description: Easy /
---

# ✔️ Keeper

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA keeper 10.129.229.41 --open
```

{% hint style="info" %}
扫描太慢了......
{% endhint %}

* 在扫描的同时可尝试查看目标的80端口上是否有内容，发现有一个链接，指向域名tickets.keeper.htb，将其添加到hosts文件里：

<figure><img src="../../.gitbook/assets/1 (16).png" alt=""><figcaption></figcaption></figure>

* 再次点击链接，会跳转到[http://tickets.keeper.htb/rt/](http://tickets.keeper.htb/rt/)，一个登录界面：

<figure><img src="../../.gitbook/assets/2 (13).png" alt=""><figcaption></figcaption></figure>

* 该界面底部有当前正在运行的服务及其版本号：RT 4.4.4，尝试了几个弱口令均失败















### 漏洞查阅









### 漏洞利用









### GET SHELL











## 权限提升

### 本地信息收集







### 漏洞利用









### ROOT







{% hint style="info" %}

{% endhint %}
