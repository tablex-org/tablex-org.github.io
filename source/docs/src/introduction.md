# Introduction

tablex is a database client for macOS. It is written in Rust and built for speed: results travel from the driver to the grid as Apache Arrow, and the grid is virtualized, so it only draws the rows in view.

These pages describe tablex 0.1.9.

## Databases

tablex connects to:

- PostgreSQL
- MySQL
- MariaDB
- SQLite
- MongoDB
- Redis
- ClickHouse

Each database has its own driver. Drivers are not part of the app download. You install the ones you need from **Driver Manager**, inside the app. See [Drivers](drivers.md).

## What you can do

- Keep your connections in a tree, with folders, search, SSH tunnels, and import from other database tools.
- Browse and edit data in a grid. Edits wait as staged changes until you apply them.
- Write SQL and MongoDB scripts in editors with completion, query history and saved scripts.
- Work in tabs and split panes, use the command palette, and draw table and database diagrams.
- Ask an [AI assistant](assistant.md) about your schema and data. It only sees what you allow for each connection, and it asks before it changes anything.
- Let other AI tools use your saved connections through [tablex-mcp](tablex-mcp.md).

## Requirements

- macOS 12 or later.
- A Mac with Apple silicon (M1 and later) or an Intel processor. There is a download for each.

tablex is free and open source under the AGPL-3.0 licence. There are no downloads for Linux or Windows yet; those builds are not proven.

## Links

- [Download the latest release](https://github.com/tablex-org/tablex/releases/latest)
- [Source code on GitHub](https://github.com/tablex-org/tablex)
- [Report a bug or ask for a feature](https://github.com/tablex-org/tablex/issues)

## Where to start

1. [Install tablex and open it for the first time.](install.md)
2. [Install the driver for your database.](drivers.md)
3. [Add a connection.](connections.md)
