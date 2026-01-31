---
name: terraform
description: This skill should be used when setting up a new development environment or provisioning a machine with essential development tools. It detects the operating system (macOS, Ubuntu, RHEL/CentOS/Fedora) and installs a standard set of CLI tools using the appropriate package manager. Triggers on "set up dev environment", "install dev tools", "provision machine", "terraform", or requests to bootstrap a new workstation.
---

# Development Environment Terraform

## Prerequisites

Ensure that `~/.zshrc_local` exists (empty is OK).

## Tool Installation

Detect the operating system (macOS, Ubuntu, RHEL/CentOS/Fedora) and install the following tools if not already present:

- uv
- just
- bat (batcat)
- ripgrep
- zsh
- entr
- nvm (node version manager) and then a modern node
- zoxide (manual installation from source)
- tmux
- neovim (use read https://github.com/neovim/neovim/blob/master/INSTALL.md and use a static download, not a package manager version. except on RHEL 8.9 we have to install from source because glibc is too old)
- direnv
- oh-my-posh
- vivid

## Package Manager Selection

- **macOS**: use `just brew` command (that will install everything in brew/Brewfile.common)
- **Ubuntu**: Use apt
- **RHEL/CentOS/Fedora**: Use dnf/yum

## Post-Install

Set zsh as the default shell if not already configured.

## Authentication

The user's password will likely be needed. Ask for it and use `sudo -S`.
