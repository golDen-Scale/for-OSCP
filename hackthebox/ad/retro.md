---
description: DC / SMB枚举 / ESC1 / Easy
---

# ✔️ Retro

## 建立立足点

### 1. 基本信息收集&枚举：

* 端口扫描，从开启的端口判断该主机为域控机器：

```bash
nmap -sC -sV -p- -oA retro 10.129.234.44 --open 
```







* 将域名写入/hosts文件：

```bash
echo '10.129.13.15   DC.retro.vl retro.vl' > /etc/hosts
```



### 2. 服务枚举：

#### SMB服务：





#### 其他服务：





### 2. Get Shell：













## 权限提升

### 1. 域内信息收集&枚举：











### 2. ROOT：











{% hint style="info" %}

{% endhint %}
