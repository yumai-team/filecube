# 老板键与托盘图标功能设计

**日期**: 2026-06-17
**项目**: 文件酷 (FileCube / VaultTags)
**状态**: 已确认，待实施

---

## 1. 功能概述

为文件酷添加"老板键"和"系统托盘图标"功能，实现隐蔽运行能力：
- 用户可随时通过全局热键隐藏/唤出窗口
- 窗口关闭时默认隐藏到托盘而非退出
- 托盘图标提供快速访问和状态指示

---

## 2. 技术方案

### 2.1 选定方案

**H.NotifyIcon.Wpf + Win32 RegisterHotKey**

| 组件 | 技术 | 说明 |
|------|------|------|
| 托盘图标 | H.NotifyIcon.Wpf (NuGet) | 现代活跃分支，XAML原生托盘菜单，HiDPI图标支持 |
| 全局热键 | Win32 RegisterHotKey (P/Invoke) | 自行封装，零额外依赖 |

### 2.2 NuGet 依赖

```
H.NotifyIcon.Wpf  (最新稳定版)
```

### 2.3 不选用方案

- **Hardcodet.NotifyIcon.Wpf**: 已多年未更新，与.NET 8有兼容警告
- **纯Win32 API**: 需自行处理HiDPI和菜单，维护成本高

---

## 3. 核心行为契约

### 3.1 老板键

**默认快捷键**: `Ctrl+Shift+H`

| 行为 | 说明 |
|------|------|
| 全局生效 | 即使文件酷未获焦也能触发 |
| 切换显隐 | 窗口可见→隐藏到托盘；已隐藏→唤出并聚焦 |
| 可配置 | 设置中可关闭或修改快捷键 |
| 默认启用 | 首次启动即生效 |

### 3.2 托盘图标

| 交互 | 行为 |
|------|------|
| 单击 | 切换窗口显隐（与老板键同效果） |
| 双击 | 唤出窗口并聚焦到前台 |
| 右键菜单 | 见下表 |

**右键菜单项**:

```
打开文件酷          → 唤出窗口
隐藏/显示           → 动态文案，与当前状态一致
────────────
设置...             → 打开设置页（快捷键子页）
────────────
退出                → 真正退出应用
```

### 3.3 窗口关闭按钮行为

| 原行为 | 新行为 |
|--------|--------|
| 点击× → 退出进程 | 点击× → 隐藏到托盘，进程继续运行 |

**真正退出途径**:
1. 托盘菜单"退出"
2. 设置页"退出应用"按钮
3. 命令行 `app.exe --exit`（可选）

### 3.4 退出语义

退出时执行现有 `OnExit` 流程：
- 数据库 checkpoint（WAL写入磁盘）
- 日志 flush
- 缩略图缓存清理
- 数据库连接关闭

---

## 4. 架构设计

### 4.1 新增组件

| 文件 | 位置 | 职责 |
|------|------|------|
| `TrayService.cs` | VaultTags.Infrastructure/Services | 托盘图标生命周期管理 |
| `HotKeyService.cs` | VaultTags.Infrastructure/Services | 全局热键注册/注销 |
| `AppTrayIcon.xaml` | VaultTags.App | 托盘图标XAML定义（含菜单） |
| `AppTrayIcon.xaml.cs` | VaultTags.App | 托盘图标代码后台 |

### 4.2 组件关系

```
Program.cs (App入口)
    ├── TrayService (托盘图标管理)
    │       └── H.NotifyIcon.Wpf
    │       └── AppTrayIcon.xaml (托盘UI)
    │
    └── HotKeyService (全局热键)
    │       └── RegisterHotKey P/Invoke
    │
    └── MainWindow
            └── Closing事件拦截 → 隐藏而非退出
```

### 4.3 与现有代码集成点

| 集成点 | 文件 | 修改内容 |
|--------|------|----------|
| 启动初始化 | `Program.cs` OnStartup | 初始化TrayService和HotKeyService |
| 关闭拦截 | `MainWindow.xaml.cs` Closing | 拦截关闭，改为隐藏 |
| 设置持久化 | `SettingRepository` | 新增键：BossKeyEnabled, BossKeyShortcut, TrayIconEnabled |
| ShutdownMode | `Program.cs` | 改为 `OnExplicitShutdown`（托盘退出时才真正关闭） |

