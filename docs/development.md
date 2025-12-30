# Development Guide

Guide for contributing to and developing I2P Easy Manager.

---

## Table of Contents

- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Development Workflow](#development-workflow)
- [Testing](#testing)
- [Code Style](#code-style)
- [Adding Features](#adding-features)
- [Debugging](#debugging)
- [Release Process](#release-process)

---

## Quick Start

### 1. Fork and Clone

```bash
# Fork on GitHub, then clone
git clone https://github.com/YOUR_USERNAME/i2p-easy-manager.git
cd i2p-easy-manager
```

### 2. Setup Development Environment

```bash
# Create virtual environment
python -m venv venv

# Activate
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows

# Install in editable mode with dev dependencies
pip install -e ".[dev]"
```

### 3. Verify Setup

```bash
# Run the tool
i2p-manager --version
python -m i2p_manager --version

# Run tests
pytest

# Format code
black i2p_manager/

# Lint
ruff check i2p_manager/
```

---

## Project Structure

```
i2p-easy-manager/
├── i2p_manager/              # Main package
│   ├── __init__.py          # Package version and exports
│   ├── __main__.py          # Entry point for python -m
│   ├── cli.py               # Click CLI interface
│   ├── dashboard.py         # Rich TUI dashboard
│   ├── config.py            # Configuration management
│   ├── firefox.py           # Firefox profile manager
│   ├── i2pd.py              # I2Pd daemon controller
│   ├── utils.py             # Shared utilities (optional)
│   │
│   ├── commands/            # CLI command implementations
│   │   ├── __init__.py
│   │   ├── cmd_init.py
│   │   ├── cmd_start.py
│   │   ├── cmd_stop.py
│   │   ├── cmd_status.py
│   │   ├── cmd_restart.py
│   │   ├── cmd_browser.py
│   │   ├── cmd_config.py
│   │   ├── cmd_logs.py
│   │   └── cmd_reset.py
│   │
│   └── data/                # Static data files
│       └── user.js          # Arkenfox hardening template
│
├── tests/                   # Test suite
│   ├── __init__.py
│   ├── test_config.py
│   ├── test_firefox.py
│   ├── test_i2pd.py
│   └── test_cli.py
│
├── docs/                    # Documentation
│   ├── installation.md
│   ├── usage.md
│   └── development.md      # This file
│
├── pyproject.toml          # Package configuration
├── README.md               # User documentation
├── CONTRIBUTING.md         # Contribution guidelines
├── LICENSE                 # GPL-3.0
└── .gitignore             # Git ignore rules
```

### Module Responsibilities

**Core Modules:**
- `cli.py` - Click command definitions, manager initialization
- `dashboard.py` - Interactive TUI with Rich
- `config.py` - JSON config loading/saving/validation
- `firefox.py` - Profile creation, hardening, proxy config
- `i2pd.py` - I2Pd start/stop/status, cross-platform
- `utils.py` - Shared helper functions

**Command Modules:**
- Each `cmd_*.py` is self-contained
- Receives managers dict: `{'config', 'firefox', 'i2pd'}`
- Contains single `run()` function
- Handles UI output with Rich

---

## Development Workflow

### 1. Create Feature Branch

```bash
git checkout -b feat/your-feature-name
```

### 2. Make Changes

Edit files, add features, fix bugs.

### 3. Test Changes

```bash
# Run tests
pytest

# Run specific test
pytest tests/test_config.py

# Run with coverage
pytest --cov=i2p_manager
```

### 4. Format Code

```bash
# Format with black
black i2p_manager/

# Lint with ruff
ruff check i2p_manager/

# Fix auto-fixable issues
ruff check --fix i2p_manager/
```

### 5. Commit Changes

```bash
git add .
git commit -m "feat(cli): add bandwidth monitoring command"
```

Follow [Conventional Commits](https://www.conventionalcommits.org/):
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `refactor:` - Code refactoring
- `test:` - Adding tests

### 6. Push and Create PR

```bash
git push origin feat/your-feature-name
```

Then create Pull Request on GitHub.

---

## Testing

### Running Tests

```bash
# All tests
pytest

# Specific file
pytest tests/test_config.py

# Specific test
pytest tests/test_config.py::test_default_config

# With coverage
pytest --cov=i2p_manager --cov-report=html

# Verbose
pytest -v

# Stop on first failure
pytest -x
```

### Writing Tests

Create test files in `tests/` with `test_` prefix:

```python
# tests/test_config.py
import pytest
from i2p_manager.config import ConfigManager


def test_default_config():
    """Test default configuration loads correctly"""
    config = ConfigManager()
    assert config.get('i2pd.http_port') == 4444


def test_config_get_nested():
    """Test nested config access"""
    config = ConfigManager()
    port = config.get('i2pd.http_port')
    assert isinstance(port, int)


@pytest.fixture
def temp_config(tmp_path):
    """Fixture for temporary config"""
    config_dir = tmp_path / "config"
    config_dir.mkdir()
    return config_dir
```

### Test Coverage

Aim for 80%+ coverage:

```bash
pytest --cov=i2p_manager --cov-report=term-missing
```

---

## Code Style

### Python Style Guide

Follow **PEP 8** with these modifications:
- Line length: 100 characters
- Use `black` for formatting
- Use `ruff` for linting

### Formatting

```bash
# Format all files
black i2p_manager/ tests/

# Check formatting (don't modify)
black --check i2p_manager/
```

### Linting

```bash
# Lint all files
ruff check i2p_manager/

# Fix auto-fixable issues
ruff check --fix i2p_manager/
```

### Import Order

```python
# Standard library
import os
import sys
from pathlib import Path

# Third-party
import click
from rich.console import Console

# Local
from i2p_manager.config import ConfigManager
from i2p_manager.firefox import FirefoxManager
```

### Docstrings

```python
def create_profile(self, profile_name: str) -> Dict[str, str]:
    """
    Create new Firefox profile.
    
    Args:
        profile_name: Name for the new profile
        
    Returns:
        Dict with 'name' and 'path' keys
        
    Raises:
        FileNotFoundError: If Firefox is not installed
    """
    pass
```

### Type Hints

```python
from typing import Dict, Optional, List

def get_status(self, console_port: int = 7070) -> Dict[str, Any]:
    """Get I2Pd status information"""
    pass
```

---

## Adding Features

### Adding a New Command

1. **Create command file:**

```python
# i2p_manager/commands/cmd_newcommand.py
"""New command implementation"""

from rich.console import Console

console = Console()


def run(managers, **kwargs):
    """Run new command"""
    config = managers['config']
    firefox = managers['firefox']
    i2pd = managers['i2pd']
    
    console.print("[cyan]New command running![/cyan]")
    # Implementation here
```

2. **Register in commands/__init__.py:**

```python
from . import cmd_newcommand

__all__ = [..., 'cmd_newcommand']
```

3. **Add to CLI:**

```python
# i2p_manager/cli.py
from .commands import cmd_newcommand

@main.command("newcommand")
@click.option('--example', help='Example option')
def new_command(example):
    """Description of new command"""
    try:
        managers = get_managers()
        cmd_newcommand.run(managers, example=example)
    except Exception as e:
        console.print(f"[red]✗ Error:[/red] {e}")
        sys.exit(1)
```

4. **Test it:**

```bash
i2p-manager newcommand --example test
```

### Adding Configuration Option

1. **Update default config:**

```python
# i2p_manager/config.py
DEFAULT_CONFIG = {
    # ... existing config ...
    "new_section": {
        "new_option": "default_value"
    }
}
```

2. **Use in code:**

```python
config = managers['config']
value = config.get('new_section.new_option')
```

3. **Document in README**

---

## Debugging

### Print Debugging

```python
from rich.console import Console

console = Console()

# Debug output
console.print(f"[dim]Debug: value={value}[/dim]")
```

### Python Debugger

```python
# Add breakpoint
import pdb; pdb.set_trace()

# Or use breakpoint() (Python 3.7+)
breakpoint()
```

### Running with Debug Output

```bash
# Set environment variable
export DEBUG=1
i2p-manager start

# Or inline
DEBUG=1 i2p-manager start
```

### Common Issues

**Import errors:**
```bash
# Reinstall in editable mode
pip install -e .
```

**Module not found:**
```bash
# Check you're in venv
which python

# Activate if needed
source venv/bin/activate
```

**Tests failing:**
```bash
# Update dependencies
pip install -e ".[dev]"

# Clear pytest cache
pytest --cache-clear
```

---

## Release Process

### 1. Update Version

Update in two places:

**pyproject.toml:**
```toml
[project]
version = "0.2.0"
```

**i2p_manager/__init__.py:**
```python
__version__ = "0.2.0"
```

### 2. Update Changelog

Create or update `CHANGELOG.md`:

```markdown
## [0.2.0] - 2024-01-15

### Added
- New bandwidth monitoring feature
- Dashboard refresh rate configuration

### Fixed
- Firefox launch error on Windows
- Config validation issue

### Changed
- Improved status output formatting
```

### 3. Commit Changes

```bash
git add .
git commit -m "chore: bump version to 0.2.0"
git push
```

### 4. Create Git Tag

```bash
git tag -a v0.2.0 -m "Release v0.2.0"
git push origin v0.2.0
```

### 5. Build Package

```bash
# Clean old builds
rm -rf dist/ build/ *.egg-info

# Build
python -m build

# Verify
twine check dist/*
```

### 6. Publish to PyPI

```bash
# Test PyPI first
twine upload --repository testpypi dist/*

# Then real PyPI
twine upload dist/*
```

### 7. Create GitHub Release

1. Go to GitHub Releases
2. Click "Create Release"
3. Select tag `v0.2.0`
4. Add release notes
5. Attach dist files
6. Publish

---

## Development Tips

### Virtual Environment

Always use venv for development:

```bash
# Create
python -m venv venv

# Activate (always do this before coding)
source venv/bin/activate

# Verify
which python  # Should show venv path
```

### Pre-commit Checks

Before committing:

```bash
# Format
black i2p_manager/

# Lint
ruff check i2p_manager/

# Test
pytest

# All at once
black i2p_manager/ && ruff check i2p_manager/ && pytest
```

### IDE Setup

**VS Code:**
```json
{
    "python.linting.enabled": true,
    "python.linting.ruffEnabled": true,
    "python.formatting.provider": "black",
    "editor.formatOnSave": true
}
```

**PyCharm:**
- Enable Black in Tools → Black
- Enable Ruff in Preferences → Tools → External Tools

---

## Getting Help

### Documentation
- [Installation Guide](installation.md)
- [Usage Guide](usage.md)
- [Contributing Guide](../CONTRIBUTING.md)

### Community
- **GitHub Discussions:** Questions and ideas
- **GitHub Issues:** Bug reports
- **I2P Forum:** http://i2pforum.i2p

### Useful Resources
- [Click Documentation](https://click.palletsprojects.com/)
- [Rich Documentation](https://rich.readthedocs.io/)
- [pytest Documentation](https://docs.pytest.org/)
- [Python Packaging Guide](https://packaging.python.org/)

---

**Happy developing!**