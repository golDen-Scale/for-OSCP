---
description: 目的：尽可能全的收集内网信息，分析网络拓扑结构，找出内网中最薄弱的环节
---

# ✔️ 内网信息收集 - Internal Network Information Gathering

## 本机信息收集

### wmic命令

```bash
// 已安装的软件产品
wmic product get name,version
// 所有本机上的服务的简要信息（无论是否正在运行）
wmic service list brief
// 所有本机上正在运行的进程的简要信息（比tasklist更详细）
wmic process list brief
// 所有开机自启动项的命令路径和名称列表
wmic startup get command,caption
// 所有本机上已安装的补丁
wmic qfe get Caption,Description,HotFixID,InstalledOn
// 当前主机的共享资源列表（名称、路径、状态）
wmic share get name,path,status
// 系统/域内所有用户的详细信息（具体根据上下文环境，在域控中查返回域内信息，在本机查返回本地信息）
wmic useraccount get /all
```

### Powershell命令

```powershell
// Some code
```

### Windows命令

```bash
// 本机中所有网络接口的配置信息
ipconfig /all
// 当前本机的操作系统和版本信息
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
// 当前正在运行的进程列表（比wmic process list brief更简洁）
tasklist
// 当前系统中的计划任务的详细信息
schtasks /query /fo LIST /v
// 当前主机的开机时间
net statistics workstation
// 列出本机所有的用户列表
net user
// 本地管理员信息（包含域用户）
net localgroup administrators
// 当前登录到系统的用户会话信息
query user || qwinsta
// 当前登录到系统的用户会话信息(与本地系统建立的所有网络会话的详细信息)
net session
// 
```



### Empire