---

## 5. 数据流

### 5.1 老板键触发流程

```
用户按下 Ctrl+Shift+H
    ↓
HotKeyService 收到 WM_HOTKEY
    ↓
HotKeyService 触发 ToggleWindowVisibility 事件
    ↓
Program.cs 处理事件
    ↓
if (MainWindow.Visible)
    MainWindow.Hide()
    Remove from Taskbar & Alt-Tab
else
    MainWindow.Show()
    MainWindow.Activate()
```

### 5.2 托盘图标交互流程

```
托盘图标单击
    ↓
TrayService.ToggleMainWindow()
    ↓
同老板键流程

托盘菜单"退出"
    ↓
TrayService.RequestExit()
    ↓
Program.cs 执行 OnExit 流程
    ↓
Application.Current.Shutdown()
```

---

## 6. 设置项

### 6.1 新增设置键

| 键名 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `BossKeyEnabled` | bool | true | 是否启用老板键 |
| `BossKeyShortcut` | string | "Ctrl+Shift+H" | 老板键快捷键组合 |
| `TrayIconEnabled` | bool | true | 是否显示托盘图标 |
| `CloseToTray` | bool | true | 关闭窗口时隐藏到托盘 |

### 6.2 设置界面位置

在现有"设置"页新增子页：
- **快捷键设置**（可扩展为未来更多快捷键）
  - 老板键开关
  - 老板键快捷键（下拉选择或自定义输入）
  - 关闭到托盘开关

---

## 7. 国际化

### 7.1 新增资源键

| 键名 | 中文 | 英文 |
|------|------|------|
| `TrayMenuOpen` | 打开文件酷 | Open FileCube |
| `TrayMenuHide` | 隐藏 | Hide |
| `TrayMenuShow` | 显示 | Show |
| `TrayMenuSettings` | 设置... | Settings... |
| `TrayMenuExit` | 退出 | Exit |
| `BossKeySettings` | 老板键设置 | Boss Key Settings |
| `EnableBossKey` | 启用老板键 | Enable Boss Key |
| `BossKeyShortcut` | 老板键快捷键 | Boss Key Shortcut |
| `CloseToTray` | 关闭时隐藏到托盘 | Hide to tray on close |
| `TrayIconEnabled` | 显示托盘图标 | Show tray icon |

---

## 8. 错误处理

| 场景 | 处理 |
|------|------|
| 热键注册失败（冲突） | 提示用户选择其他快捷键 |
| 托盘图标创建失败 | 日志记录，禁用托盘功能，窗口行为不变 |
| 托盘退出时数据库checkpoint失败 | 日志记录，仍退出（不阻塞用户） |

---

## 9. 测试要点

| 测试项 | 验证 |
|--------|------|
| 老板键全局生效 | 窗口最小化/其他应用获焦时仍能触发 |
| 托盘图标显隐 | 单击/双击正确切换窗口状态 |
| 关闭按钮行为 | 点击×隐藏而非退出 |
| 托盘退出 | 数据库正确checkpoint，日志完整 |
| 快捷键冲突 | 检测并提示用户 |
| 设置持久化 | 重启后设置生效 |
| HiDPI图标 | 高分辨率下托盘图标清晰 |

---

## 10. 实施顺序

1. 添加 H.NotifyIcon.Wpf NuGet 依赖
2. 实现 HotKeyService（P/Invoke封装）
3. 实现 TrayService + AppTrayIcon.xaml
4. 修改 Program.cs 启动流程
5. 修改 MainWindow Closing 行为
6. 添加设置项和设置界面
7. 添加国际化资源
8. 测试与调试

---

## 11. 风险与缓解

| 风险 | 缓解措施 |
|------|----------|
| 热键与其他应用冲突 | 提示用户修改，提供常用备选快捷键 |
| 托盘图标在某些Windows版本异常 | 使用H.NotifyIcon.Wpf（已处理Win10/11兼容） |
| 用户误以为关闭=退出 | 托盘图标提示气泡（首次启动） |

---

**设计确认**: 用户已于 2026-06-17 确认此设计，进入实施阶段。