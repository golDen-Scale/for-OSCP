---
description: Easy - Linux / changedetection v0.45.1 / CVE-2024-32651
---

# ✔️ Detection

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描，获取到2个端口：22和5000

```bash
nmap -sC -sV -oA detection 192.168.124.97 --open
```

<figure><img src="../../.gitbook/assets/1 (36).png" alt=""><figcaption></figcaption></figure>

* 检查5000端口上的内容，找到了当前运行的程序及其版本号：**changedetection v0.45.1**

<figure><img src="../../.gitbook/assets/2 (34).png" alt=""><figcaption></figcaption></figure>

* 枚举5000端口上的隐藏文件/目录，找到4个目录并依次查看：

```bash
dirsearch -u http://192.168.124.97 -x 404,403,302,301
```

<figure><img src="../../.gitbook/assets/3 (31).png" alt=""><figcaption></figcaption></figure>

* 在/backup中发现了一个压缩包，自动下载后解压：

```bash
unzip changedetection-backup-20250130031700.zip
```

<figure><img src="../../.gitbook/assets/4 (32).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (32).png" alt=""><figcaption></figcaption></figure>

* 本例兔子洞是secret.txt文件中的字符串解码，没有任何收获：

<figure><img src="../../.gitbook/assets/6 (32).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 查询changedetection 0.45.1，发现有一个远程代码执行的漏洞可以利用： https://www.exploit-db.com/exploits/52027

<figure><img src="../../.gitbook/assets/8 (32).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7 (32).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 将该脚本下载到Kali本地，直接执行即可获取到拥有root权限的shell：

<figure><img src="../../.gitbook/assets/9 (31).png" alt=""><figcaption></figcaption></figure>
