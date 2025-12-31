# Contributing to I2P Easy Manager

First off, thank you for considering contributing to I2P Easy Manager! It's people like you that make this tool better for everyone.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Project Structure](#project-structure)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Pull Request Process](#pull-request-process)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Features](#suggesting-features)

---

## 📜 Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for all. Please be respectful and constructive in all interactions.

### Our Standards

**Positive behavior includes:**
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards others

**Unacceptable behavior includes:**
- Trolling, insulting comments, or personal attacks
- Public or private harassment
- Publishing others' private information without permission
- Any conduct which could reasonably be considered inappropriate

---

## 🤝 How Can I Contribute?

### Reporting Bugs

**Before submitting a bug report:**
1. Check the [existing issues](https://github.com/tundra-node/i2p-easy-manager/issues)
2. Try the latest version from the main branch
3. Collect information about your environment

**When submitting a bug report, include:**
- **Clear title** - Summarize the issue in one line
- **Description** - What happened vs. what you expected
- **Steps to reproduce** - Detailed steps to recreate the issue
- **Environment** - OS, Python version, I2Pd version, Firefox version
- **Logs** - Output from `i2p-manager logs` or error messages
- **Screenshots** - If applicable

**Example:**
```markdown
**Bug:** Dashboard crashes when I2Pd is not installed

**Environment:**
- OS: Ubuntu 22.04
- Python: 3.10.6
- I2Pd: Not installed
- Firefox: 119.0

**Steps to reproduce:**
1. Install i2p-manager without I2Pd
2. Run `i2p-manager`
3. Dashboard crashes with KeyError

**Expected:** Dashboard should show friendly error message
**Actual:** Python traceback shown

**Logs:**
[Paste error output here]
```

### Suggesting Features

**Before submitting a feature request:**
1. Check if it's already been suggested
2. Make sure it aligns with the project goals (lightweight, simple, privacy-focused)

**When suggesting a feature:**
- **Use case** - Why do you need this feature?
- **Proposed solution** - How should it work?
- **Alternatives** - What other solutions have you considered?
- **Additional context** - Screenshots, mockups, examples

### Improving Documentation

Documentation improvements are always welcome!

**Areas that need help:**
- Fixing typos or unclear explanations
- Adding examples or tutorials
- Translating to other languages
- Improving installation guides for different platforms

### Adding Code

See the [Development Setup](#development-setup) section below.

---

## 🛠️ Development Setup

### Prerequisites

- Python 3.8 or higher
- Git
- Firefox (for testing)
- I2Pd (for testing)

### Initial Setup

```bash
# Fork the repository on GitHub, then clone your fork
git clone https://github.com/tundra-node/i2p-easy-manager.git
cd i2p-easy-manager

# Create a virtual environment
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # macOS/Linux
# OR
venv\Scripts\activate     # Windows

# Install in editable mode with dev dependencies
pip install -e ".[dev]"

# Verify installation
i2p-manager --version
```

### Running from Source

```bash
# Run the installed command
i2p-manager

# Or run as a module
python -m i2p_manager

# Run specific commands
python -m i2p_manager status
```

### Development Tools

**Formatting:**
```bash
# Format code with black
black i2p_manager/

# Check formatting
black --check i2p_manager/
```

**Linting:**
```bash
# Lint with ruff
ruff check i2p_manager/

# Auto-fix issues
ruff check --fix i2p_manager/
```

**Type Checking:**
```bash
# Check types with mypy
mypy i2p_manager/
```

**Testing:**
```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=i2p_manager

# Run specific test file
pytest tests/test_config.py

# Run with verbose output
pytest -v
```

---

## 📁 Project Structure

```
i2p-easy-manager/
├── i2p_manager/              # Main package
│   ├── __init__.py          # Package exports and version
│   ├── __main__.py          # Entry point for python -m
│   ├── cli.py               # Click CLI interface
│   ├── dashboard.py         # Rich TUI dashboard
│   ├── config.py            # Configuration management
│   ├── firefox.py           # Firefox profile manager
│   ├── i2pd.py              # I2Pd daemon controller
│   ├── utils.py             # Shared utilities
│   │
│   ├── commands/            # CLI command implementations
│   │   ├── __init__.py
│   │   ├── cmd_start.py
│   │   ├── cmd_stop.py
│   │   ├── cmd_status.py
│   │   ├── cmd_restart.py
│   │   ├── cmd_browser.py
│   │   ├── cmd_config.py
│   │   ├── cmd_logs.py
│   │   ├── cmd_reset.py
│   │   └── cmd_init.py
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
│   └── development.md
│
├── pyproject.toml          # Package configuration
├── README.md               # User documentation
├── CONTRIBUTING.md         # This file
├── LICENSE                 # GPL-3.0
└── .gitignore             # Git ignore rules
```

### Module Responsibilities

**`cli.py`** - Command-line interface
- Defines Click commands
- Manages command routing
- Creates manager instances

**`dashboard.py`** - Interactive TUI
- Rich-based terminal UI
- Real-time status updates
- User input handling

**`config.py`** - Configuration
- Load/save JSON config
- Default settings
- Config validation

**`firefox.py`** - Firefox management
- Profile creation/deletion
- Proxy configuration
- Arkenfox hardening application

**`i2pd.py`** - I2Pd control
- Start/stop daemon
- Status checking
- Cross-platform process management

**`commands/cmd_*.py`** - Individual commands
- Self-contained command logic
- Receives manager instances
- Returns success/failure

---

## 🎨 Coding Standards

### Python Style Guide

We follow **PEP 8** with some modifications:
- Line length: 100 characters (not 79)
- Use `black` for automatic formatting
- Use `ruff` for linting

### Code Conventions

**Imports:**
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

**Docstrings:**
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

**Type Hints:**
```python
from typing import Dict, Optional, List

def get_config(key: str, default: Optional[str] = None) -> str:
    """Get configuration value"""
    pass
```

**Error Handling:**
```python
# Good - Specific exceptions
try:
    firefox.launch(profile_name)
except FileNotFoundError:
    console.print("[red]Firefox not found[/red]")
    sys.exit(1)

# Bad - Catch all exceptions
try:
    firefox.launch(profile_name)
except Exception:
    pass
```

**Console Output:**
```python
# Use rich for formatting
from rich.console import Console

console = Console()

# Colors: red, yellow, green, cyan, blue
console.print("[green]✓ Success[/green]")
console.print("[yellow]⚠ Warning[/yellow]")
console.print("[red]✗ Error[/red]")
console.print("[cyan]Info[/cyan]")
```

### Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
type(scope): subject

body (optional)

footer (optional)
```

**Types:**
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style changes (formatting, no logic change)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

**Examples:**
```
feat(dashboard): add peer count graph

fix(firefox): handle missing profile directory

docs(readme): add Windows installation guide

test(config): add tests for config validation
```

---

## 🧪 Testing

### Writing Tests

Place tests in the `tests/` directory with the naming convention `test_*.py`.

**Example test:**
```python
# tests/test_config.py
import pytest
from i2p_manager.config import ConfigManager


def test_default_config():
    """Test that default config loads correctly"""
    config = ConfigManager()
    assert config.get('i2pd.http_port') == 4444
    assert config.get('firefox.profile_name') == 'i2p-secure'


def test_config_get_nested():
    """Test nested config access"""
    config = ConfigManager()
    port = config.get('i2pd.http_port')
    assert isinstance(port, int)
    assert port > 0


def test_config_get_with_default():
    """Test default value for missing keys"""
    config = ConfigManager()
    value = config.get('nonexistent.key', 'default')
    assert value == 'default'
```

### Running Tests

```bash
# Run all tests
pytest

# Run specific file
pytest tests/test_config.py

# Run specific test
pytest tests/test_config.py::test_default_config

# Run with coverage
pytest --cov=i2p_manager --cov-report=html

# Run with verbose output
pytest -v

# Run and stop on first failure
pytest -x
```

### Test Coverage

Aim for at least 80% test coverage. Check coverage with:

```bash
pytest --cov=i2p_manager --cov-report=term-missing
```

---

## 🔄 Pull Request Process

### Before Submitting

1. **Create an issue first** - Discuss major changes before implementing
2. **Fork the repository** - Work in your own fork
3. **Create a branch** - Use descriptive branch names
4. **Write tests** - Add tests for new functionality
5. **Update docs** - Update README or other docs if needed
6. **Run tests** - Ensure all tests pass
7. **Format code** - Run `black` and `ruff`

### Branch Naming

```
feat/add-bandwidth-monitor
fix/firefox-launch-error
docs/improve-installation-guide
refactor/simplify-config-loading
```

### Submitting the PR

1. **Push to your fork:**
```bash
git push origin feat/your-feature
```

2. **Create pull request** on GitHub

3. **Fill out the PR template:**
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Refactoring

## Testing
How to test these changes

## Checklist
- [ ] Tests pass
- [ ] Code formatted with black
- [ ] Docs updated
- [ ] CHANGELOG updated (if applicable)
```

4. **Wait for review** - Maintainers will review your PR

5. **Address feedback** - Make requested changes

6. **Get merged** - Once approved, we'll merge it!

### PR Review Criteria

We'll check for:
- ✅ Code quality and readability
- ✅ Test coverage
- ✅ Documentation updates
- ✅ Adherence to coding standards
- ✅ No breaking changes (or properly documented)
- ✅ Commit message quality

---

## 🐛 Reporting Bugs

### Security Vulnerabilities

**Do NOT open a public issue for security vulnerabilities.**

Instead, email: security@example.com

We'll work with you to address the issue privately.

### Regular Bugs

Use the [bug report template](https://github.com/tundra-node/i2p-easy-manager/issues/new?template=bug_report.md) on GitHub.

Include:
- Clear description
- Steps to reproduce
- Expected vs. actual behavior
- Environment details
- Relevant logs

---

## 💡 Suggesting Features

Use the [feature request template](https://github.com/tundra-node/i2p-easy-manager/issues/new?template=feature_request.md).

**Good feature requests:**
- Solve a real problem
- Align with project goals (lightweight, simple, privacy-focused)
- Are well-explained with examples
- Consider implementation complexity

**Project goals:**
- Keep it lightweight (< 10MB)
- Keep it simple (non-technical users)
- Maximize privacy
- Cross-platform support
- Zero cost

---

## 📚 Additional Resources

### Learning Resources
- [Python Packaging Guide](https://packaging.python.org/)
- [Click Documentation](https://click.palletsprojects.com/)
- [Rich Documentation](https://rich.readthedocs.io/)
- [pytest Documentation](https://docs.pytest.org/)

### Project-Specific
- [I2P Documentation](https://geti2p.net/en/docs)
- [I2Pd Documentation](https://i2pd.readthedocs.io/)
- [Arkenfox user.js](https://github.com/arkenfox/user.js)

---

## 🎯 Good First Issues

Looking for something to work on? Check issues labeled:
- `good first issue` - Perfect for newcomers
- `help wanted` - We need help with these
- `documentation` - Improve docs

---

## 📬 Questions?

- **GitHub Discussions:** For general questions and ideas
- **GitHub Issues:** For bugs and feature requests
- **I2P Forum:** http://i2pforum.i2p (via I2P network)

---

## 🙏 Thank You!

Your contributions make this project better for everyone. Whether you're fixing a typo, adding a feature, or reporting a bug - every contribution matters.

**Happy coding!** 🚀