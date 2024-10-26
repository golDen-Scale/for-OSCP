---
description: Hard / 枚举 /
---

# ✔️ Search

## 建立立足点

## 信息收集

* 使用Nmap针对目标进行开放端口扫描，发现如下端口：

```bash
nmap -sC -sV -p- -oA search 10.129.65.154 --open
```

<figure><img src="../../.gitbook/assets/7 (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (19).png" alt=""><figcaption></figcaption></figure>

* 目标开放了80 端口，使用gobuster扫描隐藏目录/文件：

```bash
gobuster dir -u http://10.129.65.154/ -w /usr/share/wordlists/dirb/common.txt
```

<figure><img src="../../.gitbook/assets/2 (10).png" alt=""><figcaption></figcaption></figure>

* 有几个目录值得关注一下：/staff、/certenroll、/certsrv、/images，分别尝试后都是拒绝访问：

<figure><img src="../../.gitbook/assets/3 (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (21).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (22).png" alt=""><figcaption></figcaption></figure>

* 在/certsrv页面发现登录框，尝试几个弱口令后均失败，暂时放下：

<figure><img src="../../.gitbook/assets/6 (21).png" alt=""><figcaption></figcaption></figure>

* 初步查看网页，找到一些用户名，可以先收集起来，后续也许有用：

<figure><img src="../../.gitbook/assets/1 (8).png" alt=""><figcaption></figcaption></figure>

* 使用smbcclient可以匿名登录，但是没有收获：

```bash
smbclient -N -L 10.129.65.154
```

<figure><img src="../../.gitbook/assets/9 (18).png" alt=""><figcaption></figcaption></figure>

* 尝试使用crackmapexec工具进行匿名登录SMB服务时，找到了主机名RESEARCH：

<figure><img src="../../.gitbook/assets/10 (21).png" alt=""><figcaption></figcaption></figure>

* 使用enum4linux枚举所有可收集到的内容，没找到什么特别有用的，获取到了域名和域SID：

```bash
enum4linux 10.129.65.154
```

<figure><img src="../../.gitbook/assets/11 (19).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12 (18).png" alt=""><figcaption></figcaption></figure>

* 将收集到的用户名及其变种写法全部归纳到一个username.txt文件中，使用Kerbrute暴破出了3个有效用户名：

```bash
./kerbrute_linux_amd64 userenum --dc 10.129.229.57 -d search.htb username.txt
```

<figure><img src="../../.gitbook/assets/13 (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/14 (17).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
之后实在枚举不出来什么信息了，至此还没找到任何有效凭证。看了网上的提示后，发现一个有效凭证 <mark style="color:red;">**Hope Sharp:IsolationIsKey？**</mark>真的是无语...😑😑😑
{% endhint %}

<figure><img src="../../.gitbook/assets/15 (17).png" alt=""><figcaption></figcaption></figure>

*



















### GET SHELL





## 权限提升

### 本地信息收集









### ROOT







{% hint style="info" %}
本例机器中途重置过，因此IP地址有所变化，不影响利用过程及其结果。
{% endhint %}
