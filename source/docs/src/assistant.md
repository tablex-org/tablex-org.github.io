# AI assistant

The assistant is a chat panel inside tablex. You ask about a database in plain words. It can write a query from a description, explain or fix one, and describe a table. To answer, it reads the schema, and it can run queries, as far as you allow for each connection.

The assistant needs an AI provider that you choose. You enter your own API key, or the address of a server of your own. tablex does not supply a model.

## Set it up

1. Open **Settings → AI Assistant**. In the menu bar, choose **tablex → Settings…**.
2. Choose a **Provider**:
   - **Anthropic**
   - **OpenAI**
   - **OpenAI-compatible**: any server that speaks the OpenAI API, for example one on your own machine
   - **Ollama**
3. Enter the API key and click **Save**. See [API keys](#api-keys).
4. Choose a **Model**. See [Models](#models).
5. For **OpenAI-compatible**, set the **Base URL** in the **Advanced** group. See [Provider addresses](#provider-addresses).

The key, the model and the address are kept separately for each provider. When you switch the provider, you see the values of that provider.

<!-- screenshot: Settings > AI Assistant with the provider, key and model -->

### API keys

- **Anthropic** and **OpenAI** need a key.
- **OpenAI-compatible** and **Ollama** work without a key. Add one only if your server asks for it.

Type or paste the key into the **API key** row and click **Save**. Saving does not wait for **Apply**. tablex never shows a saved key again. **Remove** deletes it.

The key is not written to the settings file. tablex keeps it in the same encrypted store as your passwords. See [Where secrets are stored](connections.md#where-secrets-are-stored).

### Models

- **Anthropic** has a list of models built in, and a default model.
- For the other providers, choose a model, or type its id.
- Click **Refresh models** to load the list that the provider offers now. It uses the saved key and the **Base URL** shown on the page, applied or not.
- **Reasoning effort** sets how much the model thinks before it answers: **Low**, **Medium**, **High**, **Extra high** or **Max**. A change applies from the next new chat.
- **Retry a declined request on a fallback model** is only for Anthropic.

### Provider addresses

The **Base URL** starts with these values:

| Provider | Base URL |
| --- | --- |
| Anthropic | `https://api.anthropic.com` |
| OpenAI | `https://api.openai.com/v1` |
| Ollama | `http://localhost:11434/v1` |
| OpenAI-compatible | none: you enter it, for example `http://localhost:1234/v1` |

Use `https://`. Plain `http://` is accepted only for a server on your own machine, such as Ollama or LM Studio. The address must not contain a user name or password, a query, or a fragment.

## Open the assistant

Do one of these:

- Click **Assistant** in the title bar.
- Choose **View → AI Assistant**.
- Press ⇧⌘A.

The chat is about the connection of the tab you have open. If you switch to a tab of another connection, the panel offers **New chat for** that connection.

To ask about a table, right-click it in the sidebar and choose **Ask the assistant about this table**. In an SQL editor, right-click and choose **Assistant: Explain**, **Assistant: Optimise**, or, after a statement failed, **Assistant: Fix**. The command palette has **Assistant: Explain Query**, **Assistant: Fix Query** and **Assistant: Optimise Query**.

The panel also has **New chat**, and **History** to reopen earlier conversations of the connection.

## What it can see and do

The assistant uses the agent access level of the connection. The levels are the same as for [tablex-mcp](tablex-mcp.md#agent-access-levels): **Off**, **Schema only**, **Read**, and **Read and write**. The default is **Off**.

The first time you ask about a connection that is **Off**, the panel asks whether to let the assistant use the connection (**Let the assistant use …?**). It lists what each level means, and which service and model your messages go to. Choose a level, or click **Not now**. You can change it later with the **Change** button in the panel, or in the connection's **Overrides** tab. See [tablex-mcp](tablex-mcp.md#where-to-set-the-level) for all the places.

| Level | What goes to the AI service |
| --- | --- |
| **Off** | Nothing. The assistant cannot use the connection. |
| **Schema only** | The names and types of tables and columns, and the definitions of functions, procedures and triggers. No rows. |
| **Read** | Also the rows returned by read-only queries that the assistant runs. It cannot change data. |
| **Read and write** | Also what the statements it proposes return. It may propose statements that change data or structure. See [Confirmations before changes](#confirmations-before-changes). |

The panel never sends passwords, connection addresses or keys.

The assistant can also read other connections that you have allowed for it, each at its own level. Its steps then say which connection they are on. It works with MongoDB too.

The connection type can limit the choices. A **Production** connection stays **Off** until you choose a level on the connection. If the type allows none, the panel says so.

If you lower the level while a chat has history, the chat ends, because it may hold more than the new level allows.

### Conversations

tablex keeps finished conversations on your computer, in `tablex.db`, so **History** can reopen them. They can contain query results. To stop saving them, turn off **Keep conversations on this computer** in **Settings → AI Assistant**. That does not delete what is saved. To delete, click **Delete all conversations** in the same page, or **Delete all for this connection…** in **History**.

## Confirmations before changes

With **Read and write**, the assistant can propose a statement that changes data or structure. It never runs it by itself.

1. A card appears in the chat. Its title is **Run this statement?**, **Run this script?**, or **Run this command?** (for MongoDB). A statement that is flagged as dangerous says **Run this dangerous statement?**
2. Read the statement. If it is long, the card says how many lines are below, and asks you to scroll to read them before you run it. If a statement has text that reads right to left, which can reorder quotes and comment marks, the card warns you.
3. Click **Run**, or **Run anyway** for a dangerous statement. Click **Don't run** to decline. Pressing Escape on the card also declines.

Some rules:

- Nothing runs without a click on that statement's own card. tablex does not remember answers, and there is no timeout that approves.
- A card ignores clicks on **Run** for a moment after it appears, so a double-click on one card cannot answer the next one.
- **Stop** cancels the run, and a statement that waits is not run.
- Statements that only read do not need a click.

Code blocks in an answer have **Copy** and **Insert** buttons. **Insert** puts the SQL in the editor. It is not run.

## If something does not work

- **Add your … API key in Settings → AI**: save a key for the provider that the message names.
- **Choose a model in Settings → AI**: pick a model, or type its id.
- **The assistant is off for this connection.**: click **Choose access**.
- A message about the address: check the **Base URL**. See [Provider addresses](#provider-addresses).
