# I2P Easy Manager

A lightweight, user-friendly tool that simplifies I2P network access through automated I2Pd and Firefox configuration.

**Two versions available:**
- **CLI** - Interactive dashboard + command-line interface (this branch)
- **GUI** - Desktop application with system tray (separate branch)

---

## Project Philosophy

- **Lightweight** - Minimal dependencies, maximum efficiency
- **Clear & Simple** - Non-technical users should feel comfortable
- **Hardened by Default** - Maximum privacy without breaking I2P sites
- **Zero Cost** - Completely free and open source

---

## Quick Start

```bash
# Install dependencies (macOS)
brew install i2pd firefox node

# Install I2P Manager
npm install -g i2p-easy-manager

# Run interactive dashboard
i2p-manager

# Or use individual commands
i2p-manager start
i2p-manager status
```

---

## Features

### Interactive Dashboard Mode
Run `i2p-manager` with no arguments to launch a clean, interactive dashboard:

```
╔══════════════════════════════════════════════════════════╗
║              I2P EASY MANAGER v0.1.0                     ║
╚══════════════════════════════════════════════════════════╝

Status: ● CONNECTED
Peers:  156 routers
Tunnels: 8 active

[1] Start I2P              [5] View Configuration
[2] Stop I2P               [6] View Logs
[3] Restart I2P            [7] Reset Everything
[4] Launch I2P Browser     [8] Help & About

[Q] Quit

Choose an option:
```

### Command-Line Interface
All dashboard functions available as standalone commands:

```bash
i2p-manager start          # Start I2P + launch browser
i2p-manager stop           # Stop I2P
i2p-manager status         # Check connection
i2p-manager browser        # Launch I2P Firefox profile
i2p-manager logs           # View I2P router logs
i2p-manager config         # Edit configuration
i2p-manager reset          # Clean up everything
```

---

## Installation

### Prerequisites

**macOS:**
```bash
brew install i2pd firefox node
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install i2pd firefox nodejs npm
```

**Linux (Arch):**
```bash
sudo pacman -S i2pd firefox nodejs npm
```

### Install I2P Manager

**Option 1: NPM (Future)**
```bash
npm install -g i2p-easy-manager
i2p-manager
```

**Option 2: From Source**
```bash
git clone https://github.com/eliasz130/i2p-easy-manager.git
cd i2p-easy-manager
npm install
npm link
i2p-manager
```

---

## First Run

When you first run `i2p-manager`, it will:

1. ✓ Check for Firefox and I2Pd
2. ✓ Create dedicated Firefox profile
3. ✓ Apply maximum privacy hardening
4. ✓ Configure I2P proxy settings
5. ✓ Start I2P router

**Important:** First connection takes 10-30 minutes to integrate into the I2P network. This is normal.

---

## Privacy & Security

### Firefox Hardening Applied

Based on Arkenfox user.js with I2P-specific optimizations:

**Privacy Protections:**
- ✓ Fingerprinting resistance enabled
- ✓ WebRTC disabled (prevents IP leaks)
- ✓ WebGL disabled (prevents GPU fingerprinting)
- ✓ All telemetry disabled
- ✓ DNS over proxy enforced
- ✓ Third-party cookies blocked
- ✓ Tracking protection (strict mode)

**I2P-Specific Settings:**
- ✓ All traffic routed through I2P proxy (127.0.0.1:4444)
- ✓ DNS queries through I2P
- ✓ .i2p domain resolution enabled
- ✓ HTTP/HTTPS proxy configured
- ✓ No clearnet fallback (prevents leaks)

**What's NOT Hardened:**
- JavaScript enabled (required for most I2P sites)
- Cookies allowed for .i2p domains (needed for functionality)
- Canvas API available (many I2P sites require it)
- IndexedDB enabled (used by some I2P applications)

This configuration provides maximum security while keeping I2P sites fully functional.

---

## Usage

### Interactive Dashboard

Simply run:
```bash
i2p-manager
```

Navigate using number keys or arrow keys. The dashboard shows:
- Real-time connection status
- Peer count (network health indicator)
- Active tunnels
- Quick access to all functions

### Command-Line Mode

For scripts or quick operations:

```bash
# Start I2P and browser
i2p-manager start

# Check if connected
i2p-manager status

# Launch browser only (if I2P already running)
i2p-manager browser

# Stop I2P
i2p-manager stop

# View last 50 log lines
i2p-manager logs

# Follow logs in real-time
i2p-manager logs -f

# Edit configuration
i2p-manager config

# Complete reset (removes profile and config)
i2p-manager reset
```

---

## Configuration

### Default Settings

```json
{
  "i2pd": {
    "host": "127.0.0.1",
    "httpPort": 4444,
    "httpsPort": 4444,
    "socksPort": 4447,
    "consolePort": 7070
  },
  "firefox": {
    "profileName": "i2p-secure",
    "hardenWithArkenfox": true
  },
  "dashboard": {
    "refreshInterval": 5,
    "showWelcome": true
  }
}
```

