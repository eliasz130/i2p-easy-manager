# I2P Easy Manager

A user-friendly configuration manager that simplifies access to the I2P (Invisible Internet Project) network by automating I2Pd setup and Firefox configuration.

---

## Project Goals

- **Simple Setup** - One-click installation and configuration of I2P
- **Beginner-Friendly** - Intuitive interface for users new to I2P
- **Privacy-Focused** - Hardened Firefox profile with maximum security
- **Zero Cost** - No paid dependencies or services required
- **Lightweight** - Configuration manager, not a full browser bundle

---

## Project Status

**Pre-Alpha / Planning Stage**

This project is in early development. Contributions and feedback are welcome!

---

## Architecture

### Core Components

1. **I2Pd Router Manager** - Controls I2Pd daemon lifecycle
2. **Firefox Profile Configurator** - Creates and manages hardened Firefox profile
3. **Control Interface** - Simple UI for I2P management
4. **Auto-Configuration** - Automated setup and optimization

### What This App Does

Instead of bundling a browser, this lightweight manager:

- Installs and manages I2Pd
- Creates a dedicated Firefox profile configured for I2P
- Applies hardened Firefox settings (Arkenfox/Betterfox)
- Installs and configures uBlock Origin with maximum blocking
- Provides simple start/stop controls
- Launches Firefox with the I2P profile automatically

### Technology Stack

