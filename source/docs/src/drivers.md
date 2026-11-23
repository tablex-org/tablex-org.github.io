# Drivers

## Why drivers are separate downloads

A driver is the part of tablex that talks to one kind of database. tablex does not include any. It downloads the ones you ask for, verifies them, and loads them when it runs.

This means:

- You install only the databases you use.
- A driver is updated on its own, without a new version of the app.
- Before a driver is loaded, tablex checks its signature. The install dialog shows where the driver comes from, its size, and who signed it.

You manage drivers in **Driver Manager**.

## Open Driver Manager

Do one of these:

- Click **Driver Manager** in the title bar.
- Choose **tablex → Driver Manager…** in the menu bar.
- Open the command palette (⌘K or ⌘P) and run **Driver Manager**.

Driver Manager opens in a tab.

<!-- screenshot: the Driver Manager tab with the filter chips and the driver list -->

## The driver list

At the top there is a search box (**Search drivers, schemes, capabilities…**), a **Check for updates** button, and a **⋯** menu. Below them are filter chips with counts: **All**, **Installed**, **Updates**, **SQL**, **NoSQL**, **Analytics**, **Key-Value** and **Tools**.

Each row shows the driver, its version, and a status:

- **Installed ✓**: the driver is installed.
- **Update**: a newer version is available.
- **Queued**: the install is waiting for its turn.
- **Disabled**: the driver is installed but turned off.

Click a row to see its details on the right (the window must be wide enough): the version that is active, its size, who signed it (**Signed by**), its **License**, its **Source**, and a link to the release notes.

## Install a driver

1. Open Driver Manager.
2. Find the driver. Use the search box or a filter chip.
3. Click **Install**.
4. Wait for the progress bar to finish. The steps are **Downloading**, **Verifying signature**, and **Loading driver**.

If the install fails, the button changes to **Retry**.

## Update drivers

tablex looks for driver updates when it starts, at most once a day. If it finds any, it shows one message, for example **1 update is available in Driver Manager**, and a dot appears on the **Driver Manager** button in the title bar. The dot also shows for tool updates.

To check by hand, click **Check for updates**.

To update:

- Click **Update** on a row, or
- Click **Update all** in the banner that lists the updates. While it runs, the banner shows the count (for example **Updating drivers 1/3**) and a **Cancel all** button.

An update applies to new connections. A connection that is already open keeps the old version until you reconnect it. The details of the driver say how many sessions are still on the old version, and **Reconnect them** reconnects them. A row can also show **Reconnect** as its button.

## Disable, remove and roll back a driver

Open the **⋯** menu of an installed driver. It has:

- **Disable** or **Enable**
- **Uninstall**
- **Reinstall**
- **Roll back to previous version**
- **Show in Finder**
- **Copy details**

If open connections use the driver, tablex lists them and asks first. It disconnects them before it disables or removes the driver. Your saved connections are not deleted.

## Tools

Tools are programs that run next to tablex. Click the **Tools** chip to see them. The chip shows how many there are, and how many have an update.

- **tablex MCP server** (`tablex-mcp`) lets other AI tools use your saved connections. See [tablex-mcp](tablex-mcp.md).
- **SQL Formatter (sqruff)** formats SQL in a separate process. After you install it, choose it in **Settings → Editor → Format SQL**. The built-in formatter stays the default.

Each tool has **Install**, **Update**, **Retry** and **Uninstall** buttons.

### MongoDB Shell

**MongoDB Shell** is an add-on of the MongoDB driver. It runs MongoDB shell scripts in a script tab. You find it in the driver list, in a row under the MongoDB driver, labelled **Add-on of the MongoDB driver**.

- It needs the MongoDB driver. Until the driver is installed (or recent enough), the button says **Install driver first** or **Update driver first**.
- If you run a shell script without the add-on, tablex asks **Install the MongoDB Shell plugin?** and runs the script after the install.
- To remove it, open its **⋯** menu and choose **Remove**.
- If you uninstall the MongoDB driver, tablex removes the add-on with it, and tells you first.

## When a driver is missing

If you connect to a database whose driver is not installed, tablex does not just fail. It opens a dialog.

1. The dialog is titled, for example, **PostgreSQL driver is not installed**. It says where tablex downloads the driver from, its size, and who signed it.
2. Click **Install and connect**. When the dialog was opened by a step of the New Connection dialog instead of a connection (for example **Next** or **Test**), the button says **Install**.
3. tablex shows the steps: **Downloading**, **Verifying signature**, **Loading driver**, and, when it was asked to connect, **Connecting**.
4. When the driver is loaded, tablex continues with what you were doing.

The dialog also has **Cancel**, which closes it without installing, and **Open Driver Manager**. If the install fails, or tablex cannot load the driver catalog (for example, you are offline), the dialog says so and the install button becomes **Retry**.

### The download badge in New Connection

In the **New Connection** dialog, a database whose driver is not installed has a download badge on its tile. Hover over it to read **The driver is not installed yet. Click to download it**. Click it to install the driver without leaving the dialog. The badge shows **Downloading the driver…** with a percentage. If it fails, it says **Download failed** and a reason, and you click it again to retry.

<!-- screenshot: the New Connection picker with a download badge on a tile -->

## For driver developers

The **⋯** menu next to **Check for updates** has **Import local driver…** and **Developer mode**. Developer mode accepts unsigned local manifests and ad-hoc signed drivers, so a banner reminds you while it is on. Leave it off unless you build drivers yourself.
