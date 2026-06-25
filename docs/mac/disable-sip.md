# macOS 禁用 SIP 教程

## 关闭 SIP

关闭 SIP 需要进入恢复模式：

1. 重新启动 Mac
2. 同时按住 **Command + R** 不放，直到看到苹果标志
3. 进入 macOS 恢复模式

在顶部菜单点击 **实用工具 → 终端**，输入：

```bash
csrutil disable
```

成功后会提示：

```
Successfully disabled System Integrity Protection. Please restart the machine for the changes to take effect.
```

点击顶部菜单 ** → 重新启动** 即可。

!!! note "macOS 10.15 及以上"
    关闭 SIP 重启后，还需在终端运行以下命令才能获取完全权限：

    ```bash
    sudo mount -uw /
    ```

## 重新打开 SIP

同样进入恢复模式，打开终端：

```bash
csrutil enable
```

成功后会提示：

```
Successfully enabled System Integrity Protection. Please restart the machine for the changes to take effect.
```

点击 ** → 重新启动** 即可。

!!! tip "建议"
    SIP 能有效保护系统文件被恶意程序修改和删除，正常情况下建议保持开启。
