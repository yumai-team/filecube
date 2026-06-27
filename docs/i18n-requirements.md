# 多语言国际化需求规范（v1.2 / 2026-06-21 更新）

> 本次更新新增：**多语言扩展（至少 3 种内置语言）**、**主题/外观资源字典管理**、**文件导出功能文本国际化**。

## 背景

本次翻译工作中发现项目存在以下多语言处理不当的问题：
1. MessageBox、StatusText 等 UI 文本硬编码为中文
2. XAML 中的部分 TextBlock 文本硬编码为中文
3. 快捷键帮助面板等文本未接入语言资源
4. 主题颜色配置尚未设计资源字典结构
5. 文件导出功能的 UI 文本尚未定义多语言 key

## 需求规范

### 1. 核心原则（强制要求）

> **⚠️ 铁律：永远不要在程序中硬编码任何用户可见的文本。**
>
> **所有用户可见的文本必须通过语言资源管理，程序中禁止出现任何硬编码字符串。**

违反此原则的代码将被拒绝合并。

用户可见文本包括但不限于：
- MessageBox 的消息内容和标题
- StatusBar 的状态文本
- XAML 中直接写的 TextBlock.Text
- 按钮、标签、提示文本
- 快捷键帮助面板的说明文字
- 公司名称、版权信息
- 托盘菜单与气泡提示
- 文件导出对话框 / 进度对话框 / 结果报告
- 设置页面中主题与外观相关文本

### 2. 代码层面要求

#### 2.1 MessageBox 和 StatusText

```csharp
// ❌ 禁止：硬编码中文
MessageBox.Show("文件不存在", "错误", MessageBoxButton.OK, MessageBoxImage.Error);
StatusText = "正在加载...";

// ✅ 正确：通过语言资源
MessageBox.Show(
    Application.Current?.Resources["FileNotFound"]?.ToString() ?? "File not found",
    Application.Current?.Resources["Error"]?.ToString() ?? "Error",
    MessageBoxButton.OK, MessageBoxImage.Error);
StatusText = Application.Current?.Resources["Loading"]?.ToString() ?? "Loading...";
```

#### 2.2 XAML 文本

```xml
<!-- ❌ 禁止：硬编码中文 -->
<TextBlock Text="移入回收站" />

<!-- ✅ 正确：使用 DynamicResource -->
<TextBlock Text="{DynamicResource MoveToTrash}" />
```

#### 2.3 字符串拼接

```csharp
// ❌ 禁止：拼接中文字符串
$"确定要删除文件「{fileName}」吗？"

// ✅ 正确：使用格式化字符串资源
string.Format(
    Application.Current?.Resources["ConfirmDeleteFile"]?.ToString() ?? "Delete \"{0}\"?",
    fileName)
```

### 3. 语言资源文件结构

```
Resources/
  Languages/
    zh-CN.xaml      // 简体中文（默认/基准语言，key 集合的源头）
    en-US.xaml      // 英文
    ja-JP.xaml      // 日语（新增，2026-06-21）
    ko-KR.xaml      // 韩语（预留，后续版本）
    de-DE.xaml      // 德语（预留，后续版本）
  Themes/
    Light.xaml      // 浅色主题（颜色/画刷 key，非文本）
    Dark.xaml       // 深色主题（颜色/画刷 key，非文本）
  Accents/
    Blue.xaml       // 主色调：经典蓝（默认）
    Green.xaml      // 森林绿
    Red.xaml        // 玫瑰红
    Purple.xaml     // 极光紫
    Orange.xaml     // 金秋橙
    Gray.xaml       // 石墨灰
```

> **重要：zh-CN.xaml 是基准语言文件。** 任何新 key 必须先在 zh-CN.xaml 中定义，再同步到所有其他语言文件。缺失的 key 在运行时会回退到 zh-CN。

