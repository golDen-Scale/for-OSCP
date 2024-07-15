---
description: Hard / 枚举
---

# ✔️ Search

## 建立立足点

## 信息收集

* 使用Nmap针对目标进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA search 10.129.65.154 --open
```



* 目标开放了80 端口，使用gobuster扫描隐藏目录/文件：

```bash
gobuster dir -u http://10.129.65.154/ -w /usr/share/wordlists/dirb/common.txt
```

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

有几个目录值得关注一下：/staff、/certenroll、/certsrv、/images

* 初步查看网页，找到一些用户名，可以先收集起来，后续也许有用：

<figure><img src="../../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

*



















### GET SHELL





## 权限提升

### 本地信息收集









### ROOT







