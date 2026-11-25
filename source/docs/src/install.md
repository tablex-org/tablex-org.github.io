# Install and first launch

## Install tablex

tablex runs on macOS, Linux and Windows. The Linux and Windows builds are a preview. Pick your system below. All downloads are on the [latest release](https://github.com/tablex-org/tablex/releases/latest) page on GitHub.

- [macOS](#macos)
- [Linux (preview)](#linux-preview)
- [Windows (preview)](#windows-preview)

## macOS

1. Open the [latest release](https://github.com/tablex-org/tablex/releases/latest) on GitHub.
2. Download the `.dmg` for your Mac:
   - Apple silicon (M1 and later): `tablex-<version>-macos-apple-silicon.dmg`
   - Intel: `tablex-<version>-macos-intel.dmg`
3. Open the `.dmg` and drag **tablex** to **Applications**.

tablex needs macOS 12 or later.

<!-- screenshot: the disk image window with tablex and the Applications folder -->

### Open it for the first time

tablex is not notarized by Apple yet. macOS therefore asks you to confirm the first launch. Use one of these three ways.

#### Right-click and Open

1. In **Applications**, right-click **tablex** and choose **Open**.
2. In the dialog, click **Open**.

#### Open Anyway in System Settings

Newer versions of macOS do not show an **Open** button in that dialog. Use this instead:

1. Try to open tablex once. macOS says it cannot verify the app.
2. Open **System Settings** and go to **Privacy & Security**.
3. Scroll down to the message about tablex and click **Open Anyway**.

#### Remove the quarantine flag in Terminal

Run this command once:

```sh
xattr -dr com.apple.quarantine /Applications/tablex.app
```

Then open tablex as usual.

You only need to do this for the first launch.

## Linux (preview)

1. Open the [latest release](https://github.com/tablex-org/tablex/releases/latest) on GitHub.
2. Download the file for your CPU, as an `.AppImage` or a `.tar.gz`:
   - x86_64: `tablex-<version>-linux-x86_64`
   - aarch64: `tablex-<version>-linux-aarch64`
3. Start it:
   - **AppImage:** run `chmod +x` on the file, then start it.
   - **Tarball:** unpack it. tablex runs from where you unpack it: `bin/tablex` is the app, and `bin/tablex-mcp` is the MCP server.

tablex needs:

- glibc 2.31 or newer (Debian 11, Ubuntu 20.04, RHEL 9 and later).
- A Wayland or X11 session.
- A Vulkan or OpenGL driver. If none is found, tablex says so and what it needs.

### Saved passwords on Linux

tablex keeps the key for your saved passwords in the system keyring (Secret Service: GNOME Keyring, KWallet or KeePassXC). On a desktop without a keyring, such as i3, Sway or a bare X session, tablex asks once what to do:

- keep the key in a private local file (less secure), or
- do not save passwords.

**Settings → Security** changes this later. See [Where secrets are stored](connections.md#where-secrets-are-stored).

## Windows (preview)

tablex needs Windows 10 or 11 on x86_64. There are two downloads on the [latest release](https://github.com/tablex-org/tablex/releases/latest) page.

### Installer

1. Download `tablex-<version>-windows-x86_64-setup.exe` and run it.
2. tablex installs for your user. It needs no administrator rights and nothing else to install.
3. It is added to the Start menu and to **Apps → Installed apps**.

A newer installer upgrades tablex in place. If you uninstall it, your connections and settings stay.

### Portable zip

1. Download `tablex-<version>-windows-x86_64.zip`.
2. Unzip it and run `tablex.exe`.

### SmartScreen

The Windows builds are not code signed yet. The first time you start tablex, SmartScreen asks for a confirmation. Click **More info**, then **Run anyway**.

Saved passwords are kept with the Windows Credential Manager. See [Where secrets are stored](connections.md#where-secrets-are-stored).

## After the first launch

tablex does not include database drivers. To connect to a database:

1. Install the driver for it in **Driver Manager**. See [Drivers](drivers.md).
2. Click **New Connection** in the title bar. See [Connections](connections.md).

## Updates

On macOS, tablex can check for a new version and install it for you. The Linux and Windows builds do not update themselves: tablex tells you about a new release and opens its page, and you install it as in [Install tablex](#install-tablex).

### How tablex checks

- When tablex starts, it asks GitHub whether a newer release exists. It does this at most once a day. Only the request itself is sent.
- To turn this off, clear **Check for updates automatically** in **Settings → Appearance** (the **Updates** group).
- To check right now, choose **tablex → Check for Updates…** in the macOS menu bar (on Linux and Windows, run **Check for Updates** from the command palette, ⌘K / Ctrl+K), or click the **Updates** button in the title bar.

The **Updates** button opens the Update Center. It has two tabs: **tablex** (the status and the release notes of the new version) and **Changelog** (earlier versions). Its buttons are **Check Now** and **Open Release**. A dot on the **Updates** button means a newer version is known.

### Install an update (macOS)

When a newer version exists, a notice says **tablex *version* is available**.

1. Click **Install & Restart**.
2. tablex shows the progress: **Downloading**, **Verifying**, **Installing**. You can click **Cancel** while it downloads.
3. When it says **tablex *version* is installed**, click **Restart**. tablex may ask first about open transactions, as it does when you quit.

The notice also has **Later**, and **Skip This Version**. A skipped version is not announced again by the automatic check. **Check Now** offers it again.

### What tablex checks before it installs (macOS)

- The list of update files is signed. tablex verifies the signature with the same keys it uses for the driver catalog.
- The download must match the size and SHA-256 hash in that list.
- The unpacked app is checked before it replaces the old one.
- The old copy is kept next to the app for a short time and then removed.

### When you see Download instead of Install & Restart

On Linux and Windows, tablex does not replace itself. It tells you about the new release and opens its page. On macOS, tablex can only replace itself when the folder that holds it is writable. If it is not, the notice shows **Download**, which opens the release page, and a short reason:

- **Move tablex to Applications to update it.** You started tablex from the disk image, or macOS opened a temporary copy of it. Drag it to **Applications** and open it from there.
- **tablex can't update itself: *folder* is not writable.** Move tablex to a folder you can write to, or install the new version by hand from the release page.

### Drivers and tools

Drivers and tools update separately, in **Driver Manager**. See [Drivers](drivers.md).
