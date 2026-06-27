# 文件酷（FileCube）- 文件管理从未如此优雅

<div align="center">

✨ **文件管理从未如此优雅**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![.NET](https://img.shields.io/badge/.NET-8.0-purple.svg)](https://dotnet.microsoft.com/)
[![Platform](https://img.shields.io/badge/platform-Windows-lightgrey.svg)](https://www.microsoft.com/windows)
[![Release](https://img.shields.io/badge/release-v0.9-brightgreen.svg)](https://github.com/yumai-team/filecube/releases)
[![Download](https://img.shields.io/badge/download-绿色版-orange.svg)](https://yumai-team.github.io/filecube/)

> **中文名称**：文件酷　|　**英文名称**：FileCube　|　**代码代号**：VaultTags
>
> 🌐 **官网下载**：[yumai-team.github.io/filecube](https://yumai-team.github.io/filecube/)
>
> 📦 **GitHub Releases**：[github.com/yumai-team/filecube/releases](https://github.com/yumai-team/filecube/releases)

</div>

---

## 📖 项目简介

文件酷（FileCube）是一款专为 Windows 设计的本地文件管理工具。它的核心理念是——**用多标签代替层级目录**，让文件管理回归简单与优雅。

**为什么要放弃目录层级？**

- 文件往往从多个维度都有归属意义（一张旅行照片既属于"旅行"，也属于"2025"，还属于"家庭"），但传统目录逼迫你把它塞进某一个子目录，然后与其他维度说再见；
- 对有整理强迫症的用户来说，"到底放哪个目录最合适"是一个无休止的纠结；
- 为了在多个路径下都能找到同一份文件，人们会复制多份副本，产生大量文件重复；
- 目录层级越深，定位越慢，忘记放在哪一层是常态。

**文件酷的解法：**

- 🏷️ **多标签归属**：一个文件可以拥有任意多个标签，一次归类，多维度可查，不再需要选择"放哪个目录"；
- 🔑 **内容去重**：相同内容的文件在系统中只保留一份物理存储（SHA-256 哈希去重），通过标签聚合——从根源杜绝文件重复；
- 🔎 **标签 + 原名快速查找**：通过多标签交集筛选，配合原始文件名关键词搜索，比层层打开目录更高效；
- 🔒 **隐私保护**：UUID 重命名存储，外部无法识别。

### ✨ 核心特性

- 🏷️ **多标签归属** - 文件不再被迫属于某一个子目录，一个文件可拥有任意多个标签
- 🧹 **内容去重** - SHA-256 哈希比对，相同内容只存一份，彻底告别"文件副本"
- 🔎 **标签交集筛选** - 多标签组合查询，替代传统的"层层进入子目录"
- 🔐 **密码保护** - 多密码登录，BCrypt 哈希加密
- 🤖 **智能标签** - 自动分词提取标签，快速筛选
- 🔒 **隐私保护** - UUID 重命名存储，外部无法识别
- 🖼️ **内置预览** - 图片查看器，支持缩放/全屏
- 🎬 **多媒体支持** - 视频/音频播放，缩略图生成
- 🗑️ **回收站** - 软删除机制，可恢复文件
- ⚡ **高性能** - 支持 10 万+ 文件流畅管理

---

## 🚀 快速开始

### 系统要求

- **操作系统**: Windows 10 21H2+ / Windows 11 (x64)
- **.NET 版本**: .NET 8.0 LTS
- **内存**: 最低 4GB，推荐 8GB
- **磁盘空间**: 至少 500MB (不含文件存储)

### 安装步骤

1. **下载最新版本**
   - 🟢 官网下载页：[yumai-team.github.io/filecube](https://yumai-team.github.io/filecube/)
   - 📦 直接下载 zip：[FileCube.zip](https://github.com/yumai-team/filecube/releases/download/v0.9/FileCube.zip)
   - 或前往 [GitHub Releases](https://github.com/yumai-team/filecube/releases) 选择版本

2. **解压即用（绿色版，无需安装）**
   ```
   # 解压到任意目录
   cd FileCube
   ./FileCube.exe
   ```

3. **首次启动设置密码**
   - 首次使用会提示设置密码
   - 密码用于保护文件访问安全

---

## 📂 项目结构

```
VaultTags/
├── src/
│   ├── VaultTags.App/              # WPF 主应用程序
│   │   ├── App.xaml                # 应用定义
│   │   ├── Program.cs              # 入口类
│   │   └── VaultTags.App.csproj    # 项目文件
│   │
│   ├── VaultTags.Core/             # 核心业务层
│   │   ├── Models/                 # 数据模型
│   │   ├── Services/               # 服务接口
│   │   ├── Repositories/           # 仓储接口
│   │   └── Utilities/              # 工具类
│   │
│   ├── VaultTags.Infrastructure/   # 基础设施层
│   │   ├── Data/                   # 数据访问实现
│   │   ├── Storage/                # 存储服务
│   │   └── Logging/                # 日志服务
│   │
│   └── VaultTags.UI/               # UI 层
│       ├── Views/                  # 窗口和控件
│       ├── ViewModels/             # 视图模型
│       ├── Converters/             # 值转换器
│       └── Styles/                 # 样式资源
│
├── tests/                          # 单元测试(待添加)
├── docs/                           # 文档(待添加)
├── publish/                        # 发布输出(绿色版)
└── README.md                       # 本文件
```

---

## 🛠️ 开发指南

> **注意**：源代码托管于内部代码仓库，GitHub 仓库仅用于发布下载和 Issue 跟踪。

### 从源码构建

1. **克隆仓库**
   ```bash
   git clone <源码仓库地址>
   cd VaultTags
   ```

2. **还原依赖**
   ```bash
   dotnet restore
   ```

3. **编译项目**
   ```bash
   dotnet build --configuration Release
   ```

4. **运行应用**
   ```bash
   dotnet run --project src/VaultTags.App
   ```

### 技术栈

| 类别 | 技术 |
|------|------|
| **框架** | .NET 8.0 + WPF |
| **架构模式** | MVVM |
| **数据库** | SQLite + Dapper |
| **MVVM 框架** | CommunityToolkit.Mvvm |
| **密码加密** | BCrypt.Net-Next |
| **中文分词** | jieba.NET (待集成) |
| **日志** | Serilog |

---

## 📋 当前版本状态

### ✅ v0.9 - 当前发布版本

- [x] 项目结构搭建
- [x] 数据库初始化
- [x] 密码登录功能（多密码支持）
- [x] 主窗口布局
- [x] 日志系统
- [x] 工具类实现
- [x] 拖拽导入文件
- [x] SHA-256 内容去重
- [x] UUID 文件重命名存储
- [x] 多标签归类管理
- [x] 标签交集筛选搜索
- [x] 图片/视频/音频预览播放
- [x] 回收站软删除机制
- [x] 绿色版打包发布

### 🚧 后续计划

- [ ] 文件导出 / 另存为
- [ ] 自动中文分词标签
- [ ] 缩略图网格视图
- [ ] 批量标签操作
- [ ] 快捷键支持

---

## 📸 界面预览

*(截图待添加)*

---

## 🔧 配置说明

### 数据存储位置

所有数据存储在程序目录下的以下位置:

```
FileCube/
├── Data/              # 原始文件(UUID 命名,无后缀)
├── Thumbs/            # 缩略图(UUID 命名,无后缀)
├── Logs/              # 日志文件(按日期分割)
└── VaultTags.db       # SQLite 数据库
```

### 备份与恢复

**备份**: 复制整个 `FileCube` 目录即可完整备份

**恢复**: 将备份目录覆盖到目标位置即可

---

## ❓ 常见问题

### Q: 忘记密码怎么办?
A: 目前版本不支持密码找回。如需重置，可删除 `VaultTags.db` 文件(**注意:会丢失所有数据**)。

### Q: 导入的文件在哪里?
A: 文件被移动到 `Data/` 目录并重命名为 UUID，原位置不再保留。这是为了保护隐私。

### Q: 如何导出文件?
A: 右键点击文件选择"另存为"，可选择保存位置和文件名。

### Q: 为什么不用传统文件夹?
A: 因为传统文件夹逼迫一个文件只属于一个位置——一张旅行照片不能同时在"旅行"、"2025"、"家庭"三个目录里（除非复制三份副本，而这会制造重复文件）。文件酷用**多标签**解决了这个问题：一个文件可以同时拥有任意多个标签，按多维度归属，并且只保留一份物理存储。

### Q: 支持哪些文件格式?
A: 
- **图片**: JPG, PNG, GIF, BMP, WebP, TIFF
- **视频**: MP4, AVI, MKV, MOV, WMV, FLV
- **音频**: MP3, WAV, FLAC, AAC, OGG, WMA

### Q: 软件是免费的吗？
A: 完全免费，无广告，无捆绑。绿色版解压即用，不写注册表。

### Q: 如何在多台电脑间同步？
A: 将整个 `FileCube` 目录复制到 U 盘或同步网盘，即可在其他电脑上直接运行。

---

## 🤝 参与贡献

- 🐛 **报告 Bug**：[GitHub Issues](https://github.com/yumai-team/filecube/issues)
- 💡 **功能建议**：[GitHub Issues](https://github.com/yumai-team/filecube/issues)
- ⭐ **支持我们**：给 [yumai-team/filecube](https://github.com/yumai-team/filecube) 点个 Star！

> 源代码托管于内部代码仓库，GitHub 仓库主要用于发布分发和 Issue 跟踪。

---

## 📝 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

---

## 📮 联系方式

| 渠道 | 链接 |
|------|------|
| 🌐 **官网** | [yumai-team.github.io/filecube](https://yumai-team.github.io/filecube/) |
| 📦 **下载** | [GitHub Releases](https://github.com/yumai-team/filecube/releases) |
| 🐛 **反馈** | [GitHub Issues](https://github.com/yumai-team/filecube/issues) |
| 🏠 **组织** | [yumai-team.github.io](https://yumai-team.github.io/) |

---

<div align="center">

**Made with ❤️ by Yumai Team**

⭐ 如果这个项目对您有帮助，请给我们一个 Star！

</div>
