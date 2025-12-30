# I2P Easy Manager

> A lightweight, user-friendly tool that simplifies access to the I2P (Invisible Internet Project) network through automated I2Pd and Firefox configuration.

[![License: GPL-3.0](https://img.shields.io/badge/License-GPL%203.0-blue.svg)](LICENSE)
[![Python: 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Platform: Cross-Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Linux%20%7C%20Windows-lightgrey.svg)](https://github.com/yourusername/i2p-easy-manager)

---

## 🌟 Features

- **Interactive Dashboard** - Clean terminal UI with real-time status updates
- **One-Command Setup** - Automated Firefox profile creation and hardening
- **Maximum Privacy** - WebRTC disabled, fingerprinting protection, no telemetry
- **I2P Optimized** - Pre-configured for I2P network with proper proxy settings
- **Cross-Platform** - Works on macOS, Linux, and Windows
- **Lightweight** - Only ~5MB with 4 dependencies
- **Zero Cost** - Completely free and open source

---

## 📸 Screenshots

### Interactive Dashboard
```
╔══════════════════════════════════════════════════════════╗
║              I2P EASY MANAGER v0.1.0                     ║
╚══════════════════════════════════════════════════════════╝

┌─ Connection Status ────┐  ┌─ Network Info ──────────────┐
│ Status: ● CONNECTED    │  │ Known Peers: 156            │
│ Router: Running        │  │ Active Tunnels: 8           │
│ Proxy: 127.0.0.1:4444  │  │ ✓ Fully Integrated          │
└────────────────────────┘  └─────────────────────────────┘

[1] Start I2P              [5] View Configuration
[2] Stop I2P               [6] View Logs
[3] Restart I2P            [7] Reset Everything
[4] Launch I2P Browser     [8] Help & About

[Q] Quit
```

---

## 🚀 Quick Start

### Installation

**Prerequisites:**
- Python 3.8 or higher
- Firefox browser
- I2Pd daemon

**Install I2P Manager:**
```bash
pip install i2p-easy-manager
```

**Run Interactive Dashboard:**
```bash
i2p-manager
```

That's it! The dashboard will guide you through first-time setup.

---

## 📦 Installation Guide

### macOS

```bash
# Install dependencies
brew install python3 i2pd firefox

# Install I2P Manager (Future)
pip3 install i2p-easy-manager

# Run
i2p-manager
```

### Linux (Ubuntu/Debian)

```bash
# Install dependencies
sudo apt update
sudo apt install python3 python3-pip i2pd firefox

# Install I2P Manager (Future)
pip3 install i2p-easy-manager

# Run
i2p-manager
```

### Linux (Arch)

```bash
# Install dependencies
sudo pacman -S python python-pip i2pd firefox

# Install I2P Manager (Future)
pip install i2p-easy-manager

# Run
i2p-manager
```

### Windows

1. Install [Python 3.8+](https://www.python.org/downloads/) (check "Add to PATH")
2. Install [Firefox](https://www.mozilla.org/firefox/)
3. Install [I2Pd](https://i2pd.website/)
4. Open PowerShell or Command Prompt: (Future)

```powershell
pip install i2p-easy-manager
i2p-manager
```

---

## 💻 Usage

### Interactive Dashboard (Default)

Simply run `i2p-manager` to launch the interactive dashboard:

```bash
i2p-manager
```

**Dashboard Controls:**
- Press `1-8` to select menu options
- Press `R` to refresh status
- Press `Q` to quit

### Command-Line Interface

All dashboard functions are available as standalone commands:

```bash
# Initialize I2P profile (first time only)
i2p-manager init

# Start I2P and launch Firefox
i2p-manager start

# Check connection status
i2p-manager status

# Launch Firefox with I2P profile (if I2P already running)
i2p-manager browser

# Stop I2P daemon
i2p-manager stop

# Restart I2P daemon
i2p-manager restart

# View I2P router logs
i2p-manager logs
i2p-manager logs -f    # Follow logs in real-time

# Edit configuration
i2p-manager config

# Reset everything (removes profile and config)
i2p-manager reset

# Show help
i2p-manager --help
```

---

## 🔒 Security & Privacy

### What's Protected

I2P Easy Manager applies **maximum hardening** to your Firefox profile:

**Critical Protections:**
- ✅ **WebRTC disabled** - Prevents IP leaks through proxy
- ✅ **WebGL disabled** - Prevents GPU fingerprinting
- ✅ **All DNS through proxy** - No DNS leaks
- ✅ **No telemetry** - Zero data sent to Mozilla
- ✅ **No safe browsing** - No external lookups
- ✅ **Fingerprinting resistance** - Makes your browser look generic

**Privacy Features:**
- ✅ Third-party cookies blocked
- ✅ Tracking protection (strict mode)
- ✅ HTTPS-only mode enabled
- ✅ Geolocation disabled
- ✅ No prefetching or speculation

**I2P Compatibility:**
- ✅ JavaScript enabled (required for I2P sites)
- ✅ Canvas enabled (many sites need it)
- ✅ Cookies allowed for .i2p domains
- ✅ Connection optimization for I2P latency

### What's NOT Protected

⚠️ **Important:**
- Only the I2P Firefox profile is hardened
- Other Firefox profiles are not affected
- System traffic still uses regular internet
- Other applications are not proxied

---

## ⚙️ Configuration

### Default Settings

Configuration is stored at:
- **macOS/Linux:** `~/.config/i2p-manager/config.json`
- **Windows:** `%APPDATA%\i2p-manager\config.json`

**Default configuration:**
```json
{
  "i2pd": {
    "host": "127.0.0.1",
    "http_port": 4444,
    "https_port": 4444,
    "socks_port": 4447,
    "console_port": 7070
  },
  "firefox": {
    "profile_name": "i2p-secure",
    "harden_with_arkenfox": true
  },
  "dashboard": {
    "refresh_interval": 5,
    "show_welcome": true
  }
}
```

### Customization

Edit configuration:
```bash
i2p-manager config
```

Or manually edit the JSON file above.

---

## 🔧 Troubleshooting

### I2Pd Won't Start

```bash
# Check if I2Pd is installed
which i2pd  # macOS/Linux
where i2pd  # Windows

# Try starting manually
brew services start i2pd        # macOS
sudo systemctl start i2pd       # Linux
# On Windows, run i2pd.exe from install directory

# Check logs
i2p-manager logs
```

### Firefox Won't Launch

```bash
# Verify Firefox is installed
which firefox  # macOS/Linux
where firefox  # Windows

# Reinitialize profile
i2p-manager reset
i2p-manager init
```

### Can't Access .i2p Sites

1. **Wait 10-30 minutes** after first I2Pd start (network integration)
2. **Check peer count:** Run `i2p-manager status` - need 50+ peers
3. **Visit console:** http://127.0.0.1:7070 to verify I2P is working
4. **Try restarting:** Run `i2p-manager restart`

### Dashboard Display Issues

- Ensure terminal window is at least 80x24 characters
- Try using command-line mode instead: `i2p-manager start`

### Permission Errors (Linux)

```bash
# Add your user to i2pd group (if it exists)
sudo usermod -a -G i2pd $USER
newgrp i2pd
```

---

## 🌐 Getting Started with I2P

### First Connection

**⏳ First time takes 10-30 minutes**

When you first run I2P, the router needs to:
1. Find other routers on the network
2. Build encrypted tunnels
3. Integrate into the network

This is **normal and expected**. Monitor progress:
```bash
i2p-manager status
```

### I2P Sites to Try

Once connected (50+ peers), visit:

- **http://planet.i2p** - I2P news aggregator
- **http://i2pforum.i2p** - Community forum
- **http://127.0.0.1:7070** - Your I2P router console

### Understanding Peer Count

- **< 10 peers:** Still connecting, wait longer
- **10-50 peers:** Integrating into network
- **50+ peers:** Fully integrated, ready to browse
- **100+ peers:** Excellent connectivity

---

## 🛠️ Development

### Install from Source

```bash
# Clone repository
git clone https://github.com/yourusername/i2p-easy-manager.git
cd i2p-easy-manager

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install in editable mode with dev dependencies
pip install -e ".[dev]"

# Run
i2p-manager
# Or directly: python -m i2p_manager
```

### Run Tests

```bash
pytest
pytest --cov=i2p_manager  # With coverage
```

### Code Formatting

```bash
black i2p_manager/
ruff check i2p_manager/
```

### Project Structure

```
i2p-easy-manager/
├── i2p_manager/          # Main package
│   ├── cli.py           # CLI interface
│   ├── dashboard.py     # Interactive TUI
│   ├── config.py        # Configuration
│   ├── firefox.py       # Firefox manager
│   ├── i2pd.py          # I2Pd manager
│   ├── commands/        # CLI commands
│   └── data/            # Static files (Arkenfox)
├── tests/               # Test suite
├── docs/                # Documentation
└── pyproject.toml       # Package config
```

---

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Areas needing help:**
- Testing on different systems
- Windows support improvements
- Documentation improvements
- Additional I2P site bookmarks
- Translation to other languages

---

## 📚 Resources

### I2P Network
- [Official I2P Website](https://geti2p.net/)
- [I2Pd Documentation](https://i2pd.readthedocs.io/)
- [I2P Compared to Tor](https://geti2p.net/en/comparison/tor)

### Privacy & Security
- [Arkenfox user.js](https://github.com/arkenfox/user.js)
- [I2P Threat Model](https://geti2p.net/en/docs/how/threat-model)

### Python Packaging
- [Python Packaging Guide](https://packaging.python.org/)
- [Click Documentation](https://click.palletsprojects.com/)
- [Rich Documentation](https://rich.readthedocs.io/)

---

## 📋 Requirements

- **Python:** 3.8 or higher
- **Firefox:** Any recent version
- **I2Pd:** 2.48 or higher
- **OS:** macOS 11+, Linux (Ubuntu 20.04+, Arch, Fedora), Windows 10+

**Python Dependencies:**
- `click` - CLI framework
- `rich` - Terminal UI
- `requests` - HTTP client
- `psutil` - Process management

---

## 📄 License

This project is licensed under the **GNU General Public License v3.0** - see the [LICENSE](LICENSE) file for details.

This ensures the project remains free and open source forever.

---

## ⚠️ Disclaimer

This tool configures existing software (Firefox and I2Pd). Users are responsible for understanding the legal implications of anonymous networks in their jurisdiction.

**The developers assume no liability for misuse.**

**Use responsibly and respect others' privacy.**

---

## 🙏 Acknowledgments

- The I2P and I2Pd development teams
- Arkenfox project for Firefox hardening
- The privacy and security community
- All contributors to this project

---

## 📬 Support

- **Issues:** [GitHub Issues](https://github.com/yourusername/i2p-easy-manager/issues)
- **Discussions:** [GitHub Discussions](https://github.com/yourusername/i2p-easy-manager/discussions)
- **I2P Forum:** http://i2pforum.i2p (accessible via I2P network)

---

## 🌟 Star History

If you find this project useful, please consider giving it a star on GitHub!

---

**Welcome to the Invisible Internet! 🚀**