<p align="center">
  <img alt="IchigoWM Logo" src="https://github.com/user-attachments/assets/5ec2f388-a4c0-423e-bae7-59daa4422cad" width="920" />
</p>

# Ichigo Window Manager

> **一期** - *ichigo* - "one time, one meeting" - making every window arrangement perfect

Ichigo is a lightweight, efficient tiling window manager for Windows, inspired by the Japanese philosophy of appreciating each moment. Built in pure C using Windows APIs for maximum performance and minimal resource usage.

## 🌟 Features

### 🏗️ **Multiple Layout Algorithms**
- **BSP (Binary Space Partitioning)** - Recursive splits for optimal space usage
- **Master-Stack** - i3/dwm style with configurable master area  
- **Grid** - Automatic grid arrangement for multiple windows
- **Floating** - Traditional window management when needed

### ⌨️ **Intuitive Hotkeys**
- `Win + H` - Force horizontal split (BSP)
- `Win + V` - Force vertical split (BSP)
- `Win + F` - Toggle fullscreen
- `Win + T` - Toggle floating mode
- `Win + Space` - Cycle through layouts
- `Win + M` - Move window to next monitor
- `Win + R` - Refresh/retile current monitor

### 🖥️ **Multi-Monitor Support**
- Independent layouts per monitor
- Automatic window placement
- Cross-monitor window movement
- Per-monitor configuration

### ⚙️ **Highly Configurable**
- Text-based configuration file (`ichigo.conf`)
- Window class exclusions
- Customizable gaps and borders
- Layout-specific settings
- Real-time configuration reload

## 🚀 Installation

### Build from Source

**Prerequisites:**
- MinGW-w64 or Visual Studio Build Tools
- Git (optional)

```bash
# Clone the repository
git clone https://github.com/yourusername/ichigo.git
cd ichigo

# Build release version
make

# Build debug version (with console output)
make debug

# Build and run immediately
make dev

# Install to system (requires administrator privileges)
make install
```

