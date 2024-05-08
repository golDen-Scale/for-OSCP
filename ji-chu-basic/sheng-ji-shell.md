---
description: 适用于将功能有限的非交互式shell升级成完全交互式shell
---

# ✔️ 升级Shell

## Python

```python
python -c 'import pty;pty.spawn("/bin/bash")'
python3 -c 'import pty;pty.import("/bin/bash")'
```







