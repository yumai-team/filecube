# VaultTags v0.1 项目结构说明

## 📁 目录结构

```
文件管理器/
├── VaultTags.sln                          # Visual Studio 解决方案文件
├── README.md                              # 项目说明文档
├── run.bat                                # Windows 快速启动脚本
├── .gitignore                             # Git 忽略配置
│
├── src/                                   # 源代码目录
│   │
│   ├── VaultTags.App/                     # 🚀 主应用程序
│   │   ├── App.xaml                       # WPF 应用定义
│   │   ├── Program.cs                     # 应用程序入口
│   │   └── VaultTags.App.csproj           # 项目文件
│   │
│   ├── VaultTags.Core/                    # 💼 核心业务层
│   │   ├── Models/                        # 数据模型
│   │   │   ├── MediaFile.cs               # 媒体文件实体
│   │   │   ├── Tag.cs                     # 标签实体
│   │   │   └── Setting.cs                 # 设置实体
│   │   │
│   │   ├── Services/                      # 服务接口
│   │   │   ├── IAuthService.cs            # 认证服务接口
│   │   │   └── AuthService.cs             # 认证服务实现
│   │   │
│   │   ├── Repositories/                  # 数据访问接口
│   │   │   ├── IMediaFileRepository.cs    # 文件仓储接口
│   │   │   ├── ITagRepository.cs          # 标签仓储接口
│   │   │   └── ISettingRepository.cs      # 设置仓储接口
│   │   │
│   │   ├── Utilities/                     # 工具类
│   │   │   ├── HashHelper.cs              # SHA-256 哈希计算
│   │   │   ├── UuidHelper.cs              # UUID 生成
│   │   │   └── MimeHelper.cs              # MIME 类型判断
│   │   │
│   │   └── VaultTags.Core.csproj          # 项目文件
│   │
│   ├── VaultTags.Infrastructure/          # 🔧 基础设施层
│   │   ├── Data/                          # 数据访问实现
│   │   │   ├── DatabaseContext.cs         # 数据库上下文
│   │   │   ├── MediaFileRepository.cs     # 文件仓储实现
│   │   │   ├── TagRepository.cs           # 标签仓储实现
│   │   │   └── SettingRepository.cs       # 设置仓储实现
│   │   │
│   │   ├── Logging/                       # 日志服务
│   │   │   └── LoggerConfiguration.cs     # Serilog 配置
│   │   │
│   │   └── VaultTags.Infrastructure.csproj # 项目文件
│   │
│   └── VaultTags.UI/                      # 🎨 UI 层
│       ├── Views/                         # 窗口视图
│       │   ├── LoginWindow.xaml           # 登录窗口界面
│       │   ├── LoginWindow.xaml.cs        # 登录窗口代码
│       │   ├── MainWindow.xaml            # 主窗口界面
│       │   └── MainWindow.xaml.cs         # 主窗口代码
│       │
│       ├── ViewModels/                    # 视图模型
│       │   ├── LoginViewModel.cs          # 登录 ViewModel
│       │   └── MainViewModel.cs           # 主窗口 ViewModel
│       │
│       ├── Converters/                    # 值转换器
│       │   └── CommonConverters.cs        # 通用转换器
│       │
│       └── VaultTags.UI.csproj            # 项目文件
│
├── docs/                                  # 📚 文档目录(待创建)
│   ├── VaultTags 详细设计文档.txt         # 完整设计文档
│   └── VaultTags 开发任务清单.md          # 任务跟踪清单
│
└── tests/                                 # 🧪 测试项目(待创建)
    ├── VaultTags.Core.Tests/              # 核心层单元测试
    └── VaultTags.Infrastructure.Tests/    # 基础设施层测试
```

---

## 🏗️ 架构说明

### 分层架构

```
┌─────────────────────────────────────┐
│      Presentation Layer (UI)        │  ← WPF + MVVM
│         VaultTags.UI                │
└──────────────┬──────────────────────┘
               │ 依赖
┌──────────────▼──────────────────────┐
│       Application Layer             │  ← 业务逻辑
│        VaultTags.App                │
└──────────────┬──────────────────────┘
               │ 依赖
┌──────────────▼──────────────────────┐
│        Domain Layer (Core)          │  ← 领域模型 + 接口
│       VaultTags.Core                │
└──────────────┬──────────────────────┘
               │ 依赖
┌──────────────▼──────────────────────┐
│   Infrastructure Layer              │  ← 数据访问 + 日志
│  VaultTags.Infrastructure           │
└─────────────────────────────────────┘
```

### 依赖关系

```
VaultTags.App
  ├─→ VaultTags.Core
  ├─→ VaultTags.Infrastructure
  └─→ VaultTags.UI

VaultTags.UI
  └─→ VaultTags.Core

VaultTags.Infrastructure
  └─→ VaultTags.Core
```

---

## 📦 核心组件说明

### 1. VaultTags.Core (核心业务层)

**职责**: 定义领域模型、业务规则和服务接口

#### Models (数据模型)
- **MediaFile**: 媒体文件实体,包含文件元数据
- **Tag**: 标签实体,支持扁平化标签系统
- **Setting**: 配置项实体,存储应用设置

#### Services (服务接口)
- **IAuthService**: 密码认证服务
  - `VerifyPassword()`: 验证密码
  - `AddPassword()`: 添加新密码
  - `RemovePassword()`: 移除密码

#### Repositories (数据访问接口)
- **IMediaFileRepository**: 文件数据访问
- **ITagRepository**: 标签数据访问
- **ISettingRepository**: 设置数据访问

