# Arch Setup Bootstrapper

## 🚀 Overview

This bootstrapper is the "entry point" for my system configuration. It automates the setup of a new machine by executing the following workflow:

1.  **Security Check:** Ensures the script is NOT run as root (required for `makepkg`).
2.  **System Update:** Syncs and upgrades all core packages.
3.  **AUR Helper:** Bootstraps `paru` from source if not present.
4.  **APKGS Integration:** Installs [Arch-Package-Sync](https://github.com/TomBos/Arch-Package-Sync) to manage package state.
5.  **Environment Cleanup:** Purges default Bash configuration files to make room for custom dotfiles.
6.  **Dotfile Deployment:** Clones personal configurations and symlinks them via `stow`.
7.  **Package Sync:** Runs a full sync for Pacman, Paru, and Flatpak.

## 📋 Prerequisites

* A fresh installation of **Arch Linux**.
* A user account with **sudo** privileges.
* An active internet connection.
* `curl` installed (`sudo pacman -S curl`).

> [!CAUTION]
> **Do not run this script as root.** The script will exit if run with `sudo` or as the root user, as AUR helpers and `makepkg` require an unprivileged user for security.

## 🛠️ Usage

The easiest way to run this is to pipe the raw script directly into bash. 

**Note:** Ensure you have your network configured before running.

```bash
curl https://raw.githubusercontent.com/TomBos/Arch-Setup-Bootstrapper/master/bootstrap.sh | bash
```

## 🔍 Logic Flow

### 1. Dependencies
The script ensures `base-devel`, `git`, and `curl` are installed. These are the bare essentials for any Arch user building from the AUR.

### 2. Paru (AUR Helper)
If `paru` isn't in your `$PATH`, the script:
* Clones `https://aur.archlinux.org/paru.git`.
* Runs `makepkg -si`.
* Cleans up the build directory immediately after.

### 3. Dotfiles & Stow
The script clones your `.dotfiles` repo to `$HOME/.dotfiles`. It uses **GNU Stow** with the `--no-folding` flag to ensure that your configuration files are symlinked into your home directory structure precisely as intended.

### 4. Triple-Sync (APKGS)
Using custom `apkgs` tool, the script triggers a synchronization for:
* **Native Packages** (Pacman)
* **AUR Packages** (Paru)
* **Flatpaks**


## 🔗 Related Repositories

* **[Arch-Package-Sync](https://github.com/TomBos/Arch-Package-Sync):** The tool that handles package state management.
* **[.dotfiles](https://github.com/TomBos/.dotfiles):** Personal configuration files.

*Maintained by [TomBos](https://github.com/TomBos)*
