# Mac 电脑 Ruby 版本升级

Mac 自带 Ruby 版本为 2.x，可通过以下命令查看当前版本：

```bash
ruby -v
```

## 方法一：通过 Homebrew 升级

```bash
brew update
brew install ruby
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```

已安装过的情况，直接重装：

```bash
brew reinstall ruby
```

## 方法二：通过 rbenv 管理（推荐）

### 安装 rbenv

```bash
brew install rbenv
rbenv init
```

### 验证 rbenv

```bash
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-doctor | bash
```

### 查看可安装版本

```bash
rbenv install -l
```

### 安装并设置 Ruby 版本

```bash
rbenv install 3.0.3
rbenv global 3.0.3
```

### 验证

```bash
ruby -v
```
