# FileCube — Rethinking File Management

<div align="center">

✨ **File management, reimagined.**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![.NET](https://img.shields.io/badge/.NET-8.0-purple.svg)](https://dotnet.microsoft.com/)
[![Platform](https://img.shields.io/badge/platform-Windows-lightgrey.svg)](https://www.microsoft.com/windows)
[![Release](https://img.shields.io/badge/release-v0.9-brightgreen.svg)](https://github.com/yumai-team/filecube/releases)
[![Download](https://img.shields.io/badge/download-Portable-orange.svg)](https://yumai-team.github.io/filecube/)

> **Codename**: VaultTags
>
> 🌐 **Website**: [yumai-team.github.io/filecube](https://yumai-team.github.io/filecube/)
>
> 📦 **GitHub Releases**: [github.com/yumai-team/filecube/releases](https://github.com/yumai-team/filecube/releases)

</div>

---

## 📖 Overview

FileCube is a local file management tool built for Windows. Its core idea is simple yet powerful: **replace nested folder hierarchies with multi-label tagging**, so organizing files feels effortless and intuitive.

**Why ditch folders?**

- A single file often belongs in multiple contexts. A vacation photo might be "Travel," "2025," and "Family" all at once — but a traditional folder forces you to pick just one and forget the rest.
- For the organizationally minded, the eternal question of "which folder is the *right* folder?" never ends.
- People resort to duplicating files across directories just to make them findable from different paths, leading to rampant file duplication.
- Deeply nested trees slow down retrieval. Forgetting which level something lives in is the norm, not the exception.

**How FileCube solves it:**

- 🏷️ **Multi-label tagging** — Assign any number of tags to a file. Classify once, discover through multiple dimensions. No more folder dilemmas.
- 🧹 **Content deduplication** — Files with identical content share a single physical copy on disk (SHA-256 hash-based). Tags aggregate them logically — duplication is eliminated at the root.
- 🔎 **Tag intersection + keyword search** — Filter by intersecting multiple tags and pair with original filename search. Faster than clicking through folder after folder.
- 🔒 **Privacy by design** — Files are stored under UUID names on disk, unreadable and meaningless to anyone browsing the raw storage.

### ✨ Key Features

- 🏷️ **Multi-label tagging** — Files aren't forced into a single folder; assign as many tags as you like
- 🧹 **Content deduplication** — SHA-256 hash comparison. Identical content stored only once. Say goodbye to duplicate files forever.
- 🔎 **Tag intersection filtering** — Combine tags to narrow results. A smarter alternative to drilling down through nested directories.
- 🔐 **Password protection** — Multi-password login with BCrypt hashing
- 🤖 **Smart tagging** — Automatic keyword extraction for quick tag suggestions
- 🔒 **Privacy protection** — UUID-renamed storage; raw files are opaque to external inspection
- 🖼️ **Built-in preview** — Image viewer with zoom and fullscreen support
- 🎬 **Multimedia support** — Video and audio playback, plus thumbnail generation
- 🗑️ **Trash / soft-delete** — Recoverable deletion before permanent removal
- ⚡ **High performance** — Smooth operation with 100,000+ files

---

## 🚀 Getting Started

### System Requirements

- **OS**: Windows 10 21H2+ / Windows 11 (x64)
- **.NET Runtime**: .NET 8.0 LTS
- **RAM**: 4 GB minimum, 8 GB recommended
- **Disk**: At least 500 MB free (excluding file storage)

### Installation

1. **Download the latest release**
   - 🟢 Website: [yumai-team.github.io/filecube](https://yumai-team.github.io/filecube/)
   - 📦 Direct download: [FileCube.zip](https://github.com/yumai-team/filecube/releases/download/v0.9/FileCube.zip)
   - Or browse [GitHub Releases](https://github.com/yumai-team/filecube/releases)

2. **Extract and run** (portable — no installer needed)
   ```
   # Extract anywhere you like
   cd FileCube
   ./FileCube.exe
   ```

3. **Set your password on first launch**
   - You'll be prompted to create a password the first time you run the app
   - This password secures access to your files

---

## 📂 Project Structure

```
VaultTags/
├── src/
│   ├── VaultTags.App/              # WPF application entry point
│   │   ├── App.xaml                # Application definition
│   │   ├── Program.cs              # Entry class
│   │   └── VaultTags.App.csproj    # Project file
│   │
│   ├── VaultTags.Core/             # Core business logic
│   │   ├── Models/                 # Data models
│   │   ├── Services/               # Service interfaces
│   │   ├── Repositories/           # Repository interfaces
│   │   └── Utilities/              # Utility classes
│   │
│   ├── VaultTags.Infrastructure/   # Infrastructure layer
│   │   ├── Data/                   # Data access implementations
│   │   ├── Storage/                # Storage services
│   │   └── Logging/                # Logging services
│   │
│   └── VaultTags.UI/               # UI layer
│       ├── Views/                  # Windows and controls
│       ├── ViewModels/             # View models
│       ├── Converters/             # Value converters
│       └── Styles/                 # Style resources
│
├── tests/                          # Unit tests (planned)
├── docs/                           # Documentation (planned)
├── publish/                        # Build output (portable release)
└── README.md                       # This file
```

---

## 🛠️ Development

> **Note**: Source code is hosted in an internal repository. The GitHub repository is used for releases and issue tracking only.

### Building from Source

1. **Clone the repository**
   ```bash
   git clone <source-repo-url>
   cd VaultTags
   ```

2. **Restore dependencies**
   ```bash
   dotnet restore
   ```

3. **Build**
   ```bash
   dotnet build --configuration Release
   ```

4. **Run**
   ```bash
   dotnet run --project src/VaultTags.App
   ```

### Tech Stack

| Category | Technology |
|----------|------------|
| **Framework** | .NET 8.0 + WPF |
| **Architecture** | MVVM |
| **Database** | SQLite + Dapper |
| **MVVM Toolkit** | CommunityToolkit.Mvvm |
| **Password Hashing** | BCrypt.Net-Next |
| **Chinese Tokenization** | jieba.NET (to be integrated) |
| **Logging** | Serilog |

---

## 📋 Current Status

### ✅ v0.9 — Current Release

- [x] Project scaffolding
- [x] Database initialization
- [x] Password login with multi-password support
- [x] Main window layout
- [x] Logging infrastructure
- [x] Utility classes
- [x] Drag-and-drop file import
- [x] SHA-256 content deduplication
- [x] UUID-renamed file storage
- [x] Multi-label tag management
- [x] Tag intersection filtering and search
- [x] Image, video, and audio preview/playback
- [x] Trash with soft-delete
- [x] Portable packaging

### 🚧 Roadmap

- [ ] File export / Save as
- [ ] Automatic Chinese word-segmentation tagging
- [ ] Thumbnail grid view
- [ ] Batch tag operations
- [ ] Keyboard shortcuts

---

## 📸 Screenshots

*(Coming soon)*

---

## 🔧 Configuration

### Data Storage Layout

All data lives under the application directory:

```
FileCube/
├── Data/              # Original files (UUID-named, no extension)
├── Thumbs/            # Thumbnails (UUID-named, no extension)
├── Logs/              # Logs (split by date)
└── VaultTags.db       # SQLite database
```

### Backup & Restore

**Backup**: Copy the entire `FileCube` directory — that's it.

**Restore**: Overwrite the target location with your backup directory.

---

## ❓ FAQ

### Q: I forgot my password. What now?
There is no password recovery in the current version. You can reset by deleting `VaultTags.db`, but **be aware this wipes all your data**.

### Q: Where do imported files actually go?
Files are moved into the `Data/` directory and renamed to UUIDs. The original location is not preserved. This is by design — it protects your privacy.

### Q: How do I export a file?
Right-click a file and choose "Save As" to pick a destination and filename.

### Q: Why not just use regular folders?
Because folders force a single-parent hierarchy. A vacation photo can't simultaneously live under "Travel," "2025," and "Family" — unless you copy it three times, creating needless duplicates. FileCube solves this with **multi-label tagging**: one file, many labels, one physical copy.

### Q: What file formats are supported?
- **Images**: JPG, PNG, GIF, BMP, WebP, TIFF
- **Video**: MP4, AVI, MKV, MOV, WMV, FLV
- **Audio**: MP3, WAV, FLAC, AAC, OGG, WMA

### Q: Is it free?
Completely free. No ads, no bundled junk. It's a portable app — extract and run, no registry entries.

### Q: Can I sync across multiple computers?
Sure — copy the entire `FileCube` directory to a USB drive or a cloud-sync folder, and run it directly from there.

---

## 🤝 Contributing

- 🐛 **Report a bug**: [GitHub Issues](https://github.com/yumai-team/filecube/issues)
- 💡 **Suggest a feature**: [GitHub Issues](https://github.com/yumai-team/filecube/issues)
- ⭐ **Show support**: Star [yumai-team/filecube](https://github.com/yumai-team/filecube)!

> Source code is hosted internally. The GitHub repository is used for distribution and issue tracking.

---

## 📝 License

MIT License — see [LICENSE](LICENSE)

---

## 📮 Contact

| Channel | Link |
|---------|------|
| 🌐 **Website** | [yumai-team.github.io/filecube](https://yumai-team.github.io/filecube/) |
| 📦 **Downloads** | [GitHub Releases](https://github.com/yumai-team/filecube/releases) |
| 🐛 **Feedback** | [GitHub Issues](https://github.com/yumai-team/filecube/issues) |
| 🏠 **Organization** | [yumai-team.github.io](https://yumai-team.github.io/) |

---

<div align="center">

**Made with ❤️ by Yumai Team**

⭐ If FileCube is useful to you, give us a star!

</div>
