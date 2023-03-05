# Redis 环境准备

## macOS

在命令行界面运行下列命令即可：

```shell
brew install redis
brew services start redis
```

## Windows

### 安装

在命令行界面运行下列命令：

```shell
scoop install redis
```

### 作为 Windows 系统服务运行

1. 将 Redis 安装目录加入系统 Path 路径

* 在文件浏览器中找到 This PC（这台电脑）右键点击之，选择 Properties（属性设置）；
* 在弹出的窗口中选择 Advanced system settings（高级系统设置）；
* 在弹出的窗口中选择下面的 Environment Variables（环境变量）；
* 在弹出的窗口中上面一个列表框里找到 Path 一项，双击打开编辑；
* 在编辑窗口选择 New（新建），然后选择 Browse...（浏览），找到 `C:\Users\本地用户名\scoop\apps\redis\current`，确认并关闭编辑窗口。

> 上面最后一步中，如果 Windows 版本较老，可能打开的编辑窗口里没有新建按钮，只有一个直接编辑 Path 变量值的文本输入框，那么在输入框的最后加入`;`，然后将 redis 安装路径 `C:\Users\本地用户名\scoop\apps\redis\current` 粘贴在最后，点击确定直至关闭系统设置窗口。

2. 在 ConEMU 中打开一个 PowerShell (Admin) 会话窗口（即管理员权限的命令行界面，如果有安全提示选择允许运行），然后执行下面的命令：

```powershell
cd C:\Users\trust\scoop\apps\redis\current
redis-server.exe --service-install redis.windows-service.conf --loglevel verbose
```

3. 在 Windows 搜索栏输入 Services 找到并运行 Services 管理程序，如果上一步没有问题，打开的列表中应该有 Redis 一项，右键选择启动（Start）即可。

> 在较老的 Windows 上没有 Services 管理程序，可以打开 Control Panel（控制面板），找到 Administration Tools（管理工具）里面的 Services（服务）。

## 验证

在命令行界面运行 `redis-cli` 命令，如果前面 Redis 服务安装运行成功可以看到 Redis 环境的提示符：

```shell
127.0.0.1:6379>
```
