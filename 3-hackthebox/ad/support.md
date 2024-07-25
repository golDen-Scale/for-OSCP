---
description: Easy / 分析程序文件 / 枚举 /
---

# ✔️ Support

## 建立立足点

### 信息收集

* 使用Nmap对目标开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA support 10.129.230.181 --open
```

<figure><img src="../../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

* 先进行SMB匿名登录测试，发现可以登录上去，且有感兴趣的共享文件/support-tools（这种和靶机名称一致的提示还是蛮明显的）：

```bash
smbclient -N -L 10.129.230.181
```

<figure><img src="../../.gitbook/assets/3 (9).png" alt=""><figcaption></figcaption></figure>

* 分别尝试把这些共享文件递归下载到本地，也发现只有/support-tools的可以下载，其他的都拒绝：

```bash
smbclient //10.129.230.181/support-tools
RECURSE ON
PROMPT OFF
mget *
```

<figure><img src="../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

* 下载到Kali本地后，查看发现都是exe文件和一个压缩包文件，名称是UserInfo，猜测可能包含用户信息：

<figure><img src="../../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

*















### GET SHELL







## 权限提升

### 本地信息收集













### ROOT





















{% hint style="info" %}
本例虽然是被归类为简单机器，但是涉及到自己的盲区较多，看的提示较多完成度不高，对我来讲算难的机器。
{% endhint %}
