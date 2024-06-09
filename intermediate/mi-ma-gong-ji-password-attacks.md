---
description: 用于发现和使用有效凭证
---

# ✔️ 密码攻击 - Password Attacks

## 字典

* 优先考虑速度，会舍弃密码的有效覆盖率为代价
* 在创建和目标相关的字典文件时，精确度比覆盖率更重要（可创建与目标相关性更高的字典文件）
* /usr/share/wordlists/
* [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)
* 工具：John the Ripper（根据实际需求修改规则/etc/john/john.conf）

## 暴力破解

* 优先考虑密码的覆盖率，以牺牲速度为代价
* 直接跑现成的字典文件的话简单，耗时，但效率低下
* 创建更符合目标需求的字典列表：
  * 与特定模式匹配的所有可能密码的字典文件
  * crunch + 占位符

## 常见的网络服务攻击方式

### HTTP htaccess攻击（Medusa）





### 远程桌面协议攻击（Crowbar）







### SSH攻击（THC-Hydra）







### HTTP POST攻击（THC-Hydra）





## 利用密码散列

* 一旦获取对目标系统的访问权限，能够提取密码hash时，就可以通过破解哈希来获取其明文密码
*





