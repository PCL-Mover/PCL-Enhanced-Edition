# Plain Craft Launcher 2 - China Patch (Portable)

基于 PCL 2.99.0.0 开源版本的修改版，解除了下载页面对 Minecraft 下载的限制，并构建为 Portable 单文件版本。

## 修改内容

### 1. 解除 MC 下载限制

原版 PCL 开源版出于法律法规原因，通过 `RestrictMcDownload` 常量封锁了所有 MC 下载功能。本补丁将其完全解除：

| 文件 | 改动 |
|------|------|
| `Modules/ModSecret.vb` | `RestrictMcDownload` 从 `True` 改为 `False` |
| `Pages/PageDownload/PageDownloadInstall.xaml.vb` | 移除限制分支，直接加载 MC 版本列表 |
| `Pages/PageDownload/PageDownloadModify.vb` | MC 版本选择卡片始终可见 |
| `Modules/Minecraft/ModModpack.vb` | 移除两处整合包安装限制 |
| `FormMain.xaml.vb` | 移除拖拽整合包文件时的限制异常捕获 |
| `Pages/PageDownload/Resource/PageDownloadCompDetail.xaml.vb` | 整合包"安装"按钮不再受限 |

### 2. Portable 便携版本

将 3 个 C# 项目的 `ProjectReference` 设为 `<Private>False</Private>`，所有 DLL（NAudio、Newtonsoft.Json、Ookii.Dialogs.Wpf、Imazen.WebP、CacheCow、ThrottleDebounce、System.Net.Http.Formatting、PCLCS、MeloongCore、MeloongCore.Wpf、Microsoft.Win32.Registry、libwebp64）均已内嵌在 exe 中。

编译后 `bin/` 目录仅包含：
- `Plain Craft Launcher 2.exe`（5.84 MB，单文件可直接运行）
- `Plain Craft Launcher 2.exe.config`（运行时配置，建议保留）

## 编译方法

需要安装 **Visual Studio 2022**（需包含 .NET Framework 4.8 SDK 和 VB.NET 工具链），然后使用 VS2022 的 MSBuild 编译：

```bat
:: 1. 还原 NuGet 包
"C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\MSBuild.exe" "Plain Craft Launcher 2\Plain Craft Launcher 2.vbproj" -t:Restore -p:Configuration=Debug -p:Platform=AnyCPU -p:RuntimeIdentifier=win -v:minimal

:: 2. 编译整个解决方案
"C:\Program Files\Microsoft Visual Studio\2022\Community\MSBuild\Current\Bin\MSBuild.exe" "Plain Craft Launcher 2.sln" -t:Build -p:Configuration=Debug -p:Platform="Any CPU" -p:RuntimeIdentifier=win -restore -m -v:minimal
```

> **注意：** `dotnet build` 无法编译此项目，因为 .NET SDK 版 MSBuild 不支持 legacy VB.NET WPF 项目的 XAML 标记编译。必须使用 Visual Studio 2022 自带的 MSBuild。

## 上游来源

本仓库基于 [Meloong-Git/PCL](https://github.com/Meloong-Git/PCL) 的 2.99.0.0 正式版源码（commit `6b9a50d`）。

- [PCL 官网](https://meloong.com/pcl)
- [PCL 原版开源仓库](https://github.com/Meloong-Git/PCL)
- [帮助文档库](https://github.com/LTCatt/PCL2Help)

## 许可证

继承上游仓库的许可证，详见 [LICENCE](LICENCE)。
