# Mac 电脑 Oh My Zsh 安装指南

## 一、Oh My Zsh 是什么

Oh My Zsh 是一款社区驱动的命令行工具，基于 zsh 命令行，提供了主题配置、插件机制以及内置的便捷操作。

## 二、Zsh 是什么

Zsh 是一款强大的虚拟终端，既是一个系统的虚拟终端，也可以作为脚本语言的交互解析器。

```bash
# 查看 Zsh 版本
zsh --version
```

```bash
# 查看系统所有 Shell
cat /etc/shells
```

## 三、安装 Oh My Zsh

curl 方式安装：

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

wget 方式安装：

```bash
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

## 四、开启语法高亮和行号

```bash
echo 'syntax on' >> ~/.vimrc
echo 'set nu!' >> ~/.vimrc
```

## 五、安装主题

```bash
vim ~/.zshrc
```

找到 `ZSH_THEME` 这行，将双引号内文字改成想要套用的主题风格，例如 `agnoster`。

## 六、安装命令高亮插件

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

## 七、安装命令自动补全插件

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

## 八、配置插件

```bash
vim ~/.zshrc
```

找到 `plugins=(git)` 这行，修改为：

```
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
```

## 九、保存生效

```bash
source ~/.zshrc
```

## 十、修改终端显示名称

编辑主题文件，注释掉 `prompt_context`，显示的名称就可以变短。

```bash
vi ~/.oh-my-zsh/themes/agnoster.zsh-theme
```