资源文件示例：
```xml
<!-- en-US.xaml -->
<system:String x:Key="FileNotFound">File not found</system:String>
<system:String x:Key="Error">Error</system:String>
<system:String x:Key="MoveToTrash">Move to Trash</system:String>
<system:String x:Key="ConfirmDelete">Are you sure you want to delete "{0}"?</system:String>

<!-- zh-CN.xaml -->
<system:String x:Key="FileNotFound">文件不存在</system:String>
<system:String x:Key="Error">错误</system:String>
<system:String x:Key="MoveToTrash">移入回收站</system:String>
<system:String x:Key="ConfirmDelete">确定要删除「{0}」吗？</system:String>

<!-- ja-JP.xaml（新增） -->
<system:String x:Key="FileNotFound">ファイルが見つかりません</system:String>
<system:String x:Key="Error">エラー</system:String>
<system:String x:Key="MoveToTrash">ゴミ箱へ移動</system:String>
<system:String x:Key="ConfirmDelete">ファイル「{0}」を削除しますか？</system:String>
```

### 3.1 LanguageManager 扩展要求

- `LanguageManager` 应自动枚举所有 `Resources/Languages/*.xaml`，而不是硬编码"仅中英双语"
- `SupportedLanguages` 属性返回 `List<(string Code, string DisplayName)>`，显示名从资源中读取（例如 `LanguageName_zh-CN`、`LanguageName_en-US`）
- `SetLanguage(code)` 切换后写入 `Settings/appsettings.json`，key: `AppLanguage`
- 若语言资源加载失败，自动回退到 zh-CN，再回退到 `Program.cs` fallbackDict，并在日志中记录"缺失 key 列表"

### 3.2 主题与主色调（颜色资源，非文本）

以下是主题资源字典中必须定义的颜色 / 画刷 key（用于所有界面样式的 `DynamicResource` 引用）：

| Key | 用途 |
| --- | --- |
| `BackgroundColor` | 主窗口背景色 |
| `SurfaceColor` | 卡片/面板/对话框背景色 |
| `TextColor` | 主要文本颜色 |
| `SecondaryTextColor` | 次要/提示文本颜色 |
| `BorderColor` | 控件边框与分隔线颜色 |
| `AccentColor` | 主色调（按钮、选中态、链接） |
| `AccentHoverColor` | 主色调悬停态 |
| `AccentPressedColor` | 主色调按下态 |
| `DangerColor` | 危险操作（删除、清空）颜色 |
| `SuccessColor` | 成功/确认颜色 |
| `FocusColor` | 键盘焦点矩形颜色 |
| `DisabledOpacity` | 禁用态不透明度 |

> 主题切换使用 `Application.Current.Resources.MergedDictionaries` 的统一替换：先移除旧的 `Themes/*.xaml` 和 `Accents/*.xaml`，再添加新的，整体过程在一次 dispatcher 操作中完成，避免闪烁。

### 4. 枚举值和多值文本

快捷键、操作说明等多值文本建议使用统一前缀便于管理：

```xml
<system:String x:Key="Shortcut_PreviousNext">Previous / Next</system:String>
<system:String x:Key="Shortcut_PlayPause">Play / Pause</system:String>
```

### 4.1 文件导出功能的文本 key 规范

文件导出相关的 UI 文本使用 `Export_` 前缀（所有语言文件必须同步）：

| Key | 说明（zh-CN 样例） |
| --- | --- |
| `Export_SaveAs` | 另存为 |
| `Export_BatchExport` | 批量导出 |
| `Export_ExportByTag` | 按标签导出 |
| `Export_SelectFolder` | 请选择导出目录 |
| `Export_Exporting` | 正在导出... |
| `Export_Progress` | 已导出 {0}/{1} |
| `Export_CurrentFile` | 当前文件：{0} |
| `Export_Cancel` | 取消 |
| `Export_Success` | 导出成功 |
| `Export_Failed` | 导出失败 |
| `Export_ResultSummary` | 成功 {0}，跳过 {1}，失败 {2} |
| `Export_FileExists` | 文件已存在，是否覆盖？ |
| `Export_ConfirmTrashExport` | 此文件在回收站中，仍要导出吗？ |
| `Export_StrategyRename` | 自动重命名 |
| `Export_StrategyOverwrite` | 覆盖 |
| `Export_StrategySkip` | 跳过 |

