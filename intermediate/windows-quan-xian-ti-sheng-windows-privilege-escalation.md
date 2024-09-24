# ✔️ Windows权限提升 - Windows Privilege Escalation

## 本地信息收集

### 手动枚举

```powershell
# 枚举系统信息
systeminfo
systeminfo | finder /B /C:"OS Name" /C:"OS Version" /C:"System Type"
hostname
wmic qfe
wmic qfe get caption,Description,HotFixID,InstalledOn
wmic logicaldisk
wmic logicaldisk get caption,description,providername
# 用户枚举
whoami
whoami /priv
whoami /groups
net users
net user administrator
net localgroup
net localgroup administrators
# 枚举网络信息
ipconfig /all
route print
arp -A
netstat -ano
netsh firewall show state
netsh firewall show config
# 枚举计划任务、正在运行的进程
schtasks /query /fo LIST /v
tasklist /SVC
net start
DRIVERQUERY
```

### 工具收集

* [x] winPEAS.exe
* [ ] Seatbelt.exe
* [ ] Watson.exe
* [ ] SharpUp.exe
* [ ] sherlock.ps1
* [ ] PowerUp.ps1
* [ ] jaws-enum.ps1
* [x] windows-exploit-suggester.py
* [ ] Exploit Suggester （Metasploit）
