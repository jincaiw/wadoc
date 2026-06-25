# Mac 上 Git 安装及 GitHub 的使用

## 目录

- 安装 Git
- 创建 SSH Key、配置 Git
- 提交本地项目到 GitHub

## 一、安装 Git

通过终端查看是否安装 Git：

```bash
git
```

如果已安装，会输出 Git 帮助信息。通过 Homebrew 安装：

```bash
brew install git
```

## 二、创建 SSH Key、配置 Git

### 1. 设置用户名和邮箱

```bash
git config --global user.name "jincaiw"
git config --global user.email "wangjincai@gmail.com"
```

### 2. 创建 SSH Key

```bash
ssh-keygen -t rsa -C "wangjincai@gmail.com"
```

一路回车即可（如已存在会提示是否覆盖）。

查看公钥：

```bash
cat ~/.ssh/id_rsa.pub
```

复制输出的内容。

### 3. 登录 GitHub 添加 SSH Key

GitHub → Settings → SSH and GPG keys → New SSH key → 粘贴公钥 → Add SSH key

### 4. 验证链接

```bash
ssh -T git@github.com
```

输出类似以下内容即表示成功：

```
Hi jincaiw! You've successfully authenticated, but GitHub does not provide shell access.
```

## 三、提交本地项目到 GitHub

### 1. GitHub 上创建仓库

New repository → 填写信息 → Create repository

### 2. Clone 到本地

```bash
git clone git@github.com:jincaiw/your-repo.git
cd your-repo
```

### 3. 提交修改

```bash
git add .
git commit -m "First Commit"
git push
```
