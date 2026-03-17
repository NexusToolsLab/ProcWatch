<p align="center">
  <img src="ProcWatch.png" alt="ProcWatch Logo" width="128" height="128">
</p>

<h1 align="center">ProcWatch</h1>

<p align="center">
  <strong>🔍 A modern cross-platform process monitor and manager</strong>
</p>

<p align="center">
  English | <a href="README_CN.md">中文</a>
</p>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#screenshots">Screenshots</a> •
  <a href="#installation">Installation</a> •
  <a href="#usage">Usage</a> •
  <a href="#development">Development</a> •
  <a href="#tech-stack">Tech Stack</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Go-1.21+-00ADD8?style=flat&logo=go" alt="Go Version">
  <img src="https://img.shields.io/badge/Platform-macOS%20%7C%20Windows-blue?style=flat" alt="Platform">
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat" alt="License">
</p>

---

## 📖 Overview

**ProcWatch** is a cross-platform process monitoring app built with Go and Wails. It provides a clean GUI for viewing, managing, and automatically terminating processes. Whether you are debugging or keeping your system tidy, ProcWatch makes it fast and simple.

### ✨ Highlights

- 🎨 **Modern UI** - polished interface with real-time data
- ⚡ **High performance** - Go-powered and lightweight
- 🖥️ **Cross-platform** - native support for macOS and Windows
- 🔄 **Live monitoring** - auto refresh with adjustable interval
- 🎯 **Smart rules** - wildcard matching for auto-kill policies
- 💾 **Persistent settings** - rules and preferences saved automatically

---

## 🚀 Features

### 📊 Real-time process monitoring

- Displays PID, name, CPU, memory, status, user, and more
- Sort by CPU, memory, PID, or name
- Paginated list for large process tables
- Accurate CPU sampling

### 🔍 Smart filtering and search

- CPU/memory threshold filtering
- Keyword search (process name, PID, user)
- Hide system processes
- Refresh interval control and auto-refresh toggle

### ⚙️ Auto-kill rules

- Wildcard `*` matching (e.g. `*chrome*`)
- Exact match mode
- CPU and memory thresholds (either triggers)
- Enable/disable per rule
- Rule testing (preview matches)
- Persistent rule storage

### 🎯 Process management

- Kill single process
- Batch kill selected processes
- Confirmation dialogs
- Process detail panel

### 🔔 Notifications

- Action success/failure notifications
- Auto-kill notifications
- Toggle notifications on/off

### ⚙️ Settings and persistence

- Auto-refresh and interval controls
- Settings persisted locally (filters, toggles, sorting)

---

## 📸 Screenshots

> Main view: process list, system stats, filters

<p align="center">
  <img src="docs/pic1.png" alt="Main View" width="800">
</p>

> Auto-kill rule configuration

<p align="center">
  <img src="docs/pic2.png" alt="Rule Configuration" width="400">
</p>

---

## 💻 Installation

### Download from Releases

