# ✔️ Return

## 建立立足点

### 信息收集

* 使用Nmap对目标系统进行开放端口扫描：

```bash
nmap -sC -sV -p- -oA return 10.129.95.241 --open
```

<figure><img src="../../.gitbook/assets/1 (19).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/2 (15).png" alt=""><figcaption></figcaption></figure>

* 根据Nmap的输出信息发现目标系统开放了80端口，登录查看是一个打印机的管理员界面：

<figure><img src="../../.gitbook/assets/3 (17).png" alt=""><figcaption></figcaption></figure>

* 使用dirsearch没有找出来什么特别的隐藏文件/目录，在查看settings.php页面发现了域名、端口还有用户名和密码，其中密码部分无法查看：

<figure><img src="../../.gitbook/assets/4 (16).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/5 (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/6 (16).png" alt=""><figcaption></figcaption></figure>

* 将找到的域名和之前Nmap输出中的域名添加到hosts文件里：

<figure><img src="../../.gitbook/assets/7 (18).png" alt=""><figcaption></figcaption></figure>















### 漏洞查阅







### 漏洞利用









### GET SHELL









## 权限提升

### 本地信息收集







### 漏洞利用







### ROOT







{% hint style="info" %}

{% endhint %}
