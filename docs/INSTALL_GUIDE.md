# VaultTags 安装指南

## ⚠️ 当前问题

您遇到的错误提示：
```
'dotnet' 不是内部或外部命令，也不是可运行的程序
```

**原因**: 系统中未安装 .NET 8 SDK

---

## 🔧 解决方案

### 方法一：安装 .NET 8 SDK（推荐）⭐

#### 步骤 1: 下载 SDK

访问官方下载页面：
```
https://dotnet.microsoft.com/download/dotnet/8.0
```

选择 **Windows x64** 版本下载：
- 文件名类似：`dotnet-sdk-8.0.xxx-win-x64.exe`
- 文件大小：约 200MB

#### 步骤 2: 安装 SDK

1. 双击下载的 `.exe` 文件
2. 点击 "Install" 按钮
3. 等待安装完成（约 2-5 分钟）
4. **重要**: 安装完成后关闭所有命令行窗口

#### 步骤 3: 验证安装

打开**新的**命令行窗口（CMD 或 PowerShell），输入：
```bash
dotnet --version
```

如果显示版本号（如 `8.0.100`），说明安装成功！✅

#### 步骤 4: 运行项目

```bash
cd "c:\Users\Administrator\Desktop\文件管理器"
dotnet restore
dotnet build
dotnet run --project src/VaultTags.App
```

或者直接双击运行 `run.ps1`（PowerShell 脚本）

---

### 方法二：使用 Visual Studio 2022

如果您已安装 **Visual Studio 2022**（社区版免费）：

#### 步骤 1: 确认工作负载

打开 Visual Studio Installer，确保已安装：
- ✅ ASP.NET and web development
- ✅ .NET desktop development

#### 步骤 2: 打开项目

1. 双击 [`VaultTags.sln`](file://c:\Users\Administrator\Desktop\文件管理器\VaultTags.sln)
2. Visual Studio 会自动还原 NuGet 包
3. 等待右下角进度条完成

#### 步骤 3: 运行项目

按 `F5` 或点击绿色播放按钮 ▶️

---

### 方法三：使用 Rider（JetBrains）

如果您使用 JetBrains Rider：

1. 打开 Rider
2. File → Open → 选择 `VaultTags.sln`
3. 等待索引完成
4. 点击运行按钮

---

## 📝 常见问题

### Q1: 安装后仍然提示 "dotnet 不是内部命令"

**解决方法**:
1. 重启电脑
2. 或者重新打开命令行窗口（旧的窗口不会自动更新环境变量）

### Q2: 如何检查是否已安装 .NET？

**方法一**: 命令行
```bash
dotnet --list-sdks
```

**方法二**: 控制面板
- 打开 "控制面板" → "程序和功能"
- 查找 "Microsoft .NET SDK 8.0.xxx"

### Q3: PowerShell 执行策略限制

如果运行 `run.ps1` 时提示：
```
无法加载文件 run.ps1，因为在此系统上禁止运行脚本
```

**解决方法**:
以管理员身份运行 PowerShell，输入：
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

然后输入 `Y` 确认。

### Q4: NuGet 包还原失败

如果看到网络错误：
```
Unable to load the service index for source https://api.nuget.org/v3/index.json
```

**解决方法**:
1. 检查网络连接
2. 或使用国内镜像源

创建 `NuGet.Config` 文件：
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    <add key="aliyun" value="https://mirrors.aliyun.com/nuget/v3/index.json" />
  </packageSources>
</configuration>
```

---

## 🚀 快速开始（安装完成后）

### 方式一：使用批处理脚本
```bash
cd "c:\Users\Administrator\Desktop\文件管理器"
run.bat
```

### 方式二：使用 PowerShell 脚本
```powershell
cd "c:\Users\Administrator\Desktop\文件管理器"
.\run.ps1
```

### 方式三：手动命令
```bash
cd "c:\Users\Administrator\Desktop\文件管理器"
dotnet restore      # 还原依赖
dotnet build        # 编译项目
dotnet run --project src/VaultTags.App  # 运行应用
```

---

## 📊 系统要求

| 项目 | 要求 |
|------|------|
| **操作系统** | Windows 10 21H2+ / Windows 11 (x64) |
| **.NET SDK** | 8.0 或更高版本 |
| **内存** | 最低 4GB，推荐 8GB |
| **磁盘空间** | 至少 1GB（含 SDK） |
| **IDE（可选）** | Visual Studio 2022 / Rider / VS Code |

---

## 🎯 下一步

安装完成后，您将看到：

1. **登录窗口** - 首次使用可直接回车进入
2. **主界面** - 左侧导航栏 + 右侧内容区
3. **功能提示** - v0.1 版本显示各功能的开发计划

---

## 📞 需要帮助？

如果安装过程中遇到问题：

1. 查看 [.NET 官方文档](https://learn.microsoft.com/zh-cn/dotnet/core/install/windows)
2. 检查 [GitHub Issues](https://github.com/dotnet/sdk/issues)
3. 或在项目中提出 Issue

---

**祝您安装顺利！** 🎉

