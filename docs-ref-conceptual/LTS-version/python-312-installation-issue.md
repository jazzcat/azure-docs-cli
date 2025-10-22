---
title: Python 3.12+ Installation Issue - Azure CLI
description: Known issue and workaround for installing Azure CLI on systems with Python 3.12 or later
ms.service: azure-cli
ms.custom: devx-track-azurecli, linux-related-content
---

# Python 3.12+ Installation Issue

## Issue Description

**Type of issue:** Code doesn't work

**Related to:** MicrosoftDocs/azure-docs-cli issue #5533

The standard Linux installation script for Azure CLI fails on systems running Python 3.12 or later. This is because the installation script relies on the `distutils` module, which was deprecated in Python 3.10 and completely removed in Python 3.12.

## Affected Systems

- Linux systems with Python 3.12 or later
- Particularly affects users on:
  - Arch Linux
  - Other distributions that have upgraded to Python 3.12+

## Current Documentation Limitation

The current installation documentation states:

> The install script only works on Python 3.8.x, 3.9.x, or 3.10.x. This install script does not work on Python 3.11.x or later versions.

However, users with Python 3.12+ need an alternative installation method.

## Workaround

For systems with Python 3.12 or later, use the following manual installation method using a Python virtual environment:

```bash
# Create a virtual environment in your home directory
python3 -m venv ~/lib/azure-cli

# Activate the virtual environment
source ~/lib/azure-cli/bin/activate

# Upgrade pip to the latest version
pip install --upgrade pip

# Install Azure CLI
pip install azure-cli

# Create a symbolic link to make 'az' command available globally
ln -s ~/lib/azure-cli/bin/az ~/bin/az
```

**Note:** Make sure `~/bin` is in your `$PATH`. If it's not, add the following to your `~/.bashrc` or `~/.bash_profile`:

```bash
export PATH="$HOME/bin:$PATH"
```

Then reload your shell configuration:

```bash
source ~/.bashrc
```

## Verification

After installation, verify that Azure CLI is working correctly:

```bash
az --version
```

## Alternative Installation Methods

If the workaround above doesn't meet your needs, consider using one of these alternative installation methods:

1. **Use a package manager** (recommended)
   - For Debian/Ubuntu: Use `apt` package manager
   - For RHEL/CentOS/Fedora: Use `dnf` or `yum` package manager
   - For openSUSE: Use `zypper` package manager

2. **Use Docker**
   ```bash
   docker run -it mcr.microsoft.com/azure-cli
   ```

3. **Install in a container** or use Azure Cloud Shell

## Related Documentation

- [Install Azure CLI on Linux](install-azure-cli-linux.md)
- [Azure CLI Support Lifecycle](azure-cli-support-lifecycle.md)
- [Run Azure CLI in a Docker container](run-azure-cli-docker.md)

## Status

This is a known limitation of the current installation script. The Azure CLI team is aware of this issue and is working on updating the installation script to support Python 3.12+.

For the most up-to-date information, see:
- Original issue: [MicrosoftDocs/azure-docs-cli#5533](https://github.com/MicrosoftDocs/azure-docs-cli/issues/5533)
- Azure CLI GitHub: [https://github.com/Azure/azure-cli/issues](https://github.com/Azure/azure-cli/issues)

## Feedback

If you encounter issues with this workaround or have suggestions for improvement, please file an issue on the [Azure CLI GitHub repository](https://github.com/Azure/azure-cli/issues).
