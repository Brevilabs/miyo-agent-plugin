# Miyo Agent Plugin

Give AI agents access to the notes, documents, and saved AI chats connected to
[Miyo](https://www.miyo.md/docs). This repository is a portable
[Agent Plugin](https://agent-plugins.org), backed by Miyo's hosted remote MCP
service.

## Install

### Cursor and Grok Bot

Once Miyo is listed in the Cursor Marketplace, install **Miyo** from the
marketplace. Grok Bot uses the Cursor Marketplace catalog.

Until then, add this remote MCP server in the client's custom MCP settings:

```text
https://relay.miyo.md/mcp
```

### Other Agent Plugins and MCP hosts

Hosts that support Agent Plugins can load this repository as a plugin. Hosts
that support remote MCP directly can use the same endpoint without installing
the package:

```text
https://relay.miyo.md/mcp
```

Follow your host's instructions for loading a Git repository or adding a
Streamable HTTP MCP server.

## Authentication and availability

The MCP client should open Miyo's OAuth sign-in flow when it first connects. No
API key belongs in this repository or in `mcp.json`.

To use the connector:

1. Install and sign in to [Miyo](https://www.miyo.md/docs/get-started).
2. Connect the folders and chat sources you want Miyo to make available.
3. Turn on Miyo Connect and keep the Miyo desktop app online.
4. Add the plugin or MCP URL to your agent and complete OAuth in the browser.

The connector exposes these tools:

- `list_folders`
- `list_files`
- `search`
- `read_file`
- `create_file`
- `edit_file`

Only folders made available to remote AI can be read. Creating or editing a
file also requires the folder's `allow_writes` setting to be enabled.

## Package boundary

This repository is a thin connector package. The MCP endpoint and OAuth service
run on Miyo's servers; their source code is not included here. The package
contains no relay, authentication, billing, or indexing implementation, local
server launcher, credentials, or API keys.

The same hosted endpoint serves every compatible client. A host that needs a
different manifest in the future can add a thin adapter while continuing to
use `https://relay.miyo.md/mcp`.

## Contents

```text
.
├── .cursor-plugin/
│   └── plugin.json
├── LICENSE
├── README.md
├── assets/
│   └── logo.png
├── plugin.json
├── mcp.json
└── skills/
    └── miyo-notes/
        └── SKILL.md
```

The root `plugin.json` and `mcp.json` target Agent Plugins 1.0. The
`.cursor-plugin/plugin.json` manifest adds Cursor marketplace metadata and
references the Miyo app logo without changing the portable package contents.
The bundled `miyo-notes` skill guides agents to choose the correct search
corpus and honor folder write permissions.

## Marketplace publication

Maintainers can submit this repository through
[Cursor Marketplace publishing](https://cursor.com/marketplace/publish).
Marketplace publication and updates require Cursor review.

## License

[MIT](LICENSE)
