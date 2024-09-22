---
icon: check
description: Hard / 枚举 / DC / Kerberos票据伪造 /
---

# Mantis

## 建立立足点

### 信息收集

* 使用nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA mantis 10.129.40.77 --open
```

<figure><img src="../../.gitbook/assets/1 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/3 (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (11).png" alt=""><figcaption></figcaption></figure>

* 根据上面扫描获得的信息，将域名添加到/etc/hosts文件中：

<figure><img src="../../.gitbook/assets/5 (11).png" alt=""><figcaption></figcaption></figure>

* 因为扫描出了8080端口，先查看其内容发现是一个运行着名为Orchard的CMS系统：

<figure><img src="../../.gitbook/assets/6 (11).png" alt=""><figcaption></figcaption></figure>

* 页面底部发现登录入口，简单尝试几个弱口令均失败，暂时搁置：

<figure><img src="../../.gitbook/assets/7 (14).png" alt=""><figcaption></figcaption></figure>

* 使用dirsearch扫描8080端口上的隐藏文件/目录：

```bash
dirsearch -u  http://10.129.111.62:8080 -x 403,404,400
```

<figure><img src="../../.gitbook/assets/8 (15).png" alt=""><figcaption></figcaption></figure>

* 没有什么特别的收获：

<figure><img src="../../.gitbook/assets/9 (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/10 (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/11 (12).png" alt=""><figcaption></figcaption></figure>

* 接下来尝试匿名登录一下目标系统的SMB服务，均无收获：

```bash
smbclient -N -L //10.129.208.146
smbmap -H 10.129.208.146
```

<figure><img src="../../.gitbook/assets/12 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/13 (13).png" alt=""><figcaption></figcaption></figure>

* 尝试使用enum4linux枚举一下，无收获：

```bash
enum4linux 10.129.208.146
```

<figure><img src="../../.gitbook/assets/14 (11).png" alt=""><figcaption></figcaption></figure>

* 重新查看Nmap的输出信息，还发现了一个运行着IIS7 Web服务器的端口：1337，此时可考虑换gobuster重新扫描一下8080端口和1337端口上的隐藏文件/目录：

```bash
gobuster dir -u http://10.129.15.72:8080 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -q
gobuster dir -u http://10.129.15.72:1337 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -q
```

{% hint style="info" %}
dirsearch更快但是会有遗漏，gobuster更详细但是很耗时。
{% endhint %}



















### GET SHELL











## 权限提升

### 本地信息收集













### ROOT











{% hint style="info" %}
本例在初步扫描阶段从开放的端口来看，基本判定为域控机器，8080端口算是个小小的“兔子洞”，还涉及到枚举、解码、Kerberos票据伪造攻击，确实是属于困难级别。

(本例机器中途重置过，IP地址有所改变，不影响其利用过程和实现结果)
{% endhint %}
