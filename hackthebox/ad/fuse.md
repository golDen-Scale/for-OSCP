---
description: Medium / 枚举 / 密码喷洒 / 打印机日志记录
---

# ✔️ Fuse

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA fuse 10.129.2.5 --open
```

<figure><img src="../../.gitbook/assets/1 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 将域名和IP写入/etc/hosts文件：

<figure><img src="../../.gitbook/assets/3 (1).png" alt=""><figcaption></figcaption></figure>

* 因为扫描结果中有一个80端口，先查看其内容后发现当前目标运行的是一个打印机日志记录的应用PaperCut，并且还发现了一组用户名，先收集下来后续可能会用到：

<figure><img src="../../.gitbook/assets/4 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (13).png" alt=""><figcaption></figcaption></figure>

* 使用enum4linux进行信息收集没有什么有用的收获：

```bash
enum4linux 10.129.2.5
```

<figure><img src="../../.gitbook/assets/9 (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/10 (11).png" alt=""><figcaption></figcaption></figure>

* 检查SMB的无密码登录，可以成功登录但还是无收获：

```bash
smbclient -N -L //10.129.2.5
```

<figure><img src="../../.gitbook/assets/11 (10).png" alt=""><figcaption></figcaption></figure>













### GET SHELL











## 权限提升

### 本地信息收集













### ROOT













{% hint style="info" %}

{% endhint %}
