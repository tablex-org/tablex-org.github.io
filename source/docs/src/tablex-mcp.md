# tablex-mcp

`tablex-mcp` is a small program that lets AI tools use your saved tablex connections. It speaks the Model Context Protocol (MCP) over standard input and output. Your MCP client starts it, for example Claude Code, Claude Desktop or Cursor.

For each connection, you decide how far an AI tool may go. By default, it may do nothing.

## Install it

1. Open **Driver Manager**. See [Drivers](drivers.md).
2. Click the **Tools** chip.
3. Find **tablex MCP server** and click **Install**.

tablex installs it at a path that does not change when you update it, so an MCP client keeps working after an update:

```text
~/Library/Application Support/tablex/tools/tablex-mcp/tablex-mcp
```

The row shows the full path. On macOS, tablex then starts the Keychain step by itself. See [Allow Keychain access](#allow-keychain-access).

<!-- screenshot: the tablex MCP server row in Driver Manager > Tools, with its buttons -->

## Connect your MCP client

The installed row has three buttons: **Copy client configuration**, **Copy Claude Code command**, and **Allow Keychain access**.

### Claude Code

1. Click **Copy Claude Code command** in Driver Manager.
2. Paste it into Terminal and run it. It looks like this, with the path of your Mac:

```sh
claude mcp add tablex -- '/Users/you/Library/Application Support/tablex/tools/tablex-mcp/tablex-mcp'
```

### Claude Desktop and Cursor

1. Click **Copy client configuration** in Driver Manager.
2. Paste it into the MCP server settings of your client. If the client already has an `mcpServers` section, add only the `tablex` entry to it. The clients' own documentation says where that file is.

The text is this, with the path of your Mac:

```json
{
  "mcpServers": {
    "tablex": {
      "command": "/Users/you/Library/Application Support/tablex/tools/tablex-mcp/tablex-mcp",
      "args": []
    }
  }
}
```

### Any other client

Most MCP clients take the same `mcpServers` block. You can also print it from Terminal:

```sh
'/Users/you/Library/Application Support/tablex/tools/tablex-mcp/tablex-mcp' --print-config
```

`tablex-mcp` takes no arguments when a client starts it. It also understands `--version`, `--help`, `--print-config`, and `--authorize-keychain`.

After you add the server, restart your MCP client.

## Agent access levels

Each connection has an agent access level. It applies to the MCP clients and to the [AI assistant](assistant.md). There are four levels:

| Level | What an AI tool can do |
| --- | --- |
| **Off** | Nothing. This is the default. |
| **Schema only** | See table, column and routine definitions. It never sees rows. |
| **Read** | Also run statements that only read data. |
| **Read and write** | Also propose statements that change data. You approve each one first. See [Approvals for changes](#approvals-for-changes). |

Even when a connection is **Off**, the client can still list the names of your connections.

### Where to set the level

- **One connection:** open **Edit Connection**, go to the **Overrides** tab, and find **Agent access**. Clear **Use default** and choose a level. Under it, tablex shows **Resulting level**, and where the level comes from.
- **All connections:** **Settings → AI Assistant → Default agent access**.
- **All connections of one type:** **Settings → Query & Connections**, then **CONNECTION TYPES**, then **Agent access** of the type.

### How the level is decided

1. The connection's own level wins.
2. Without one, the connection uses the higher of its type's level and the default for all connections. These two never grant more than **Read**.
3. A **Production** connection stays **Off** until you set a level on the connection itself.
4. A read-only type caps every connection at **Read**.

Only you can raise a level, in tablex. An AI tool cannot.

## Approvals for changes

An AI tool can change data only when all of these are true:

1. **Let MCP clients run changes** is on in **Settings → AI Assistant**. It is off by default.
2. The connection's level is **Read and write**.
3. The connection is not of the built-in **Production** type. A custom type is not treated as production.
4. The MCP client can ask you a question. tablex uses the MCP elicitation feature for this.
5. The statement is short enough to review: at most 200 lines, 8000 characters, and 400 characters in one line.

If these are true, tablex asks the client to show you the statement, with the connection and database. The statement runs only if you accept. This is one question for each statement. For a script of several statements, you accept or decline all the changes together. A transaction has a question before it is committed.

Keep in mind:

- The MCP client is your own program, and it decides how to show the question. A client set to answer by itself can run changes without you seeing them. Turn the switch on only for a client you trust.
- Statements flagged as dangerous are refused through MCP, whatever you answer.
- If you decline several times for one connection, or no usable answer comes back, changes to that connection through MCP stay off until `tablex-mcp` is restarted.

Adding, copying and removing connections through an AI tool also needs your approval each time. The question is **Make this change to your connections**.

Every call an AI tool makes is written to an audit log, `audit.jsonl`, in tablex's data folder.

### Show something in tablex

An AI tool can ask the running tablex app to open a table (`open_table`) or to show a query and run it (`execute_and_show`). tablex must be running for this. The app applies its own rules to the query.

## Allow Keychain access

tablex keeps saved passwords encrypted, with one master key in the macOS Keychain. See [Where secrets are stored](connections.md#where-secrets-are-stored). `tablex-mcp` is a separate program, so macOS asks once whether it may read that key.

While `tablex-mcp` runs for a client, it never lets macOS ask. A dialog would appear behind your client, and the AI tool would wait for it. So you answer the question once, in advance.

1. Open **Driver Manager → Tools**.
2. On the **tablex MCP server** row, click **Allow Keychain access**.
3. When macOS asks, choose **Always Allow**.
4. Restart your MCP client.

What the button does:

1. It moves the passwords saved by earlier versions of tablex into tablex's own store. macOS asks once for each of those.
2. It runs `tablex-mcp --authorize-keychain`, which reads the master key and the saved passwords of the connections that AI tools may use. macOS asks for the master key.

You can also run the second step in Terminal:

```sh
'/Users/you/Library/Application Support/tablex/tools/tablex-mcp/tablex-mcp' --authorize-keychain
```

It prints one line for each connection, and then **Keychain access is allowed for** the number of connections. If it says **access was not kept for background use**, run it again and choose **Always Allow**.

Only connections with a level above **Off** are read. If none has one, the command says there is nothing to allow. So give at least one connection a level above **Off** first. See [Agent access levels](#agent-access-levels).

### After an update

tablex runs this step by itself after you install or update `tablex-mcp`. macOS may ask again after an update. Choose **Always Allow**, and restart your MCP client. If macOS asks later, click **Allow Keychain access** again.

## Update it

Update `tablex-mcp` in **Driver Manager → Tools**, like a driver. Then restart your MCP client.

## Troubleshooting

The messages below come from `tablex-mcp`. Your client shows them to you or to the AI tool.

| Message | What to do |
| --- | --- |
| `tablex cannot read the saved password without asking macOS (or the login keychain is locked)` | Click **Allow Keychain access** (above), then restart the client. If your login keychain is locked, unlock it first. |
| `agent access for connection ... is ...; ... needs ... or more` | Raise the connection's level. The message names the level it has: `off`, `schema_only`, `read` or `read_write`. See [Where to set the level](#where-to-set-the-level). |
| `... is a write ... and the agent access is ...; writes need read_write` | Set **Read and write** on the connection. Then check the conditions in [Approvals for changes](#approvals-for-changes). |
| `changes through the MCP server are turned off in tablex: Settings → AI` | Turn on **Let MCP clients run changes**. |
| `a production connection can only be changed from the tablex app` | Make the change in tablex. |
| `this MCP client cannot ask the user` | Your client does not support questions. Make the change in tablex. |
| `too long to be shown for approval through MCP` | Run the statement in tablex yourself, or split it. |
| `The ... driver needed by ... is not installed` | Install the driver in **Driver Manager**. See [Drivers](drivers.md). |
| `the audit log cannot be written, so writes are refused until it can; reads still work` | `tablex-mcp` cannot write `audit.jsonl` in the data folder. Check that the folder is writable. |
| An error about reaching tablex when an AI tool opens a table | Start the tablex app. `open_table` and `execute_and_show` need it. |

If the client cannot start the server at all:

1. Run `tablex-mcp --version` with the full path, in Terminal.
2. Check that the path in your client's configuration is the one on the **tablex MCP server** row. Copy the configuration again if it is not.
3. Look at the client's log. `tablex-mcp` writes its startup problems to standard error.
