# Mac mini M2 通过 Brew 安装的软件

## Homebrew 安装

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安装完成后需要设置 zsh 变量才能执行具体命令。

!!! tip "终端类型判断"
    根据执行 `echo $SHELL` 的结果：

    - `/bin/bash` → 使用 `.bash_profile`
    - `/bin/zsh` → 使用 `.zprofile`

从 macOS Catalina（10.15.x）开始，Mac 使用 zsh 作为默认 Shell，使用 `.zprofile`：

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```
