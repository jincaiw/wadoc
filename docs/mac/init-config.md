# Mac 电脑初始化配置详细指南

## macOS 简单设置

### 1. 触摸板设置

`系统设置` → `触控板`

- `单击` → 开启轻点来点按
- `查询与数据检测器` → 三指轻点

### 2. 键盘设置

`系统设置` → `键盘`

- 建议将 F1 - F12 设置为标准功能键
- `快捷键` → `所有控制`

### 3. Dock 设置

`系统设置` → `程序坞与菜单栏`

- Dock 只放置常用 App
- Dock 栏建议移动到左侧：`置于屏幕上的位置` → `左侧`
- 建议开启：`将窗口最小化至应用程序图标`

### 4. 取消自动更新

`系统设置` → `软件更新` → 关闭自动检查更新

### 5. 输入法快捷键

`系统设置` → `键盘` → `快捷键` → `输入法`

### 6. 热区锁屏

`系统设置` → `桌面与屏幕保护程序` → `屏幕保护程序` → `触发角`

右下角选择：`将显示器置于睡眠状态`

## 一、必装软件

### 1. Homebrew

安装命令：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

批量安装常用软件：

```bash
brew install wget git tree unrar telnet
brew install node nginx golang deno rustup-init
brew install --cask google-chrome
brew install --cask visual-studio-code
brew install --cask docker
brew install --cask wechat qq qqmusic
brew install --cask neteasemusic
brew install --cask tencent-meeting
brew install --cask sogouinput
brew install --cask postman
brew install --cask intellij-idea
brew install --cask datagrip
brew install --cask iina
brew install --cask mos
brew install --cask youdaodict
```

### 2. Oh My Zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 3. Git

```bash
brew install git
```

## Homebrew 常用命令

```bash
# 搜索软件
brew search git

# 安装
brew install wget

# 重新安装
brew reinstall vim

# 升级软件
brew upgrade curl

# 不升级软件
brew pin curl

# 卸载软件
brew uninstall git

# 查看软件信息
brew info git

# 列出已安装软件
brew list

# 启动服务
brew services start mariadb

# 重启服务
brew services restart nginx

# 停止服务
brew services stop mariadb
```

## 笔记本软件

### flomo

> 随时快速记录想法、读书笔记。支持双向链接、随时回顾、标签及全文搜索。
>
> 年会员费比较便宜，买个会员有更多功能，图片可原图上传。

[下载链接](https://flomoapp.com)

### Typora

> 支持 Markdown 格式的记事本软件。已开始收费，网上能找到旧版本的免费版本，基本使用不受影响。

### 妙言

> 优点：比较轻量的 Markdown 软件，使用起来特别舒服，功能较多。
>
> 缺点：网上的教程比较少，官方支持文件较少，快捷键支持不是很好。
