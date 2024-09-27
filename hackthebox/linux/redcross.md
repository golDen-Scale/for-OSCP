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

* 尝试几个弱口令均失败后，使用dirsearch扫描是否有隐藏文件/目录，没什么收获：

```bash
dirsearch -u  http://10.129.9.55 -x 403,404,400
```

<figure><img src="../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

* 使用Wfuzz尝试扫描子域，发现有两个结果：

```bash
wfuzz -c -u https://intra.redcross.htb -H "Host:FUZZ.redcross.htb" -w subdomains-top1million-110000.txt --hc 404 -t 200 --hl
```

<figure><img src="../../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

* 将以上子域也添加到/hosts文件中，再访问admin这个子域就获得了一个管理登录界面，简单尝试几个弱口令均失败：

<figure><img src="../../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

* 找到的两个登录界面也没有查看到所使用的公开已知的软件信息，因此无法通过常规的搜寻公开已知漏洞进行利用。在当前没有任何有效凭证的情况下，尝试暴力破解无果。决定从头枚举，发现遗漏了对443端口的扫描：

```bash
dirbuster -u https://intra.redcross.htb:443 -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -r
```

*

















### 漏洞利用









### GET SHELL











## 权限提升

### 本地信息收集







### 漏洞利用









### ROOT









{% hint style="info" %}


(本例机器中途重置过，IP地址有变化，但不影响漏洞利用及其实现结果)
{% endhint %}
