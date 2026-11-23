# FAQ and troubleshooting

## macOS asks for my password or Keychain access

tablex keeps your saved passwords in its own encrypted database. A single master key opens it, and that key is one item in the macOS Keychain. See [Where secrets are stored](connections.md#where-secrets-are-stored).

So macOS asks about one item. Choose **Always Allow**, not just **Allow**. Every release of tablex is signed with the same certificate, so what you allowed stays allowed after an update.

You may see more questions in these cases:

- **Passwords saved by an earlier version.** Old versions kept one Keychain item for each password. tablex copies each of them into its own store the first time it needs them, and macOS asks once for each. Choose **Always Allow**.
- **The MCP server.** `tablex-mcp` is a separate program, so macOS asks once more for it. Use **Allow Keychain access** in **Driver Manager → Tools**. See [Allow Keychain access](tablex-mcp.md#allow-keychain-access).
- **After you update `tablex-mcp`.** macOS may ask again. Run **Allow Keychain access** again.

If tablex says the master key is missing, or does not match, it cannot read the saved passwords. Open the command palette (⌘K), run **Reset Saved Passwords…**, and enter your passwords again. This removes the saved passwords and the AI key. It keeps your connections, history and settings.

## "Connection closed", "connection reset" or "broken pipe"

These messages mean that the database connection ended while you were not using it. The computer slept, the network changed, or the server closed it.

tablex treats these as a lost connection:

- When you run a query that only reads, tablex reconnects and runs it again by itself, once.
- A statement that may change data is never sent twice. tablex shows the error and a banner with **Reconnect to the database**. Click it.
- After the Mac sleeps, tablex reopens each connection the next time you use it.

To notice dead connections sooner, tablex checks a connection that has been idle for 60 seconds. You can change this in **Settings → Query & Connections**, with **Connection keep-alive interval**. Use `0` to turn it off. A connection can have its own value in the **Overrides** tab of its dialog.

If **Reconnect** fails, the server is probably not reachable. Check the host, the VPN or the SSH tunnel, then try again.

## tablex will not open

- On the first launch, macOS asks you to confirm an app that is not notarized. See [Open it for the first time](install.md#open-it-for-the-first-time).
- tablex needs macOS 12 or later.
- If it still does not open, [report it](#report-a-bug).

## Updates

- **Where do I check?** Click the **Updates** button in the title bar, or choose **tablex → Check for Updates…**. See [Updates](install.md#updates).
- **I only see Download.** tablex can only replace itself in a folder it can write to. Move it to **Applications**. See [When you see Download instead of Install & Restart](install.md#when-you-see-download-instead-of-install--restart).
- **I skipped a version by mistake.** Open the **Updates** button and click **Check Now**. It offers the version again.
- **Drivers and tools do not update with the app.** Update them in **Driver Manager**. A dot on the **Driver Manager** button means something has an update. See [Update drivers](drivers.md#update-drivers).
- **A driver updated, but my open connection behaves as before.** Open connections keep the old driver version until you reconnect them. In **Driver Manager**, click the driver and then **Reconnect them**.

## Driver Manager cannot load the catalog

tablex needs the network to load the list of drivers. When it cannot, it shows the list it saved earlier, and an error. Check your connection and click **Check for updates**.

## Where tablex keeps its data

On macOS, tablex keeps its data in one folder:

```text
~/Library/Application Support/tablex
```

To open it, choose **Go → Go to Folder…** in Finder and paste the path. It holds:

| Item | What it is |
| --- | --- |
| `tablex.db` | Your saved connections, query history, diagrams, open tabs and assistant conversations, and your saved passwords and API keys in encrypted form. |
| `settings.json` | Your settings. **Settings → About** shows its path. |
| `known_hosts` | The SSH servers whose keys you chose to trust. |
| `audit.jsonl` | The log of what AI tools did through the assistant and `tablex-mcp`. |
| `Drivers` | The installed drivers. |
| `tools` | Installed tools: `tablex-mcp`, SQL Formatter and MongoDB Shell. |
| `updates` | Updates that are being downloaded. |

The master key for the saved passwords is not in this folder. It is in the macOS Keychain.

To keep a copy of your connections without passwords, use **Export Connections**. See [Export and import tablex connections](connections.md#export-and-import-tablex-connections).

## Report a bug

Use [GitHub issues](https://github.com/tablex-org/tablex/issues). The same place takes feature requests.

Please include:

- The tablex version. Choose **tablex → About tablex**, or look at **Settings → About**.
- Your macOS version, and whether your Mac is Apple silicon or Intel.
- The database type, and the version of its driver from **Driver Manager**.
- What you did, what you expected, and what happened.
- The relevant lines of the **Error Log**. Choose **View → Error Log** to open it.

The project and its issues are public. Remove passwords, host names, addresses and anything private from what you post.
