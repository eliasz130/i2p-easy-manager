# Installation Guide

Complete installation instructions for I2P Easy Manager on all supported platforms.

---

## Table of Contents

- [System Requirements](#system-requirements)
- [macOS Installation](#macos-installation)
- [Linux Installation](#linux-installation)
- [Windows Installation](#windows-installation)
- [Verify Installation](#verify-installation)
- [Troubleshooting](#troubleshooting)

---

## System Requirements

### Minimum Requirements
- **Python:** 3.8 or higher
- **Firefox:** Any recent version (115+)
- **I2Pd:** 2.48 or higher
- **RAM:** 512MB minimum (1GB+ recommended)
- **Disk Space:** 200MB for application + I2P data
- **Network:** Active internet connection

### Supported Platforms
- macOS 11 (Big Sur) or later
- Ubuntu 20.04 LTS or later
- Debian 10 or later
- Arch Linux (rolling)
- Fedora 35 or later
- Windows 10 or later

---

## macOS Installation

### 1. Install Homebrew (if not installed)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. Install Dependencies

```bash
# Install Python 3
brew install python3

# Install I2Pd
brew install i2pd

# Install Firefox
brew install --cask firefox
# OR download from: https://www.mozilla.org/firefox/
```

### 3. Install I2P Easy Manager

**Option A: From PyPI (when published)**
```bash
pip3 install i2p-easy-manager
```

**Option B: From Source**
```bash
git clone https://github.com/tundra-node/i2p-easy-manager.git
cd i2p-easy-manager
pip3 install -e .
```

### 4. Verify Installation

```bash
i2p-manager --version
```

### 5. First Run

```bash
i2p-manager
```

---

## Linux Installation

### Ubuntu / Debian

#### 1. Update Package Lists
```bash
sudo apt update
```

#### 2. Install Dependencies
```bash
# Install Python 3 and pip
sudo apt install python3 python3-pip

# Install I2Pd
sudo apt install i2pd

# Install Firefox
sudo apt install firefox
```

#### 3. Install I2P Easy Manager
```bash
pip3 install i2p-easy-manager
```

#### 4. Add to PATH (if needed)
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

#### 5. Verify Installation
```bash
i2p-manager --version
```

### Arch Linux

#### 1. Install Dependencies
```bash
# Install base packages
sudo pacman -S python python-pip i2pd firefox
```

#### 2. Install I2P Easy Manager
```bash
pip install i2p-easy-manager
```

#### 3. Add to PATH (if needed)
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Fedora

#### 1. Install Dependencies
```bash
# Install Python and pip
sudo dnf install python3 python3-pip

# Install Firefox
sudo dnf install firefox

# Install I2Pd (from COPR)
sudo dnf copr enable salt-lick/i2pd
sudo dnf install i2pd
```

#### 2. Install I2P Easy Manager
```bash
pip3 install i2p-easy-manager
```

---

## Windows Installation

### 1. Install Python

1. Download Python 3.8+ from [python.org](https://www.python.org/downloads/)
2. Run installer
3. ✅ **IMPORTANT:** Check "Add Python to PATH"
4. Click "Install Now"

### 2. Install Firefox

1. Download from [mozilla.org/firefox](https://www.mozilla.org/firefox/)
2. Run installer
3. Complete installation

### 3. Install I2Pd

1. Download from [i2pd.website](https://i2pd.website/)
2. Extract to `C:\Program Files\i2pd\`
3. Run `i2pd.exe` to test

### 4. Install I2P Easy Manager

Open PowerShell or Command Prompt:

```powershell
pip install i2p-easy-manager
```

### 5. Verify Installation

```powershell
i2p-manager --version
```

### 6. First Run

```powershell
i2p-manager
```

---

## Verify Installation

### Check Dependencies

```bash
# Check Python version (should be 3.8+)
python3 --version

# Check if Firefox is installed
firefox --version

# Check if I2Pd is installed
i2pd --version

# Check I2P Manager installation
i2p-manager --version
```

### Test Basic Functionality

```bash
# Initialize (creates profile)
i2p-manager init

# Check status
i2p-manager status

# Start I2P
i2p-manager start
```

---

## Troubleshooting

### "Command not found: i2p-manager"

**macOS/Linux:**
```bash
# Check if pip installed to user directory
ls ~/.local/bin/i2p-manager

# Add to PATH
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**Windows:**
```powershell
# Check if Scripts directory is in PATH
$env:Path -split ';' | Select-String Python

# Add to PATH if missing (PowerShell as Admin)
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Users\YourName\AppData\Local\Programs\Python\Python3x\Scripts", "User")
```

### "Firefox not found"

**macOS:**
```bash
# Install Firefox
brew install --cask firefox
```

**Linux:**
```bash
# Ubuntu/Debian
sudo apt install firefox

# Arch
sudo pacman -S firefox
```

**Windows:**
Download and install from [mozilla.org/firefox](https://www.mozilla.org/firefox/)

### "I2Pd not found"

**macOS:**
```bash
brew install i2pd
```

**Ubuntu/Debian:**
```bash
sudo apt install i2pd
```

**Arch:**
```bash
sudo pacman -S i2pd
```

**Windows:**
Download from [i2pd.website](https://i2pd.website/)

### Permission Errors (Linux)

```bash
# Don't use sudo with pip
pip3 install --user i2p-easy-manager

# Add user to i2pd group (if exists)
sudo usermod -a -G i2pd $USER
newgrp i2pd
```

### Python Version Too Old

```bash
# Check version
python3 --version

# If < 3.8, update:

# Ubuntu/Debian (add deadsnakes PPA)
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.11

# macOS
brew install python@3.11

# Windows
# Download latest from python.org
```

### SSL Certificate Errors

```bash
# macOS
pip3 install --trusted-host pypi.org --trusted-host files.pythonhosted.org i2p-easy-manager

# Linux/Windows
pip install --upgrade pip
pip install --upgrade certifi
```

---

## Virtual Environment (Recommended for Development)

### Create Virtual Environment

```bash
# Create venv
python3 -m venv i2p-env

# Activate
source i2p-env/bin/activate  # macOS/Linux
i2p-env\Scripts\activate     # Windows

# Install
pip install i2p-easy-manager

# Deactivate when done
deactivate
```

---

## Uninstallation

### Remove I2P Easy Manager

```bash
pip uninstall i2p-easy-manager
```

### Remove Configuration

**macOS/Linux:**
```bash
rm -rf ~/.config/i2p-manager
```

**Windows:**
```powershell
Remove-Item -Recurse -Force "$env:APPDATA\i2p-manager"
```

### Remove Firefox Profile

```bash
i2p-manager reset
```

Or manually:

**macOS:**
```bash
rm -rf ~/Library/Application\ Support/Firefox/Profiles/*.i2p-secure
```

**Linux:**
```bash
rm -rf ~/.mozilla/firefox/*.i2p-secure
```

**Windows:**
```powershell
Remove-Item -Recurse -Force "$env:APPDATA\Mozilla\Firefox\Profiles\*.i2p-secure"
```

---

## Next Steps

After successful installation:

1. **Initialize:** Run `i2p-manager init`
2. **Start I2P:** Run `i2p-manager start`
3. **Read Usage Guide:** See [usage.md](usage.md)
4. **Join Community:** Visit http://i2pforum.i2p (via I2P)

---

## Getting Help

- **Documentation:** [usage.md](usage.md), [development.md](development.md)
- **Issues:** [GitHub Issues](https://github.com/yourusername/i2p-easy-manager/issues)
- **Community:** [GitHub Discussions](https://github.com/yourusername/i2p-easy-manager/discussions)