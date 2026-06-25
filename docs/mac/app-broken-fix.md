# Mac「已损坏，打不开」的解决方法

## 一、问题分析

通常在非 Mac App Store 下载的软件都会提示「xxx 已损坏，打不开。您应将它移到废纸篓」或者「打不开 xxx，因为它来自身份不明的开发者」。

## 二、原因

Mac 电脑启用了安全机制，默认只信任 Mac App Store 下载的软件以及拥有开发者 ID 签名的软件，同时也阻止了没有开发者签名的软件。

## 三、解决方法

### macOS Mojave 10.14 及以下系统

打开「终端」，输入以下命令并回车（输入开机密码）：

```bash
sudo spctl --master-disable
```

### macOS Catalina 10.15 系统

打开「终端」，输入以下命令（将软件拖入终端获取路径）：

```bash
sudo xattr -rd com.apple.quarantine /Applications/YourApp.app
```

例如 Xmind：

```bash
sudo xattr -rd com.apple.quarantine /Applications/Xmind.app
```