### Pre-built Releases
Download the latest release from the [releases page](https://github.com/yourusername/ichigo/releases).

## ⚡ Quick Start

1. **Download or build** `ichigo.exe`
2. **Create** `ichigo.conf` in the same directory (or use defaults)
3. **Run** `ichigo.exe` 
4. **Use hotkeys** to manage your windows:
   - `Win + Space` to cycle layouts
   - `Win + F` for fullscreen
   - `Win + T` for floating mode

## 📁 Project Structure

```
ichigo/
├── include/
│   └── ichigo.h              # Core data structures and APIs
├── src/
│   ├── main.c                # Entry point and main loop
│   ├── window_manager.c      # Core window management logic
│   ├── layouts.c             # Layout algorithms (BSP, master-stack, grid)
│   ├── monitor.c             # Multi-monitor support
│   ├── hotkeys.c             # Keyboard handling and shortcuts
│   └── config.c              # Configuration file parsing
├── obj/                      # Build artifacts (created during build)
├── bin/                      # Final executable (created during build)
├── ichigo.conf               # Configuration file
├── Makefile                  # Build system
└── README.md                 # This file
```

## ⚙️ Configuration

Ichigo looks for `ichigo.conf` in the same directory as the executable. If not found, it uses sensible defaults.

### Example Configuration
```ini
# Window gaps in pixels
gap_size=8

# Enable automatic tiling
auto_tile=true

# Exclude specific window classes from tiling
exclude_class=Calculator
exclude_class=TaskManagerWindow

# Master-stack layout settings
master_ratio=0.6
master_count=1
```

### Finding Window Class Names

**Method 1: Debug Mode**
```bash
ichigo.exe -d
```
Watch the console output to see window classes as they're detected.

**Method 2: PowerShell**
```powershell
Get-Process | Where-Object {$_.MainWindowTitle -ne ""} | Select-Object ProcessName,MainWindowTitle
```

**Method 3: Spy++ (Visual Studio)**
Use the finder tool to inspect any window's properties.

## 🎮 Usage

### Starting Ichigo
```bash
# Run in foreground with console output
ichigo.exe

# Run with custom config file
ichigo.exe -c myconfig.conf

# Run in debug mode (shows detailed logging)
ichigo.exe -d

# Show help
ichigo.exe -h
```

### Layouts

#### BSP (Binary Space Partitioning)
Recursively divides screen space, creating balanced layouts that adapt to any number of windows. Similar to i3 or bspwm.

#### Master-Stack
Traditional tiling with a master area (60% by default) and a stack area for remaining windows. Similar to dwm or xmonad.

#### Grid
Arranges windows in a uniform grid, perfect for monitoring multiple applications simultaneously.

#### Floating
Disables tiling for traditional window management. Individual windows can also be set to floating mode.

### Hotkeys Reference

| Hotkey | Action | Description |
|--------|---------|-------------|
| `Win + H` | Horizontal Split | Apply BSP layout with horizontal preference |
| `Win + V` | Vertical Split | Apply BSP layout with vertical preference |
| `Win + F` | Toggle Fullscreen | Make active window fullscreen or return to tiling |
| `Win + T` | Toggle Floating | Set active window to floating or return to tiling |
| `Win + Space` | Cycle Layouts | Switch between BSP → Master-Stack → Grid → Floating |
| `Win + M` | Move to Monitor | Move active window to next monitor |
| `Win + R` | Refresh | Re-apply current layout to all windows |

## 🏗️ Architecture

### Core Components

#### Window Hook System
Uses Windows Shell hooks (`WH_SHELL`) to monitor window events:
- `HSHELL_WINDOWCREATED` - Detects new windows
- `HSHELL_WINDOWDESTROYED` - Handles window closure
- `HSHELL_WINDOWACTIVATED` - Tracks focus changes

#### Layout Engine
Each layout algorithm operates on a per-monitor basis:
- **BSP**: Recursive binary partitioning with alternating split directions
- **Master-Stack**: Configurable master area with vertical stack
- **Grid**: Dynamic grid calculation based on window count

#### Multi-Monitor Management
- Automatic monitor detection using `EnumDisplayMonitors`
- Per-monitor layout state and window tracking
- Work area calculation respecting taskbar and other system UI

#### Configuration System
- INI-style configuration file parsing
- Runtime validation of all settings
- Support for comments and multiple value formats
- Graceful fallback to defaults for invalid values

## 🛠️ Development

### Building

**Debug Build (recommended for development):**
```bash
make debug
```
- Includes debugging symbols
- Enables console logging
- No optimization for easier debugging

**Release Build:**
```bash
make
# or
make release
```
- Optimized for performance
- Minimal size
- No debug output

**Clean Build:**
```bash
make clean
make
```

### Adding New Layouts

1. **Add layout type** to `LayoutType` enum in `ichigo.h`
2. **Implement layout function** in `layouts.c`:
   ```c
   void layout_my_new_layout(IchigoMonitor* monitor) {
       // Your layout algorithm here
   }
   ```
3. **Add case** to `apply_layout()` switch statement
4. **Update hotkey handler** for layout cycling in `hotkeys.c`

### Adding New Hotkeys

1. **Define hotkey ID** in `hotkeys.c`
2. **Add key detection** in `keyboard_hook_proc()` in `window_manager.c`
3. **Implement handler** in `handle_hotkey()` in `hotkeys.c`

### Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes with clear commit messages
4. Test thoroughly on different Windows versions
5. Submit a pull request with detailed description

## 📊 Performance

Ichigo is designed for minimal system impact:

| Metric | Value |
|--------|-------|
| **Memory Usage** | < 1MB RAM |
| **CPU Usage** | < 0.1% (event-driven) |
| **Startup Time** | < 100ms |
| **Window Response** | < 5ms positioning |
| **Dependencies** | None (pure Windows APIs) |

## 🔧 System Requirements

- **OS**: Windows 10/11 (64-bit recommended, 32-bit supported)
- **RAM**: 1MB minimum
- **CPU**: Any x86/x64 processor
- **Permissions**: Standard user (administrator for system install)

## 🐛 Troubleshooting

### Windows Not Tiling
1. **Check exclusions**: Verify window class isn't in `ichigo.conf`
2. **Window style**: Ensure window has `WS_CAPTION` and is resizable
3. **Visibility**: Window must be visible and not minimized
4. **Debug mode**: Run with `-d` flag to see detection logs

### Hotkeys Not Working
1. **Conflicting software**: Check for other programs using Win+ combinations
2. **Administrator mode**: Some security software requires elevated privileges
3. **Hook installation**: Check console for hook installation errors

### Multi-Monitor Issues
1. **Display changes**: Restart Ichigo after connecting/disconnecting monitors
2. **Work area**: Check if taskbar position affects window placement
3. **Monitor detection**: Use debug mode to verify monitor enumeration

### Performance Issues
1. **Window count**: Large numbers of windows (>50) may slow layout calculation
2. **Rapid changes**: Minimize window creation/destruction frequency
3. **System resources**: Ensure adequate RAM and CPU availability

## 📝 License

MIT License - see LICENSE file for details.

## 🙏 Acknowledgments

- Inspired by **i3**, **dwm**, **bspwm**, and **Hyprland**
- Thanks to the Windows API documentation
- Japanese philosophy of **ichigo ichie** (一期一会)

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/lucassilvasoftware/ichigo/issues)
- **Discussions**: [GitHub Discussions](https://github.com/lucassilvasoftware/ichigo/discussions)
- **Documentation**: This README and inline code comments

---

**Built with ❤️ for productivity and elegance**

> *"一期一会" - Every window arrangement is a unique moment to be treasured*
