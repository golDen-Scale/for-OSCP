---
description: 适用于将功能有限的非交互式shell升级成完全交互式shell
---

# ✔️ 升级Shell

## Python

```python
// 先在目标系统中查找是否已安装Python，并确认其版本
which python
// 根据实际情况使用：
python -c 'import pty;pty.spawn("/bin/bash")'
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

有时即使利用Python升级后的shell，也可能无法完整使用各种命令行的功能，此时可尝试一下方法：