- **I2P Implementation**: [I2Pd](https://github.com/PurpleI2P/i2pd) (bundled)
- **Browser**: User's existing Firefox installation (auto-configured)
- **UI Framework**: Electron or Tauri (TBD)
- **Languages**: JavaScript/TypeScript, Shell scripting
- **Primary Platform**: macOS (Apple Silicon optimized)
- **Cost**: $0 - Completely free and open source

---

## Planned Features

### Phase 1 - Configuration Manager (MVP)

- [ ] Bundled I2Pd installation (Apple Silicon native)
- [ ] I2Pd lifecycle management (start/stop/status)
- [ ] Automated Firefox profile creation
- [ ] I2P proxy configuration (HTTP: 4444, HTTPS: 4445)
- [ ] Arkenfox user.js integration
- [ ] uBlock Origin auto-install with maximum blocking presets
- [ ] Connection status indicator
- [ ] macOS menu bar integration
- [ ] Simple "Start I2P Browser" button

### Phase 2 - Enhanced Management

- [ ] Address book management
- [ ] Built-in I2P site directory (.i2p bookmarks)
- [ ] Bandwidth controls
- [ ] Tunnel manager
- [ ] Network status dashboard
- [ ] Auto-update checker for I2Pd
- [ ] Multiple profile support

### Phase 3 - Advanced Features

- [ ] Custom tunnel creation
- [ ] Advanced I2Pd configuration GUI
- [ ] Backup/restore profiles
- [ ] Cross-platform support (Windows, Linux)
- [ ] Optional: Embedded browser (if resources allow)

---

## Firefox Configuration Details

### Hardened Profile Setup

The manager creates a dedicated Firefox profile with:

**Base Configuration:**

- [Arkenfox user.js](https://github.com/arkenfox/user.js) - Comprehensive privacy/security hardening
- OR [Betterfox](https://github.com/yokoffing/Betterfox) - Balance of privacy and usability
- Custom I2P proxy settings (PAC file or manual config)

**uBlock Origin Configuration:**

- Auto-installed from Mozilla Add-ons
- Maximum blocking mode enabled:
  - Block all 3rd-party scripts (NoScript-like behavior)
  - Block all 3rd-party frames
  - Block all large media elements
  - Hard mode enabled
  - All filter lists enabled
- Custom I2P-specific filters (if needed)

**Additional Extensions (Optional):**

- CanvasBlocker (fingerprinting protection)
- Cookie AutoDelete
- LocalCDN

**Firefox Settings Applied:**

```javascript
// Proxy configuration
user_pref("network.proxy.type", 1);
user_pref("network.proxy.http", "127.0.0.1");
user_pref("network.proxy.http_port", 4444);
user_pref("network.proxy.ssl", "127.0.0.1");
user_pref("network.proxy.ssl_port", 4444);
user_pref("network.proxy.no_proxies_on", "");

// Privacy hardening (subset from Arkenfox)
user_pref("privacy.resistFingerprinting", true);
user_pref("webgl.disabled", true);
user_pref("network.dns.disablePrefetch", true);
user_pref("network.prefetch-next", false);
// ... (full list in configuration files)
```

---

## System Requirements

### User Requirements

- Firefox must be installed on the system
- No prior I2P knowledge needed
- Minimal technical expertise required

### System Requirements

- **OS**: macOS 12 (Monterey) or later
- **Chip**: Apple Silicon (M1/M2/M3) or Intel
- **RAM**: 512MB minimum, 1GB+ recommended
- **Storage**: 200MB for application + I2P data
- **Network**: Active internet connection
- **Firefox**: Version 115+ installed

---

## Development Setup

### Prerequisites

**Install Xcode Command Line Tools:**

```bash
xcode-select --install
```

**Install Homebrew (if not already installed):**

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Install required dependencies:**

```bash
# Install build tools and libraries for I2Pd
brew install cmake openssl boost git

# For Apple Silicon, ensure you're using ARM64 versions
arch -arm64 brew install cmake openssl boost

# Install Node.js for Electron development
brew install node

# Optional: Install Nix for reproducible development environment
# See: https://nixos.org/download.html
sh <(curl -L https://nixos.org/nix/install)
```

---

### Building I2Pd for macOS (Apple Silicon)

```bash
# Clone I2Pd
git clone https://github.com/PurpleI2P/i2pd.git
cd i2pd

# Build for Apple Silicon
make HOMEBREW=1

# Test the build
./i2pd --help
```

---

### Project Setup

#### Option 1: Using Nix (Recommended)

If you have Nix with flakes enabled:

```bash
# Clone this repository
git clone https://github.com/yourusername/i2p-easy-manager.git
cd i2p-easy-manager

# Enter development environment (automatically installs all dependencies)
nix develop

# Download required resources
nix run .#download-arkenfox
nix run .#download-extensions

# Install Node.js dependencies
npm install

# Run development version
npm run dev
```

**Using direnv (Optional but Recommended):**

```bash
# Create .envrc file
echo "use flake" > .envrc
direnv allow

# Now the environment loads automatically when you cd into the directory
```

**Enable Nix Flakes (if not already enabled):**

```bash
# Add to ~/.config/nix/nix.conf or /etc/nix/nix.conf
mkdir -p ~/.config/nix
echo "experimental-features = nix-command flakes" >> ~/.config/nix/nix.conf
```

#### Option 2: Manual Setup (Without Nix)

```bash
# Clone this repository
git clone https://github.com/yourusername/i2p-easy-manager.git
cd i2p-easy-manager

# Install Node.js dependencies
npm install

# Download Arkenfox user.js
curl -o resources/configs/arkenfox-user.js https://raw.githubusercontent.com/arkenfox/user.js/master/user.js

# Download uBlock Origin
# (Manual download from https://addons.mozilla.org/firefox/addon/ublock-origin/)

# Build I2Pd (see Building I2Pd section above)

# Run development version
npm run dev
```

---

### Firefox Profile Management

```bash
# Find Firefox profiles directory on macOS
~/Library/Application Support/Firefox/Profiles/

# The app will create a new profile named "i2p-profile.default"
# Located at: ~/Library/Application Support/Firefox/Profiles/xxxxxxxx.i2p-profile/
```

---

### Nix Flake Commands

If using Nix, these convenience commands are available:

```bash
# Enter development shell with all dependencies
nix develop

# Run development server
nix run .#dev

# Build I2Pd binary
nix run .#build-i2pd

# Download Firefox extensions (uBlock Origin)
nix run .#download-extensions

# Download Arkenfox user.js
nix run .#download-arkenfox

# Build the entire application
nix build
```

---

## Project Structure

```
i2p-easy-manager/
├── docs/                       # Documentation
│   ├── architecture.md         # System architecture
│   ├── user-guide.md           # End-user documentation
│   ├── development.md          # Developer guide
│   └── firefox-hardening.md    # Firefox configuration details
│
├── src/
│   ├── main/                   # Electron main process
│   │   ├── i2pd-manager.js     # I2Pd lifecycle control
│   │   ├── firefox-config.js   # Firefox profile setup
│   │   └── profile-hardener.js # Apply Arkenfox/Betterfox
│   │
│   ├── renderer/               # Electron renderer (UI)
│   │   ├── components/         # UI components
│   │   └── App.jsx             # Main application
│   │
│   └── utils/                  # Utilities and helpers
│
├── resources/
│   ├── icons/                  # Application icons (.icns)
│   │
│   ├── configs/                # Configuration files
│   │   ├── arkenfox-user.js    # Arkenfox user.js
│   │   ├── i2p-overrides.js    # I2P-specific Firefox prefs
│   │   ├── ublock-settings.json # uBlock Origin config
│   │   └── i2pd.conf           # Default I2Pd configuration
│   │
│   ├── i2pd-bin/               # Bundled I2Pd binaries
│   │   ├── arm64/              # Apple Silicon binary
│   │   └── x86_64/             # Intel binary
│   │
│   └── extensions/             # Browser extensions (.xpi files)
│       └── ublock-origin.xpi   # uBlock Origin installer
│
├── scripts/                    # Build and utility scripts
│   ├── build-i2pd.sh           # Build I2Pd for bundling
│   ├── build-macos.sh          # macOS app build script
│   ├── package-dmg.sh          # DMG creation script
│   └── download-extensions.sh  # Fetch Firefox extensions
│
├── tests/                      # Test suite
│
├── .github/
│   └── workflows/              # GitHub Actions for CI/CD
│
├── flake.nix                   # Nix flake for reproducible builds
├── flake.lock                  # Locked Nix dependencies
├── LICENSE
├── README.md
└── CONTRIBUTING.md
```

---

## macOS-Specific Features

### Planned Integrations

- **Menu Bar App** - Quick I2P start/stop, status at a glance
- **Dock Integration** - Connection status badge
- **Notification Center** - I2P connection notifications
- **LaunchAgent** - Optional start on login
- **Spotlight** - Quick launch via Spotlight search

### Distribution (Free)

- Unsigned .dmg for direct download (users bypass Gatekeeper with right-click > Open)
- GitHub Releases for distribution
- No App Store (requires $99/year developer account)
- Future: Homebrew cask for easy installation

---

## Contributing

Contributions are welcome! This project is in early stages, so there's plenty of opportunity to shape its direction.

### How to Contribute

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Areas Needing Help

- I2Pd integration and automation
- Firefox profile management and hardening
- UI/UX design (macOS Human Interface Guidelines)
- Testing on different Mac models
- Documentation and user guides
- Extension configuration automation
- Shell scripting for setup automation

---

## Resources

### I2P Network

- [I2P Official Website](https://geti2p.net/)
- [I2Pd Project](https://github.com/PurpleI2P/i2pd)
- [I2P Documentation](https://geti2p.net/en/docs)

### Firefox Hardening

- [Arkenfox user.js](https://github.com/arkenfox/user.js) - Privacy & security hardening
- [Betterfox](https://github.com/yokoffing/Betterfox) - Alternative hardening approach
- [uBlock Origin Wiki](https://github.com/gorhill/uBlock/wiki) - Advanced blocking
- [Firefox Profile Manager](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles)

### macOS Development

- [Electron Documentation](https://www.electronjs.org/docs/latest)
- [Tauri Documentation](https://tauri.app/)
- [macOS App Distribution Guide](https://developer.apple.com/distribute/)

### Nix

- [Nix Flakes](https://nixos.wiki/wiki/Flakes) - Reproducible development environments
- [Nix Pills](https://nixos.org/guides/nix-pills/) - Learn Nix
- [Zero to Nix](https://zero-to-nix.com/) - Quick start guide

### Privacy & Security

- [I2P Threat Model](https://geti2p.net/en/docs/how/threat-model)
- [I2P Compared to Tor](https://geti2p.net/en/comparison/tor)

---

## License

TBD - To be determined (GPL-3.0, MIT, or Apache-2.0)

Likely **GPL-3.0** to align with I2Pd and ensure the project remains free and open source.

---

## Acknowledgments

- The I2P development community
- I2Pd developers
- Arkenfox project for Firefox hardening
- uBlock Origin developers
- Tor Browser project (for inspiration)

---

## Contact

- **Issues**: Use GitHub Issues for bug reports and feature requests
- **Discussions**: Use GitHub Discussions for questions and ideas

---

## Known Considerations

- **Unsigned App**: Users will see Gatekeeper warning (right-click > Open to bypass)
- **Firefox Required**: App won't work without Firefox pre-installed
- **Profile Conflicts**: Users should not manually modify the I2P Firefox profile
- **First Launch**: I2Pd takes 10-30 minutes to integrate into network (this is normal)

---

## Future Possibilities

If the project grows and funding becomes available:

- Embedded browser option
- iOS/iPadOS companion app
- Code signing certificate ($99/year)
- Mac App Store distribution
- Windows and Linux versions

---

## Disclaimer

This software is provided as-is. Users are responsible for understanding the legal implications of using anonymous networks in their jurisdiction. The developers assume no liability for misuse.

**Note**: This is a configuration manager, not a fork or modification of Firefox or I2Pd. It simply automates setup and configuration of these existing tools.