---
description: Microsoft Windows / System Administrator / 枚举 / 身份验证 / 横向移动 /
---

# ✔️ 活动目录 - Active Directory

## Tips

* 通常，内部网络上的客户端计算机被连接到域控制器和各种其他内部成员服务器上，如数据库服务器、文件存储服务器等。此外，许多组织通过连接互联网的Web服务器提供内容，这些服务器有时也是内部域的成员。
* 活动目录环境对域名系统（DNS）服务非常依赖。因此，AD环境中的典型域控制器通常还会托管给一个对目标域具有权威性的DNS服务器。

## 协议

* [x] NTLM
* [x] Kerberos
* [x] LDAP

## 域

* [x] 工作组
* [x] 域：当配置了一个活动目录的实例时，就创建了一个域（域名：example.com）。在目标域中可添加各类对象（计算机对象、用户对象），system administrator借助组织单元（OU）组织和管理这些对象。
* [x] 域信任
* [x] 本地账户
* [x] 活动目录账户：
* [x] 本地组
* [x] 域组
* [x] 目录分区
* [x] 服务主体名称
* [x] 域中的组策略
* [x] 域中的访问控制列表
* [x] 域控制器（DC）：是AD的核心，存储了有关如何配置AD的特定实例的所有信息；强制执行了各类规则，这些规则规定了目标域内的对象之间如何交互，以及最终用户可以使用哪些服务和工具。

## 工具

* [ ] BloodHound
* [ ] Adfind
* [ ] Admod
* [ ] LDP
* [x] Ldapsearch
* [ ] PingCastle
* [ ] Kekeo
* [ ] Rubeus
* [x] mimikatz
* [x] Impacket

## 攻击方法

* [x] 用户名枚举
* [x] 密码喷洒
* [ ] AS-REP Roasting
* [ ] Kerberoasting
* [ ] 委派
* [ ] Kerberos Bronze Bit
* [ ] NTLM Relay
* [ ] 滥用DCSync
* [x] PTH
* [x] 定位用户登录的主机
* [ ] 域林渗透

## 漏洞利用

### 权限提升

* [ ] MS14-068
* [ ] CVE-2020-1472
* [ ] Windows Print Spooler
* [ ] CVE-2021-42287

### 绕过

* [ ] CVE-2019-1040 NTLM MIC

### ADCS攻击

### 攻击利用链

* [ ] Exchange ProxyLogon
* [ ] Exchange ProxyShell

## 权限维持

* [ ] 票据传递
* [ ] 委派
* [ ] DCShadow
* [ ] Skelenton Key
* [ ] SID History
* [ ] 重置DSRM
* [ ] AdminSDHolder滥用
* [ ] ACL滥用
* [ ] 伪造域控

## 后渗透

* [ ] Hook
* [ ] 注入SSP

## MEMO.

* [https://adsecurity.org/](https://adsecurity.org/)
