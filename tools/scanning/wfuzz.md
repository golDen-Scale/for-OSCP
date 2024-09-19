---
description: 模糊扫描 / 信息枚举
---

# ✔️ Wfuzz

## 参数

```
-c   彩色输出
-u   指定目标的URL
-w   指定字典文件
--hc 隐藏指定状态码的返回信息
-t   指定线程数 
```

## 常用命令

```bash
# 查找隐藏文件
wfuzz -c -u http://目标IP/FUZZ -w subdomains-top1million-110000.txt --hc 404 -t 200 
# 查找隐藏目录
wfuzz -c -u http://目标IP/FUZZ/ -w subdomains-top1million-110000.txt --hc 404 -t 200
# 枚举目标的子域
wfuzz -c -u http://目标域名 -H "Host:FUZZ.目标域名.com" -w subdomains-top1million-110000.txt --hc 404 -t 200 --hl 9  
```
