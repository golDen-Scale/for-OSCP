---
description: 信息收集 / 信息枚举 / 漏洞检测
---

# ✔️ Nmap

## 信息收集

<pre class="language-sh"><code class="lang-sh"><strong>// OSCP常用
</strong><strong>nmap -sV -sC -p- -oA 输出文件名 [Target IP] --open
</strong></code></pre>

## 指纹识别&版本探测

```bash
// Some code
```



## 渗透测试

### 主机发现

```bash
// 用于扫描一个目标的子网并提取活动主机的 IP 地址
nmap 目标IP/24 -sn -oA tnet | grep for | cut -d" " -f5

```

### SMB枚举

```bash
ls -al /usr/share/nmap/scripts/ | grep -e "smb"

nmap -p 445 --script smb-os-discovery 目标IP
nmap -p 445 --script smb-enum-shares 目标IP
nmap -p 445 --script smb-enum-users 目标IP -d
nmap -p 445 --script smb-protocols 目标IP
nmap -p 445 --script smb-double-pulsar-backdoor 目标IP
nmap -p 445 --script smb-vuln-ms17-010 目标IP
nmap -p 445 --script=smb-vuln* --script-args=unsafe=1 目标IP
```















## 漏洞检测

```bash
// Some code
```





## 数据库

```bash
// Some code
```





## 其他（非OSCP内容）

### 防火墙、IDS逃逸

```bash
// 隐形扫描（-sS/-sN/-sF/-sX）
nmap -sS -p- -sV -oA 文件名 目标IP
// 全端口+低速+隐形+详细探测+多混淆
nmap -sS -p- -sV -A --data-length 20 --randomize-hosts --badsum --spoof-mac Cisco -T0 -Pn -oN 输出.txt 目标IP

```





{% hint style="info" %}
隐形扫描不隐形，请在获得授权的情况下进行测试。
{% endhint %}
