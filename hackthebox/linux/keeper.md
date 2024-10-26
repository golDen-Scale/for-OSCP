---
description: Easy / 枚举 / RT 4.4.4 / KeePass / 格式转换
---

# ✔️ Keeper

## 建立立足点

### 信息收集

* 使用Nmap对目标系统的开放端口进行扫描：

```bash
nmap -sC -sV -p- -oA keeper 10.129.8.148 --open
```

<figure><img src="../../.gitbook/assets/6 (27).png" alt=""><figcaption></figcaption></figure>

* 在扫描的同时可尝试查看目标的80端口上是否有内容，发现有一个链接，指向域名tickets.keeper.htb，将其添加到hosts文件里：

<figure><img src="../../.gitbook/assets/1 (29).png" alt=""><figcaption></figcaption></figure>

* 再次点击链接，会跳转到[http://tickets.keeper.htb/rt/](http://tickets.keeper.htb/rt/)，一个登录界面：

<figure><img src="../../.gitbook/assets/2 (28).png" alt=""><figcaption></figcaption></figure>

* 该界面底部有当前正在运行的服务及其版本号：RT 4.4.4，尝试了几个弱口令均失败，但是在搜索相关漏洞时，发现了RT 4.4.4的文档文件中的关于安全提示的信息里，包含了该应用的默认凭证信息：<mark style="color:red;">**root : password**</mark>

<figure><img src="../../.gitbook/assets/3 (25).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/4 (27).png" alt=""><figcaption></figcaption></figure>

* 尝试利用该凭证登录成功：

<figure><img src="../../.gitbook/assets/5 (28).png" alt=""><figcaption></figcaption></figure>

* 在检查RT系统中的各项内容时，在Admin——Users中发现可两个用户账户信息界面，**root**和**lnorgaard**，并且找到了用户lnorgaard的明文密码：<mark style="color:red;">**Welcome2023!**</mark>

<figure><img src="../../.gitbook/assets/7 (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/8 (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/9 (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/10 (28).png" alt=""><figcaption></figcaption></figure>

### GET SHELL

* 因为当前已经有两个有效凭证了，根据Nmap的扫描结果来看，还有一个运行着SSH服务的22号端口，可以分别用这两个有效凭证尝试登录：

```bash
# 密码：Welcome2023!
ssh lnorgaard@10.129.8.148
```

<figure><img src="../../.gitbook/assets/11 (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/12 (26).png" alt=""><figcaption></figcaption></figure>

## 权限提升

### 本地信息收集

* 在之前检查RT时发现Admin——Tools——System Configuration里面有一些目标系统信息，手动枚举也没有什么收获：

<figure><img src="../../.gitbook/assets/13 (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/14 (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/15 (3).png" alt=""><figcaption></figcaption></figure>

* 上传linpeas.sh进行信息收集：

<figure><img src="../../.gitbook/assets/16 (3).png" alt=""><figcaption></figcaption></figure>

* 从linpeas的输出信息中发现，以下文件目录是与root用户账户相关联的，可能包含敏感信息，但是却被放在了低权限用户的/home目录中：

<figure><img src="../../.gitbook/assets/17 (2).png" alt=""><figcaption></figcaption></figure>

* 把RT30000.zip下载到本地过程巨慢，直接解压缩后获得两个文件：KeePassDumpFull.dmp、passcodes.kdbx

<figure><img src="../../.gitbook/assets/19 (2).png" alt=""><figcaption></figcaption></figure>

### 漏洞利用

* 通过搜索解压出来的文件得知，keepassdumpfull.dmp文件是KeePass的一个包含密码等敏感信息的转储文件，passcodes.kdbx文件是KeePass的数据库文件，而KeePass是一个密码管理器。简单来说就是要用keepassdumpfull.dmp文件里的密码打开passcodes.kdbx文件，而passcodes.kdbx文件里有目标系统用户的加密密钥，利用该密钥就能实现root。
* 通过搜索找到了可以破解keepassdumpfull.dmp文件的PoC脚本：keepass-dump-masterkey

<figure><img src="../../.gitbook/assets/20 (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/21 (13).png" alt=""><figcaption></figcaption></figure>

* 将PoC上传到目标中执行，破解keepassdumpfull.dmp文件：

```bash
python3 poc.py -d KeePassDumpFull.dmp
```

<figure><img src="../../.gitbook/assets/22 (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/23 (13).png" alt=""><figcaption></figcaption></figure>

* 得到一串看不懂的输出，但是显示的是可能的密码。逐行查询，每一条都显示了相同的内容：**Danish Red Berry Pudding (Rødgrød Med Fløde)**

<figure><img src="../../.gitbook/assets/24 (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/25 (10).png" alt=""><figcaption></figcaption></figure>

### ROOT

* 下载KeePass应用，并且将passcodes.kdbx文件下载到本地，多次尝试上面的字符串打开文件，最终密码是小写的：<mark style="color:red;">**rødgrød med fløde**</mark>

<figure><img src="../../.gitbook/assets/26 (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/27 (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/28 (9).png" alt=""><figcaption></figcaption></figure>

* 打开该文件后看不到root用户的密码部分，但是在它的Notes选项中包含了PuTTy的key：

<figure><img src="../../.gitbook/assets/29 (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/30 (7).png" alt=""><figcaption></figcaption></figure>

* 需要先将这个Key的内容都复制下来，保存为ppk格式，但是这个是PuTTy专用的文件格式，需要转换为一个更通用的文件格式，再利用其登录：

```bash
# 将.ppk转换为.pem
puttygen key.ppk -O private-openssh -o key.pem
chmod 400 key.pem
# 利用key.pem作为root用户的密码登录ssh
ssh -i key.pem root@10.129.229.41
```

<figure><img src="../../.gitbook/assets/31 (6).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/32 (4).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Get Shell阶段只需要简单的枚举即可获得shell。提权过程不算顺利，在查找破解keepassdumpfull.dmp文件的PoC脚本时，做了很多的尝试，还有再奇怪的字符串也可以是密码。(本例机器中途重置过，IP地址有变化，但不影响漏洞利用及其实现结果)
{% endhint %}
