---
description: EASY - Linux：Drupal 7（Drupalgeddon）
---

# ✔️ DC-1

## 建立立足点

### 信息枚举

使用Nmap扫描出了目标系统的4个开放端口：22、80、111、42760

```bash
nmap -sV -sC -p- -oA dc1 192.168.221.193 --open
```

<figure><img src="../.gitbook/assets/1 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

浏览检查目标系统的Web页面，发现其使用的是Drupal：

<figure><img src="../.gitbook/assets/2 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/3 (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/4 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查找

#### 手工查找

可得知目标系统的Drupal版本为7，至于详细的是7.x无从得知。但是已经足以推测使用Drupalgeddon漏洞是可行的。

<figure><img src="../.gitbook/assets/5 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 工具查找

工具：droopescan

```bash
droopescan scan drupal -u http://192.168.221.193:80/
```

### 漏洞利用

使用Metasploit对目标建立基本的立足点：

<figure><img src="../.gitbook/assets/6 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/7 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/8 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

<figure><img src="../.gitbook/assets/9 (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



## 提升权限

### 本地信息收集

```bash
// 常规检查
sudo -l
uname -a
// 本例中，因很快就尝试检查SUID/GUID，所以也就很快找到利用点：
find / -perm -u=s -type f 2>/dev/null
```

<figure><img src="../.gitbook/assets/10 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查找

将尝试在GTFOBins中查找find和exim4（备选）：

<figure><img src="../.gitbook/assets/11 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



### 漏洞利用

本例中，虽然是利用的SUID的漏洞，但我发现可以直接在目标系统中使用shell的payload进行提权操作，此方法没有错误输出，也不需要重新用Metasploit连接shell会话，可直接得到root shell：

<figure><img src="../.gitbook/assets/12 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### ROOT

获取到了local.txt：

<figure><img src="../.gitbook/assets/13 (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

获取到了proof.txt：

<figure><img src="../.gitbook/assets/14 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
MEMO.


{% endhint %}
