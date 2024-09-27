# ✔️ RedCross

## 建立立足点

### 信息收集

* 使用Nmap进行对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA redcross 10.129.9.55 --open
```

<figure><img src="../../.gitbook/assets/1 (20).png" alt=""><figcaption></figcaption></figure>

* 检查80端口上的内容，发现它重定向到https://intra.redcross.htb/这个地址，将域名添加到/hosts文件中，然后再次访问，获得一个登录页面：

<figure><img src="../../.gitbook/assets/2 (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/3 (18).png" alt=""><figcaption></figcaption></figure>

* 尝试几个弱口令均失败后，使用dirsearch扫描是否有隐藏文件/目录：

```bash
dirsearch -u  http://10.129.9.55 -x 403,404,400
```

<figure><img src="../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

*











### 漏洞查阅









### 漏洞利用









### GET SHELL











## 权限提升

### 本地信息收集







### 漏洞利用









### ROOT









{% hint style="info" %}

{% endhint %}
