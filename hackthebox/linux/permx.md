---
description: Linux / Easy / chamilo 1.11
---

# ✔️ permX

## 建立立足点

### 信息收集

* 使用Nmap对目标开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA permx 10.129.76.98 --open
```

<figure><img src="../../.gitbook/assets/1 (6).png" alt=""><figcaption></figcaption></figure>

* 有个开放端口80，登录其页面查看详情，发现是一个线上学习网站：

<figure><img src="../../.gitbook/assets/2 (6).png" alt=""><figcaption></figcaption></figure>

* 使用dirsearch工具对80端口扫描以枚举隐藏文件/目录，结果无收获：

```bash
dirsearch -u http://permx.htb/ -x 403,404,400
```

<figure><img src="../../.gitbook/assets/4 (7).png" alt=""><figcaption></figcaption></figure>

* 再试试wfuzz对其子域名进行枚举，看看还有没有其他页面：

```bash
wfuzz -c -u http://permx.htb -H "Host:FUZZ.permx.htb" -w subdomains-top1million-110000.txt --hc 404 -t 200 --hl 9
```

<figure><img src="../../.gitbook/assets/5 (7).png" alt=""><figcaption></figcaption></figure>

* 找到2个子域，分别登录查看后发现，lms.permx.htb显示的是名为chamilo应用程序的登录界面，并在页面底部返现了管理员的用户名和邮箱（**Administrator : Davis Miller**）：

<figure><img src="../../.gitbook/assets/6 (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7 (8).png" alt=""><figcaption></figcaption></figure>

* 先将域名及其子域名和IP都写进hosts文件中：

<figure><img src="../../.gitbook/assets/3 (6).png" alt=""><figcaption></figcaption></figure>

*

















### 漏洞查阅







### 漏洞利用







### GET SHELL







## 权限提升

### 本地信息枚举





### 漏洞查阅







### 漏洞利用







### ROOT











{% hint style="info" %}

{% endhint %}
