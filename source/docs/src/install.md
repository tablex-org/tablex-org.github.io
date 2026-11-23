# Install and first launch

## Install tablex

1. Open the [latest release](https://github.com/tablex-org/tablex/releases/latest) on GitHub.
2. Download the `.dmg` for your Mac:
   - Apple silicon (M1 and later): `tablex-<version>-macos-apple-silicon.dmg`
   - Intel: `tablex-<version>-macos-intel.dmg`
3. Open the `.dmg` and drag **tablex** to **Applications**.

tablex needs macOS 12 or later.

<!-- screenshot: the disk image window with tablex and the Applications folder -->

## Open it for the first time

tablex is not notarized by Apple yet. macOS therefore asks you to confirm the first launch. Use one of these three ways.

### Right-click and Open

1. In **Applications**, right-click **tablex** and choose **Open**.
2. In the dialog, click **Open**.

### Open Anyway in System Settings

Newer versions of macOS do not show an **Open** button in that dialog. Use this instead:

1. Try to open tablex once. macOS says it cannot verify the app.
2. Open **System Settings** and go to **Privacy & Security**.
3. Scroll down to the message about tablex and click **Open Anyway**.

### Remove the quarantine flag in Terminal

Run this command once:

```sh
xattr -dr com.apple.quarantine /Applications/tablex.app
```

Then open tablex as usual.

You only need to do this for the first launch.

## After the first launch

tablex does not include database drivers. To connect to a database:

1. Install the driver for it in **Driver Manager**. See [Drivers](drivers.md).
2. Click **New Connection** in the title bar. See [Connections](connections.md).

## Updates

tablex can check for a new version and install it for you.

### How tablex checks

- When tablex starts, it asks GitHub whether a newer release exists. It does this at most once a day. Only the request itself is sent.
- To turn this off, clear **Check for updates automatically** in **Settings → Appearance** (the **Updates** group).
- To check right now, choose **tablex → Check for Updates…** in the menu bar, or click the **Updates** button in the title bar.

The **Updates** button opens the Update Center. It has two tabs: **tablex** (the status and the release notes of the new version) and **Changelog** (earlier versions). Its buttons are **Check Now** and **Open Release**. A dot on the **Updates** button means a newer version is known.

### Install an update

When a newer version exists, a notice says **tablex *version* is available**.

1. Click **Install & Restart**.
2. tablex shows the progress: **Downloading**, **Verifying**, **Installing**. You can click **Cancel** while it downloads.
3. When it says **tablex *version* is installed**, click **Restart**. tablex may ask first about open transactions, as it does when you quit.

The notice also has **Later**, and **Skip This Version**. A skipped version is not announced again by the automatic check. **Check Now** offers it again.

### What tablex checks before it installs

- The list of update files is signed. tablex verifies the signature with the same keys it uses for the driver catalog.
- The download must match the size and SHA-256 hash in that list.
- The unpacked app is checked before it replaces the old one.
- The old copy is kept next to the app for a short time and then removed.

### When you see Download instead of Install & Restart

tablex can only replace itself when the folder that holds it is writable. If it is not, the notice shows **Download**, which opens the release page, and a short reason:

- **Move tablex to Applications to update it.** You started tablex from the disk image, or macOS opened a temporary copy of it. Drag it to **Applications** and open it from there.
- **tablex can't update itself: *folder* is not writable.** Move tablex to a folder you can write to, or install the new version by hand from the release page.

### Drivers and tools

Drivers and tools update separately, in **Driver Manager**. See [Drivers](drivers.md).
