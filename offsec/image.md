# ✔️ Image

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA image 192.168.210.178 --open
```

<figure><img src="../.gitbook/assets/1 (14).png" alt=""><figcaption></figcaption></figure>

* 首先查看80端口的Web页面进行检查，是一个有上传功能的界面，猜测可能是一个文件上传漏洞：

<figure><img src="../.gitbook/assets/2 (12).png" alt=""><figcaption></figcaption></figure>

* 根据尝试上传图片的返回信息，目标系统只接受后缀为JPEG和PNG格式的图片文件：

<figure><img src="../.gitbook/assets/3 (13).png" alt=""><figcaption></figcaption></figure>

* 上传成功一个测试图片后，发现返回了当前正在运行的ImageMagick的版本号：6.9.6-4

<figure><img src="../.gitbook/assets/4 (13).png" alt=""><figcaption></figcaption></figure>

* 搜索公开已知的漏洞发现有一个命令注入的漏洞：

<figure><img src="../.gitbook/assets/5 (13).png" alt=""><figcaption></figcaption></figure>

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