#### Utilities (工具类)
- **HashHelper**: SHA-256 哈希计算
- **UuidHelper**: GUID 生成
- **MimeHelper**: MIME 类型识别和文件分类

---

### 2. VaultTags.Infrastructure (基础设施层)

**职责**: 实现数据访问、日志记录等基础设施

#### Data (数据访问)
- **DatabaseContext**: 
  - 管理 SQLite 连接
  - 数据库初始化(建表、索引、触发器)
  - WAL 模式配置
  
- **MediaFileRepository**: 
  - 使用 Dapper ORM
  - 实现 CRUD 操作
  - 分页查询优化
  
- **TagRepository**: 
  - 标签管理
  - 使用统计
  
- **SettingRepository**: 
  - 内存缓存机制
  - 懒加载优化

#### Logging (日志)
- **LoggerConfiguration**: 
  - Serilog 配置
  - 按日分割日志文件
  - 保留 30 天

---

### 3. VaultTags.UI (UI 层)

**职责**: 用户界面和交互逻辑

#### Views (视图)
- **LoginWindow**: 
  - 密码输入界面
  - 回车键支持
  - 错误提示
  
- **MainWindow**: 
  - 左侧导航栏(分类+标签)
  - 顶部工具栏(排序+搜索)
  - 文件列表区域(占位)
  - 状态栏

#### ViewModels (视图模型)
- **LoginViewModel**: 
  - 密码验证逻辑
  - 错误状态管理
  
- **MainViewModel**: 
  - 分类数据初始化
  - 命令绑定(导入、标签、回收站)

#### Converters (值转换器)
- **FileSizeConverter**: 字节 → KB/MB/GB
- **DateTimeConverter**: 日期 → "刚刚"/"X分钟前"

---

### 4. VaultTags.App (应用程序层)

**职责**: 应用启动、生命周期管理

- **Program.cs**: 
  - 应用入口点
  - 日志系统初始化
  - 必要目录创建(Data/Thumbs/Logs)
  
- **App.xaml**: 
  - WPF 资源定义
  - 启动窗口配置

---

## 🔑 关键技术点

### 1. 数据库设计

```sql
-- MediaFiles 表(文件主表)
CREATE TABLE MediaFiles (
    Id TEXT PRIMARY KEY,              -- UUID
    OriginalName TEXT NOT NULL,       -- 原始文件名
    FileSize INTEGER NOT NULL,        -- 文件大小(字节)
    ContentHash TEXT NOT NULL UNIQUE, -- SHA-256
    MimeType TEXT NOT NULL,           -- MIME 类型
    Category TEXT NOT NULL,           -- IMAGE/VIDEO/AUDIO/OTHER
    ImportDate DATETIME NOT NULL,     -- 导入时间
    AccessCount INTEGER DEFAULT 0,    -- 访问次数
    IsDeleted INTEGER DEFAULT 0       -- 软删除标记
);

-- Tags 表(标签表)
CREATE TABLE Tags (
    Id INTEGER PRIMARY KEY AUTOINCREMENT,
    Name TEXT NOT NULL UNIQUE,
    UsageCount INTEGER DEFAULT 0,
    Color TEXT
);

-- FileTags 表(关联表)
CREATE TABLE FileTags (
    FileId TEXT NOT NULL,
    TagId INTEGER NOT NULL,
    PRIMARY KEY (FileId, TagId),
    FOREIGN KEY (FileId) REFERENCES MediaFiles(Id) ON DELETE CASCADE,
    FOREIGN KEY (TagId) REFERENCES Tags(Id) ON DELETE CASCADE
);
```

### 2. 密码安全

- **算法**: BCrypt (work factor = 11)
- **多密码支持**: JSON 数组存储多个哈希
- **首次使用**: 无密码时任意密码可通过

### 3. 性能优化

- **WAL 模式**: 提升并发读写性能
- **索引优化**: 为常用查询字段创建索引
- **内存缓存**: Settings 表懒加载到内存
- **Dapper ORM**: 轻量级高性能 ORM

---

## 🚀 下一步开发计划

### v0.2 - 文件导入功能
1. 实现 `IFileService` 接口
2. 拖拽导入支持
3. SHA-256 去重检测
4. UUID 文件重命名和移动
5. 缩略图生成

### v0.3 - 标签系统
1. 集成 jieba.NET 分词
2. 智能标签提取
3. 标签筛选功能
4. 多标签交集查询

### v0.4 - 文件浏览
1. 虚拟化列表
2. 分类导航
3. 排序功能
4. 搜索功能

---

## 📝 编译和运行

### 方法一: 使用批处理脚本
```bash
cd "c:\Users\Administrator\Desktop\文件管理器"
run.bat
```

### 方法二: 手动编译
```bash
# 1. 还原包
dotnet restore

# 2. 编译
dotnet build --configuration Debug

# 3. 运行
dotnet run --project src/VaultTags.App
```

### 方法三: Visual Studio
1. 双击打开 `VaultTags.sln`
2. 按 `F5` 启动调试

---

## 📊 代码统计

| 层级 | 文件数 | 代码行数 |
|------|--------|---------|
| Core | 11 | ~800 |
| Infrastructure | 5 | ~600 |
| UI | 7 | ~700 |
| App | 2 | ~100 |
| **总计** | **25** | **~2200** |

---

**最后更新**: 2026-04-15  
**版本**: v0.1  
**状态**: ✅ 基础框架完成