### Customization

Edit config:
```bash
i2p-manager config
```

Or manually edit:
- **macOS/Linux:** `~/.config/i2p-manager/config.json`
- **Windows:** `%LOCALAPPDATA%\i2p-manager\config.json`

---

## Architecture

### Lightweight Design

Total size: ~15MB (including all dependencies)

**Core Components:**
1. **I2Pd Controller** - Manages router lifecycle
2. **Firefox Profile Manager** - Creates and configures hardened profile
3. **Interactive Dashboard** - Clean TUI for easy management
4. **CLI Commands** - Direct access to all functions

**Dependencies:**
- `blessed` - Terminal UI framework (dashboard)
- `blessed-contrib` - Dashboard widgets
- `chalk` - Terminal colors
- `commander` - CLI framework
- `fs-extra` - File operations
- `ora` - Loading spinners

No heavy frameworks, no bundled browsers, no bloat.

---

## Security Considerations

### What This Tool Does

✓ Configures Firefox to use I2P proxy  
✓ Applies privacy hardening to Firefox  
✓ Manages I2Pd router  
✓ Prevents common privacy leaks  

### What You Should Know

⚠️ **The I2P profile only protects I2P traffic**  
- Other Firefox profiles are not affected
- System traffic still uses regular internet
- Other applications are not proxied

⚠️ **I2P is not Tor**  
- Different network with different properties
- Primarily designed for hidden services (.i2p sites)
- Slower than Tor for regular web browsing

⚠️ **First connection takes time**  
- 10-30 minutes to build circuits
- Need 50+ peers for good connectivity
- Be patient during initial setup

---

## Troubleshooting

### Dashboard Won't Start

```bash
# Check dependencies
i2p-manager status

# Reset and try again
i2p-manager reset
i2p-manager
```

### I2Pd Won't Start

```bash
# Check if installed
which i2pd

# Try manual start
brew services start i2pd        # macOS
sudo systemctl start i2pd       # Linux

# Check logs
i2p-manager logs
```

### Can't Access .i2p Sites

1. Wait 10-30 minutes after first start
2. Check peer count: `i2p-manager status`
3. Need 50+ peers for good connectivity
4. Visit console: http://127.0.0.1:7070
5. Try restarting: `i2p-manager restart`

### Browser Opens but No Proxy

```bash
# Verify profile configuration
i2p-manager config

# Reinitialize profile
i2p-manager reset
i2p-manager
```

---

## Comparison: CLI vs GUI

| Feature            | CLI (This Branch) | GUI (Separate)  |
|--------------------|-------------------|-----------------|
| Dashboard          | Terminal-based    | Native window   |
| System Integration | Commands          | Menu bar/tray   |
| Resource Usage     | ~20MB RAM         | ~??MB RAM       |
| Startup Time       | Instant           | ? seconds       |
| Automation         | Easy (scriptable) | Limited         |
| User-Friendliness  | Good              | Excellent       |
| Platform Support   | All               | macOS/Linux/Win |

Choose CLI if you:
- Prefer terminal workflows
- Want to script I2P management
- Need minimal resource usage
- Work on servers/headless systems

Choose GUI if you:
- Want visual indicators
- Prefer point-and-click
- Need system tray integration
- Are new to command-line tools

---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md)

**Areas needing help:**
- Testing on different systems
- Windows support improvements
- Additional I2P site bookmarks
- Documentation improvements
- Translation to other languages

---

## Roadmap

### CLI v0.2 (Next Release)
- [ ] Auto-download latest Arkenfox
- [ ] Bandwidth monitoring in dashboard
- [ ] Tunnel statistics
- [ ] Built-in I2P site directory
- [ ] Profile backup/restore
- [ ] Multi-profile support

### GUI v0.1 (Separate Branch)
- [ ] Native desktop application
- [ ] System tray integration
- [ ] Visual connection status
- [ ] Built-in bookmark manager
- [ ] One-click installer

---

## Resources

- [I2P Network](https://geti2p.net/)
- [I2Pd Documentation](https://i2pd.readthedocs.io/)
- [Arkenfox user.js](https://github.com/arkenfox/user.js)
- [I2P Forum](http://i2pforum.i2p) - (Access via I2P)
- [Planet I2P](http://planet.i2p) - (Access via I2P)

---

## License

GPL-3.0 - See [LICENSE](LICENSE)

This ensures the project remains free and open source forever.

---

## Acknowledgments

- I2P and I2Pd development teams
- Arkenfox project for Firefox hardening
- The privacy community

---

## Support

- **Issues:** GitHub Issues
- **Discussions:** GitHub Discussions
- **I2P Forum:** http://i2pforum.i2p (via I2P)

---

## Disclaimer

This tool configures existing software (Firefox and I2Pd). Users are responsible for understanding the legal implications of anonymous networks in their jurisdiction. The developers assume no liability for misuse.

**Use responsibly and respect others' privacy.**