---
description: Easy / 枚举 / 命名管道 / 永恒之蓝
---

# Blue

## 建立立足点

### 信息收集

* 使用Nmap对目标进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA blue 10.129.223.191 --open
```

<figure><img src="../../.gitbook/assets/1 (23).png" alt=""><figcaption></figcaption></figure>

* 由于本例并没有开放的Web应用的端口，所以直接无密码查看SMB共享：

<figure><img src="../../.gitbook/assets/2 (23).png" alt=""><figcaption></figcaption></figure>

* 比较值得注意的是/Share和/Users，空密码登录到/Users和/Share中，并没有什么值得关注的内容：

```bash
smbclient  //10.129.223.191/Users
smbclient  //10.129.223.191/Share
```

<figure><img src="../../.gitbook/assets/3 (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (24).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6 (23).png" alt=""><figcaption></figcaption></figure>

* 因为本例既没有开放的80端口，检查smb服务时也没有什么特别的发现，所以重新进行信息枚举（HTB的机器名字通常能起到提示作用）：

```bash
nmap -p 445 --script smb-vuln-* 10.129.223.191
```

<figure><img src="../../.gitbook/assets/7 (24).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 果然重新扫描后发现存在永恒之蓝（MS17-010）漏洞，查找相关漏洞利用：

```
searchsploit ms17-010
```

<figure><img src="../../.gitbook/assets/8 (23).png" alt=""><figcaption></figcaption></figure>

* 本例选择利用42315.py这个脚本：

<figure><img src="../../.gitbook/assets/9 (22).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 在利用该脚本的过程中，从返回的信息中得知还需要一个名为mysmb的模块：

<figure><img src="../../.gitbook/assets/10 (24).png" alt=""><figcaption></figcaption></figure>

* 在用命令行安装过程中始终不成功，所以此处我直接下载了ZIP包到Kali本机，解压后将mysmb.py脚本复制到了pyhton3的dist-packages中，随后该脚本运行成功：

```
https://github.com/worawit/MS17-010
```

<figure><img src="../../.gitbook/assets/11 (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12 (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/13 (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/14 (22).png" alt=""><figcaption></figcaption></figure>

* 但是运行后返回的信息又说明没有找到可访问的命名管道，所以此处借由Metasploit的辅助模块来查找目标主机上的任何命名管道：

```bash
msfconsole
search smb pipe auditor
use 0
show options
set RHOSTS 10.129.223.191
run
```

<figure><img src="../../.gitbook/assets/15 (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/16 (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/17 (19).png" alt=""><figcaption></figcaption></figure>

* 很遗憾没有扫出来任何命名管道

### GET SHELL

*











## 权限提升

### 本地信息收集









### 漏洞查阅









### 漏洞利用









### ROOT









{% hint style="info" %}
本例为2刷机器，第一遍用的是Metasploit完成的，所以确实属于简单机器。2刷手动利用的过程不算简单，用Metasploit的辅助模块在考试中是被允许的。
{% endhint %}
