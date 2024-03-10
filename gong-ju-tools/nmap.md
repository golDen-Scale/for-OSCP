---
description: 信息收集、信息枚举、漏洞检测
---

# ✔️ Nmap

## 信息收集

* 扫描目标IP的所有开放端口的版本号&正在运行的服务名称，并生成输出文件

<pre class="language-sh"><code class="lang-sh"><strong>nmap -sV -sC -p- -oA 输出文件名 [Target IP] --open
</strong></code></pre>

*



## SMB枚举

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

*



