---
description: EASY - Linux
---

# 👍 DC-1

## 建立立足点

### 信息收集



### 枚举

使用Nmap扫描出了目标系统的4个开放端口：22、80、111、42760

```bash
nmap -sV -sC -p- -oA dc1 192.168.221.193 --open
```

<figure><img src="../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

浏览检查目标系统的Web页面，发现其使用的是Drupal：

<figure><img src="../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

扫描目标80端口的隐藏文件和目录，获得一下有效路径：

```bash
// 方法1：使用dirsearch
dirsearch -u http://192.1688.221.193
// 方法2：使用ffuf（很慢很慢）
ffuf -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt:FUZZ -u http://192.168.221.193/FUZZ -c -e .php,.txt,.html -of html -o 80_defualt_scan.html
```

* http://192.168.221.193/robots.txt
* http://192.168.221.193/xmlrpc.php
* http://192.168.221.193/web.config
* http://192.168.221.193/views/ajax/autocomplete/user/a
* http://192.168.221.193/UPGRADE.txt
* http://192.168.221.193/MAINTAINERS.txt
* http://192.168.221.193/INSTALL.pgsql.txt
* http://192.168.221.193/INSTALL.mysql.txt
* http://192.168.221.193/INSTALL
* http://192.168.221.193/COPYRIGHT.txt

<figure><img src="../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

### 漏洞查找

#### 手工查找

可得知目标系统的Drupal版本为7，至于详细的是7.x无从得知。但是已经足以推测使用Drupalgeddon漏洞是可行的。

<figure><img src="../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

#### 工具查找

工具：droopescan

```bash
droopescan scan drupal -u http://192.168.221.193:80/
```

### 漏洞利用

使用Metasploit对目标建立基本的立足点：

<figure><img src="../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/8.png" alt=""><figcaption></figcaption></figure>

### GET SHELL

<figure><img src="../.gitbook/assets/9.png" alt=""><figcaption></figcaption></figure>



## 提升权限

### 本地信息收集



### 漏洞查找





### 漏洞利用



### ROOT





{% hint style="info" %}
MEMO.


{% endhint %}
