---
description: Easy / 枚举 /
---

# ✔️ Sauna

## 建立立足点

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA sauna 10.129.77.184 --open
```

<figure><img src="../../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

* 发现开放目标系统开放了80端口，查看后并没有发现什么特别有用的信息，但是发现了很多用户名，先收集起来也许后续会有用：

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

* 对该80端口进行常规的隐藏文件/目录扫描，没什么收获：

```bash
gobuster dir -u http://10.129.77.184/ -w /usr/share/wordlists/dirb/common.txt
```

<figure><img src="../../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

* dirsearch也没有什么收获：

<figure><img src="../../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>





## 权限提升



