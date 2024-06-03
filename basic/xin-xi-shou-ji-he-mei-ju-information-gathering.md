---
description: 是为了找到有公开已知漏洞的服务，作为进入目标的切入点。
---

# ✔️ 信息收集和枚举 - Information Gathering

## 主动信息收集

* 利用工具或手工收集，会与目标直接交互，可能会被目标发现

### 工具

#### iptables

```bash
// Some code
```

#### Nmap

```bash
// 开放端口
nmap -Pn -sC -sV -p- -oN full 10.10.xx.xxx
nmap -sV -sC -p- -oA full 192.168.xxx.xxx --open
```

#### Netcat

```bash
// TCP扫描
nc -nvv -w 1  -z 192.168.xx.xxx 3388--3390
// UDP扫描
nc -nv -u -z -w 1 192.168.xx.xxx 160-162
```

#### Masscan

* 适用于扫整个网络

```bash
// 扫描整个网络中的各个机器的80端口
masscan -o80 10.0.0.0/8
// 扫描整个网段的80端口，且指定了路由IP
masscan -p80 10.11.1.0/24 --rate=1000 -e tap0 --router-ip 10.11.0.1
```

### 方法

#### DNS枚举

```bash
host www.example.com
host -t mx example.com
host -t txt example.com
dnsrecon -d example.com -t axfr
// 自己创建list.txt
dnsrecon -d example.com -D ~/list.txt -t brt
dnsenum example.com
```

#### SMB枚举

* Windows  2000和Windows XP中的未经身份验证的SMB空会话漏洞

```bash
nmap -v -p 139,445 -oG smb.txt 192.168.xx.xxx
// 查找相关NSE脚本，可用这些脚本枚举
ls -1 /usr/share/nmap/scripts/smb*
nmap -v -p 139,445 --script=smb-os-discovery 192.168.xx.xxx
// ‘unsafe=1’意味着运行的脚本会使目标系统崩溃
nmap -v -p 139,445 --script=smb-vuln-ms08-067 --script-args=unsafe=1 192.168.xx.xxx
```

#### NFS枚举

```bash
// 查找可能已经通过rpcbind注册的服务
nmap -sV -p 111 --script=rpcinfo 192.168.xx.xxx
// 查找相关NSE脚本，可用这些脚本枚举
ls -1 /usr/share/nmap/scripts/nfs*
nmap -p 111 --script nfs* 192.168.xx.xxx
```

#### SMTP枚举

```bash
// Some code
```

#### SNMP枚举

```bash
// Some code
```

## 被动信息收集（非OSCP考察内容）

收集互联网上的各种公开信息，不与目标直接交互，不会被目标发现。

### 工具（在线/离线）

* [x] Google hacking

```
site:example.com
filetype:php/html
ext:jsp/cfm/pl
intitle:"index of" "parent directory"
```

* [x] shodan
* [x] GitHub
* [x] netcraft
* [x] OSINT框架
* [x] Maltego
* [x] [安全头（Security Headers）扫描](https://securityheaders.com/)
* [x] [SSL Server Test](https://www.ssllabs.com/ssltest/)
* [x] [Pastebin](https://pastebin.com/)

#### whois查询

```bash
// 查目标域名
whois example.com
// 查目标IP
whois 192.168.xx.xxx
```

#### Recon-ng

```bash
// 运行（注意是否有已安装的可用模块）
recon-ng
// 如果没有，则需要从GitHub上下载
marketplace search github
// 了解指定模块的具体信息
marketplace info recon/domains-hosts/google_site_web
// 下载指定模块
marketplace install recon/domains-hosts/google_site_web
// 使用指定模块对目标域名进行信息收集
modules load recon/domains-hosts/google_site_web
info
options set SOURCE example.com
run
// 退出当前模块
back
// 查看结果
show
show hosts
// 该工具的其他模块也值得逐个尝试
```

### 针对目标组织员工的信息收集

* 电子邮件
* 社交媒体
* 社工钓鱼

