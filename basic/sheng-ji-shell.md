---
description: 适用于将功能有限的非交互式shell升级成完全交互式shell
---

# ✔️ 升级Shell

## Bash

```bash
echo os.system('/bin/bash')
/bin/sh -i
```

## Python

```python
// 先在目标系统中查找是否已安装Python，并确认其版本
which python
// 根据实际情况使用：
python -c 'import pty;pty.spawn("/bin/bash")'
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

## Perl

```bash
perl -e '/bin/bash'
```



## Node.js

```bash
node -e 'var exec=require("child_process").exec;exec("cat ~/key.txt",function(error,stdOut,stdErr){console.log(stdOut);});'
```



## Ruby

```bash
ruby -e 'require "irb";IRB.start(__FILE__)'
```



## 终端

有时即使利用Python、ruby等脚本升级后的shell，也可能无法完整使用各种命令行的功能，此时可尝试一下方法：

### 方法1：

```bash
// 暂时退出当前终端实例
ctrl + z
// 查看Kali本机的终端信息
echo $TERM
// 记录行数和列数信息就行
stty -a
// 分别记录下以上输出之后，将终端设为原始模式，并关闭回显
stty raw -echo
// 重新监听端口，此时格式有点奇怪，但是不要紧
nc -lvnp 5432
reset
// 按实际情况输入'echo $TERM'的信息（例）：
xterm-256color
xterm
// 按实际情况输入行数和列数信息（例）：
stty rows 56 columns 213
```

### 方法2：

```bash
// 暂时退出当前终端实例
ctrl + z

stty size
stty raw -echo
fg
reset
export TERM=screen
```
