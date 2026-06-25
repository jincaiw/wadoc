# 搭建 GitHub 图床（PicGo 配合使用）

## 第一步：GitHub 仓库设置

### 1. 新建仓库

点击 GitHub 主页右上角的 + 创建 New repository；

> 填写仓库名称，例如创建名称为 `pic` 的仓库。
>
> **注意**：仓库必须设置为 **Public**。

![创建仓库](https://raw.githubusercontent.com/jincaiw/pic/master/image-20230221212828691.png)

### 2. 创建 Token 并复制保存

仓库创建完成后，点击右上角头像，进入设置页面：

![image-20230221213659510](https://raw.githubusercontent.com/jincaiw/pic/master/image-20230221213659510.png)

在页面最下方找到 **Developer settings**，点击进入：

![image-20230221213824964](https://raw.githubusercontent.com/jincaiw/pic/master/image-20230221213824964.png)

创建 Token：在 Description 随便填写，勾选复选框 `repo`，点击确认 Generate token。

![image-20230221214134462](https://raw.githubusercontent.com/jincaiw/pic/master/image-20230221214134462.png)

生成一串字符 token，该 token 只出现一次，请妥善保存（建议保存在 Mac 备忘录）。

![image-20230221214344779](https://raw.githubusercontent.com/jincaiw/pic/master/image-20230221214344779.png)

---

## 第二步：安装 PicGo 客户端配置

### 1. 下载与安装

如果已安装 Homebrew，直接通过 cask 安装即可，最新版本为 2.3.1。PicGo 是一款开源的图床工具，免费使用。

```bash
brew install picgo --cask
```

### 2. 图床配置

填写仓库名，粘贴 Token，其他配置保持默认即可。

![image-20230221215228129](https://raw.githubusercontent.com/jincaiw/pic/master/image-20230221215228129.png)