### 4.2 主题功能的文本 key 规范

主题相关文本使用 `Theme_` 前缀：

| Key | 说明（zh-CN 样例） |
| --- | --- |
| `Theme_Appearance` | 外观 |
| `Theme_Light` | 浅色 |
| `Theme_Dark` | 深色 |
| `Theme_FollowSystem` | 跟随系统 |
| `Theme_AccentColor` | 主色调 |
| `Theme_RestartToApply` | 重启后生效 |

### 5. 新功能开发流程

任何新增功能必须遵循以下流程：

1. **设计阶段**：确定所有用户可见文本（对话框、按钮、状态、错误提示）
2. **资源文件**：先在 zh-CN.xaml 定义 key，再同步到 en-US.xaml、ja-JP.xaml，以及所有预留语言文件（即便翻译暂用英文占位，也要保证 key 存在）
3. **颜色资源**：涉及主题的控件颜色仅在 `Themes/` 和 `Accents/` 中定义，不在 XAML 中写死颜色值
4. **编码阶段**：代码中引用资源 key，不硬编码文本；颜色一律引用 `DynamicResource`
5. **自检阶段**：提交前检查是否有遗漏的硬编码文本与颜色值；运行一次 `en-US` 语言切换，查看所有界面是否正常显示

### 6. 审查清单（强制执行）

代码提交前**必须**确认以下所有条目，否则拒绝合并：

- [ ] **所有** MessageBox.Show() 的文本已接入语言资源
- [ ] **所有** StatusText 赋值已接入语言资源
- [ ] XAML 中**所有**用户可见的 TextBlock.Text 已接入语言资源
- [ ] 资源文件包含所有必要的 key（含新增的 `Export_*`、`Theme_*`）
- [ ] **所有**语言文件（zh-CN、en-US、ja-JP）的 key 集合同步一致
- [ ] XAML 中不出现硬编码颜色值（`#FFxxxxxx`），全部通过 `DynamicResource` 引用主题 key
- [ ] Program.cs 的 fallbackDict 包含所有 key 的英文兜底值
- [ ] 代码中不存在任何用户可见的硬编码文本（中文/英文/日文都不允许）
- [ ] `Export_` 和 `Theme_` 前缀的 key 已在所有语言文件中定义

### 7. 常见遗漏点（需特别注意）

- 快捷键帮助面板的说明文字
- 工具提示（Tooltip）
- 上下文菜单项
- 对话框按钮文本
- 公司名称、版权信息
- 占位符文本
- 错误提示信息
- 托盘菜单 / 气泡提示
- 文件导出对话框、进度对话框、结果报告
- 设置页面中主题/外观相关文本
- 列表视图与缩略图视图中的空白提示（"无文件"等）

## 附录 A：fallback 机制

如果语言资源缺失，应确保有合理的 fallback 逻辑：

```csharp
// Program.cs 中的 fallback 字典
fallbackDict.Add("Key", "Default English Text");
```

但 fallback 不应替代正确的资源管理，仅作为保险机制。

## 附录 B：缺失 key 自检脚本（建议）

在开发环境中，建议添加一个"启动时自检"调试开关（仅在 DEBUG 构建下启用）：

- 比较 zh-CN.xaml 与 en-US.xaml 的 key 集合差异
- 比较 zh-CN.xaml 与 ja-JP.xaml 的 key 集合差异
- 将缺失的 key 输出到日志文件 `Logs/MissingKeys.log`，方便翻译者补全
- 不阻塞程序启动，仅在调试输出窗口提示
