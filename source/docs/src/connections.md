# Connections

## Add a connection

1. Install the driver for your database first. See [Drivers](drivers.md). If you skip this, tablex offers to install it when you need it.
2. Click **New Connection** in the title bar, or choose **File → New Connection**.
3. Choose the type of database. The picker groups the types (**Relational**, **Analytics**, **Lightweight**, **Docs/Cache/Search**). You can search, and switch between a grid and a list. Double-click a type, or select it and click **Next**.
4. Fill in the fields. They are listed below for each database.
5. Click **Test**. tablex tries to connect and says **Connected in** some milliseconds, or shows the error.
6. Click **Save**.

<!-- screenshot: the New Connection dialog, Connection tab, for PostgreSQL -->

To connect, right-click the connection in the sidebar and choose **Connect**. To change it later, choose **Edit Connection**.

### Fields that every network database has

These fields are on the **Connection** tab.

- **URL**: paste a connection URL here, then click the link button (**Parse the URL into the fields below**). tablex fills in the fields. See [Paste a URL](#paste-a-url).
- **Name**: the name in the sidebar.
- **Type**: the database type. Click it to go back to the picker.
- **Environment**: the connection type. See [Connection types](#connection-types).
- **Color**: one of **None**, **Green**, **Yellow**, **Orange**, **Red**, **Blue**, **Purple**, or a custom color as `#rrggbb`. The color marks the connection in the sidebar and on its tab group.
- **Host** and the port.
- **User** and **Password**.
- **Save password**: see [Where secrets are stored](#where-secrets-are-stored). If you clear it, tablex asks for the password when you connect, and keeps it in memory until you quit.
- **Database**.

### PostgreSQL, MySQL and MariaDB

- **Database** is optional. For PostgreSQL the default is `postgres`.
- **Show only the connection's database**: the sidebar lists only the database named above. Without a database name, every database is listed.
- **URL Params**: extra parameters for the URL, for example `application_name=tablex` for PostgreSQL.

These databases also have the **TLS/SSL** tab. See [SSL and TLS options](#ssl-and-tls-options).

### MongoDB

- **Database** is optional.
- **Auth DB**: the database to authenticate against, optional, often `admin`.
- **URL Params**: extra parameters for the URL.
- A `mongodb+srv://` URL, or a URL with several hosts, is used as entered. The form shows a note, and the host and port fields are hidden.

### Redis

- **Database** is the database index, and defaults to `0`.
- **Use TLS (rediss://)**: connect with TLS.

Redis has no **TLS/SSL** tab. This checkbox is its TLS setting.

### ClickHouse

- **Use HTTPS (TLS)**: connect over HTTPS. If the port is empty, or still one of the defaults, it changes between `8123` (plain) and `8443` (HTTPS) with the checkbox. A port you typed yourself is kept.
- **CA Cert**: an optional PEM file with the certificate authority to trust.
- **Database** is optional, and defaults to `default`.
- **Show only the connection's database**: as for PostgreSQL.
- **Settings**: ClickHouse settings, for example `max_execution_time=30&async_insert=1`.

ClickHouse has no **TLS/SSL** tab.

### SQLite

SQLite has a **File Path** field instead of host, port and password. Type the path, or click the folder button (**Choose a database file**). SQLite has no **TLS/SSL** or **Tunnel / Proxy** tab.

### Paste a URL

Paste the URL into **URL**, then click the link button. These schemes are accepted:

| Database | Schemes |
| --- | --- |
| PostgreSQL | `postgresql://`, `postgres://` |
| MySQL | `mysql://` |
| MariaDB | `mariadb://` |
| MongoDB | `mongodb://`, `mongodb+srv://` |
| Redis | `redis://`, `rediss://` |
| ClickHouse | `clickhouse://`, `http://`, `https://` |

If the scheme does not match the type you chose, tablex says so and shows the one it expects. For example: `postgresql://user:password@host:5432/database`.

### The Advanced tab

- **Timeout**: how many seconds to wait while connecting. Leave it empty to use the driver's default. It works for PostgreSQL and MongoDB.
- **Pin connection in sidebar**. See [Pin a connection](#pin-a-connection).

### The Overrides tab

Each connection can have its own values for some settings. Each setting has a **Use global** checkbox. Clear it to give this connection its own value. The settings are **Query timeout**, **Connect timeout**, **Connection keep-alive interval**, **Default page size**, **Confirm dangerous statements**, and **Agent access**. For **Agent access**, see [tablex-mcp](tablex-mcp.md#agent-access-levels).

## SSL and TLS options

The **TLS/SSL** tab is for PostgreSQL, MySQL, MariaDB and MongoDB. The fields depend on the database.

| Field | PostgreSQL | MySQL and MariaDB | MongoDB |
| --- | --- | --- | --- |
| **SSL Mode** | `Default (prefer)`, `disable`, `allow`, `prefer`, `require`, `verify-ca`, `verify-full` | `Default (PREFERRED)`, `DISABLED`, `allow`, `PREFERRED`, `REQUIRED`, `VERIFY_CA`, `VERIFY_IDENTITY` | `Default (off)`, `Off`, `On (verify certificate)`, `On (skip verification)` |
| **CA Certificate** | yes | yes | yes |
| **Client Cert** | yes | yes | yes (one PEM file with the certificate and its key) |
| **Client Key** | yes | yes | no |
| **Server Name** | no | yes | no |

Fields a database does not use show **Not used**. Under the mode, tablex says in one line what the mode does. In short:

- The default and `prefer` encrypt when the server offers TLS. Otherwise they connect in clear text.
- `require` needs TLS. It checks the certificate against the CA file only if you set one.
- `verify-ca` checks the certificate chain against the CA file, but not the host name.
- `verify-full` checks the chain and the host name.

Key files must be unencrypted PEM files. Key passphrases are not supported. For PostgreSQL and MySQL/MariaDB, the key file must not be readable by group or others (`chmod 600`).

For PostgreSQL, tablex checks the certificate against the **Host**. To reach another address, add `hostaddr` to **URL Params**.

If you use a URL that is used as entered (a `mongodb+srv://` URL), put the TLS options in the URL itself.

## SSH tunnel and proxy

The **Tunnel / Proxy** tab opens the connection through an SSH server (a bastion). The SSH server reaches the database for you. The database host and port are reached from the SSH server. Each database session of the connection gets its own tunnel.

### Set up a tunnel

1. Open the **Tunnel / Proxy** tab and select **Connect through an SSH tunnel**.
2. Fill in the server. You can choose a host from your `~/.ssh/config` in **SSH Config** (**Use a ~/.ssh/config host**). tablex fills in the fields and lists the keywords it does not apply. Or enter them yourself:
   - **SSH Host** and its port (the default is 22)
   - **SSH User**
3. Choose an **Auth Method**:
   - **Password**: enter the **Password**.
   - **Private key**: choose the **Private Key** file, and enter the **Passphrase** if it has one.
   - **SSH agent**: signs in with the keys of your running `ssh-agent`.
   - **Keyboard-interactive**: the server's questions (a one-time code, a second factor) appear in a dialog when you connect. All sessions of the connection share one tunnel, so you answer once.
4. Optional: select **Save password** (or **Save passphrase**) to keep it. If you do not save it, tablex asks when you connect and keeps it in memory only while the connection is open.
5. Click **Test tunnel**. tablex says **Tunnel works** and how long it took, or shows the error.

### More options

- **Jump hosts**: click **Add jump host** to pass through other SSH servers before the SSH host. They are used in the order shown, and you can add up to 8. Each has its own host, port, user and authentication.
- **Remote Host** and port: optional. Leave them empty to reach the database host and port of the connection from the SSH server.
- **Algorithms**: **Allow legacy algorithms (diffie-hellman-group14-sha1, ssh-rsa)** for old servers.

### Host keys

The first time tablex connects to an SSH server, it asks **Trust this host?**. If you trust it, tablex remembers its key in its own `known_hosts` file. It only reads your `~/.ssh/known_hosts`. If a server's key changes later, tablex stops the connection and says so.

### Proxy

Select **Connect through a SOCKS5 or HTTP proxy** to send the connection through a proxy first. Fill in **Proxy Type** (**SOCKS5** or **HTTP CONNECT**), **Proxy Host** and its port, and optionally **Proxy User** and **Proxy Password**. With an SSH tunnel, the proxy carries the connection to the first SSH server. Without one, it carries the connection to the database.

### What does not work through a tunnel or proxy

tablex refuses to save a connection that cannot use the route, and says why. A tunnel or a proxy does not work with:

- SQLite, which has no network address.
- A URL with several hosts.
- MongoDB with `mongodb+srv://`.
- MongoDB with TLS turned on.
- Redis with TLS (`rediss://`).
- ClickHouse with HTTPS.

## Connection types

A connection type gives its connections a color and safety settings. You choose it in the **Environment** field of the connection. A connection without a type follows the global settings only.

tablex has three types:

| Type | Color | What it does by default |
| --- | --- | --- |
| **Development** | green | Nothing extra. |
| **Test** | blue | Nothing extra. |
| **Production** | red | Asks before running SQL, and before applying data changes. |

The type shows as a small badge on the connection in the sidebar and on its tab group.

Under **Environment**, tablex says what the type does, for example *Production: asks before running SQL, asks before applying data changes*.

### Change a type or add your own

1. Open **Settings → Query & Connections**.
2. Find **CONNECTION TYPES**. Select a type, or click **Add type**.
3. Set the name (up to 32 characters) and the color (for example `#ef4444`).
4. Switch the settings you want:
   - **Confirm SQL execution**: every statement run from the SQL editor asks before it runs.
   - **Confirm data changes**: applying staged grid edits (or committing MongoDB changes) asks first and lists what will change.
   - **Read-only**: writes are refused (`INSERT`, `UPDATE`, `DELETE`, DDL, and grid edits). `SELECT` keeps working.
   - **Auto-commit by default**: SQL query tabs start in auto-commit. When it is off, they start in manual commit.
   - **Smart commit**, and **Return to auto-commit after Commit / Roll Back**.
   - **Auto-close idle transactions**: roll back a manual-commit transaction that has changes and stays idle (Development 30 minutes, Test 15 minutes, Production 10 minutes).
   - **Agent access**: how far AI tools may go on connections of this type. See [tablex-mcp](tablex-mcp.md#agent-access-levels).

Read-only is not on in any of the three types by default. Turn it on for a type to make all its connections read-only.

## Organize connections

### Groups

- Click the **New group** button in the sidebar to create a group.
- Right-click a group for **New Subgroup**, **Select group connections**, **Rename Group**, **Move to Group** and **Delete Group**. When you delete a group, its subgroups and connections move to the parent group.
- To move a connection, right-click it and choose **Move to New Group**, or drag it onto a group. You can also select several connections and use **Group selected…** or **Move selected to group**.

### Pin a connection

Right-click a connection, a database, a schema, a table, a view or a collection and choose **Pin**. It moves to the top of its group and gets a pin mark. It stays pinned after a restart. **Unpin** puts it back. For a connection you can also select **Pin connection in sidebar** on the **Advanced** tab.

### Hide a connection

1. Select the connection.
2. Click the sort and filter button at the top of the sidebar (tooltip **Sort connections / Filter by type**).
3. Under **Selected connection**, choose **Hide / Unhide**.

To hide several at once, select them and choose **Hide selected connections**. To see hidden connections again, choose **Show hidden connections** in the same menu.

### Duplicate a connection

Right-click a connection and choose **Duplicate Connection**. The copy has no passwords, and its SSH tunnel and proxy are switched off. Review its options before you connect.

## Import connections from other tools

tablex can import connections from DBeaver, DataGrip, Navicat and DBX.

1. In the sidebar, click **Import / Export**.
2. Choose the source: **Import DBX Config**, **Import Navicat NCX**, **Import DBeaver**, or **Import DataGrip**.
3. Choose what to import:

   | Source | Choose |
   | --- | --- |
   | DBX | A connections export (`.json`). If it is protected by a passphrase, tablex asks for it (**DBX export is encrypted**) and you click **Decrypt**. |
   | Navicat | A connections export (`.ncx`). |
   | DBeaver | The DBeaver workspace folder, or its `data-sources.json`. |
   | DataGrip | `dataSources.xml` in the project's `.idea` folder, or the project folder. |

4. tablex lists the connections it found. Clear the ones you do not want, or use **Select all** and **Select none**. Types tablex does not support (for example Oracle) are listed but cannot be selected. The list shows which connections come with an SSH tunnel.
5. Confirm. tablex saves the selected connections, and says how many it imported, how many it skipped, and how many were already saved.

Nothing is saved before you confirm. If you cancel at any step, nothing changes.

Passwords are imported where the file holds them. DataGrip keeps passwords in its own keychain entries, which tablex does not read. Folders and groups of the other tools are not imported.

## Export and import tablex connections

You can move connections between computers, or share them, without passwords.

### Export

1. Select the connections in the sidebar.
2. Click **Import / Export** and choose **Export Connections**.
3. Choose where to save the file.

For one connection, you can also use **Export Connection Config…** in the sort and filter menu, under **Selected connection**. **Copy Connection Config** puts the same content on the clipboard.

The file holds the name, type, address and color of each connection, and its network settings without any secret. Passwords are never exported.

### Import

1. Click **Import / Export** and choose **Import tablex file…**, and pick the file. Or copy a configuration and choose **Paste Connection Config** in the sort and filter menu.
2. tablex shows the connections in **Import Connection Configurations** and says that credentials are omitted and that SSH and proxy are disabled until you configure them again.
3. Click **Import**.
4. Enter the passwords again before you connect.

## Where secrets are stored

tablex keeps saved passwords in its own database, `tablex.db`, in its data folder. They are encrypted there with AES-256-GCM. The key for this is a random master key. tablex keeps the master key in the macOS Keychain, as a single item.

This covers database passwords, SSH passwords and key passphrases, proxy passwords, and AI API keys. A secret is never written to the settings file or to a log.

Because there is only one Keychain item, macOS asks about one item, not about each password. Choose **Always Allow** when it asks.

If the master key is lost, or does not match the database, tablex cannot read the saved passwords. Run **Reset Saved Passwords…** from the command palette. It removes every saved password and the AI key, and keeps your connections, history and settings. Then enter the passwords again.

For where the data folder is, see [FAQ and troubleshooting](faq.md#where-tablex-keeps-its-data).