Visit the [Releases](https://github.com/NexusToolsLab/ProcWatch/releases) page and download the installer for your platform:

- **macOS**: `ProcWatch-x.x.x.dmg` or `ProcWatch-x.x.x.zip`
- **Windows**: `ProcWatch-x.x.x-installer.exe`

### Build from source

#### Requirements

- [Go](https://golang.org/dl/) 1.21 or later
- [Node.js](https://nodejs.org/) 16 or later
- [Wails](https://wails.io/docs/gettingstarted/installation) CLI

```bash
# Install Wails CLI
go install github.com/wailsapp/wails/v2/cmd/wails@latest

# Clone the repo
git clone https://github.com/NexusToolsLab/ProcWatch.git
cd ProcWatch

# Install frontend dependencies
cd frontend && npm install && cd ..

# Run in dev mode
wails dev

# Build for production
wails build
```

Build output is in `build/bin/`.

---

## 📚 Usage

### Basics

1. **View processes** - launch the app to load the list automatically
2. **Search** - use the search bar for quick filtering
3. **Filter** - adjust CPU/memory thresholds in the sidebar
4. **Kill** - click the kill button or batch kill selected rows

### Auto refresh

1. Enable **Auto refresh** in the sidebar
2. Adjust the interval (1-30 seconds)
3. Settings persist across restarts

### Auto-kill rules

#### Add a rule

1. Click **+ Add Rule**
2. Configure:
   - **Process name**: supports wildcards like `*chrome*`, `node`, `*helper*`
   - **CPU threshold**: trigger when CPU exceeds this value (0 = no limit)
   - **Memory threshold**: trigger when memory exceeds this value (0 = no limit)
   - **Exact match**: require full name match

#### Rule examples

| Process Name | CPU Threshold | Memory Threshold | Exact Match | Description |
| ------------ | ------------- | ---------------- | ----------- | ----------- |
| `*chrome*`   | 80%           | 0%               | No          | Match Chrome-related processes |
| `node`       | 90%           | 50%              | Yes         | Only match process named node |
| `*helper*`   | 0%            | 0%               | No          | Match any helper process and kill immediately |

#### Enable auto-kill

1. Make sure the rule is enabled (eye icon highlighted)
2. Toggle **Enable auto-kill**
3. The app checks every 5 seconds and terminates matches

#### Test rules

Click the 🔍 icon on a rule card to preview matched processes.

### Rule persistence

Rules are saved to your user directory:

- **macOS**: `~/.procwatch_rules.json`
- **Windows**: `C:\Users\<username>\.procwatch_rules.json`

### Settings persistence

Filters and toggles are stored locally and restored on startup.

---

## 🛠️ Development

### Project structure

```
ProcWatch/
├── app.go                 # Go backend logic
├── main.go               # App entry
├── wails.json            # Wails config
├── ProcWatch.png         # App icon
├── frontend/
│   ├── src/
│   │   ├── main.js       # Frontend logic
│   │   └── style.css     # Styles
│   ├── index.html        # HTML entry
│   └── wailsjs/          # Wails generated bindings
├── build/
│   ├── appicon.png       # App icon (PNG)
│   ├── appicon.icns      # macOS icon
│   ├── windows/
│   │   └── icon.ico      # Windows icon
│   └── darwin/
│       └── Info.plist    # macOS config
└── docs/
    └── screenshots/      # Screenshots
```

### Commands

```bash
# Dev mode (hot reload)
wails dev

# Generate bindings
wails generate module

# Build for macOS
wails build -platform darwin/amd64
wails build -platform darwin/arm64

# Build for Windows
wails build -platform windows/amd64

# Build all targets
wails build -platform darwin/amd64,darwin/arm64,windows/amd64
```

### Add new features

1. Add Go methods in `app.go`
2. Run `wails dev` to generate JS bindings
3. Call methods from `frontend/src/main.js`

---

## 🔧 Tech Stack

### Backend

- **[Go](https://golang.org/)** - high-performance systems language
- **[Wails v2](https://wails.io/)** - desktop framework for Go
- **[gopsutil](https://github.com/shirou/gopsutil)** - cross-platform system utilities

### Frontend

- **Vanilla JavaScript** - lightweight and fast
- **Vite** - modern frontend build tool
- **CSS3** - modern UI styling

### Capabilities

- Responsive design for different screen sizes
- Native window experience
- Cross-platform (macOS, Windows)

---

## 🤝 Contributing

Issues and pull requests are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 Changelog

### v1.1.2 (2026-03-17)

- ✨ Added auto-refresh settings and interval modal
- 💾 Persisted common settings (refresh interval, toggles, filters, sorting)
- 🧭 Added "About ProcWatch" modal with external links

### v1.1.1 (2026-03-17)

- 🐛 Fixed GitHub API 403 during update checks (added User-Agent)
- 🔧 Migrated to [NexusToolsLab/ProcWatch](https://github.com/NexusToolsLab/ProcWatch)

### v1.1.0 (2026-03-16)

- ✨ Initial release
- ✅ Real-time process monitoring
- ✅ Auto-kill rules
- ✅ Rule persistence
- ✅ Cross-platform support

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙏 Acknowledgements

- [Wails](https://wails.io/) - excellent Go desktop framework
- [gopsutil](https://github.com/shirou/gopsutil) - powerful system monitoring library
- [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) - beautiful font

---

<p align="center">
  Made with ❤️ by <a href="https://github.com/NexusToolsLab">NexusToolsLab</a>
</p>
