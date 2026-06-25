# Mac 查看与修改系统默认 Shell

## 1. 查看 Mac 系统所有 Shell

```bash
cat /etc/shells
```

输出：

```
/bin/bash
/bin/csh
/bin/dash
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```

## 2. 查看当前 Shell

```bash
echo $SHELL
```

## 3. 查看当前使用的 Shell

```bash
echo $0
```

输出示例：

```
-zsh
```

## 4. 修改系统默认 Shell 为 Bash

```bash
chsh -s /bin/bash
```

修改完成后，需要重启终端使配置生效。
