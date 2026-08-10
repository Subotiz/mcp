# Subotiz MCP — Installation Guide for AI Agents

This file is written for an AI coding agent (Cursor, Claude, Cline, VS Code Copilot, etc.).
Follow the steps below to install and configure the **Subotiz MCP** server on behalf of the user.

Subotiz MCP is a **remote (Streamable HTTP)** MCP server. There is **nothing to clone, build, or run locally** — you only need to add a server entry to the host's MCP configuration that contains two values:

1. The **MCP URL** (depends on the target environment).
2. An **Authorization header** carrying the user's Subotiz API Key.

---

## Step 1 — Collect the two required values

You MUST obtain these before writing any configuration. Do not invent or guess them.

### 1a. MCP URL

Ask the user which environment to connect to, then use the matching URL:

| Environment | MCP URL |
|-------------|---------|
| Production (default) | `https://api.subotiz.com/mcp` |
| Sandbox / testing    | `https://api.sandbox.subotiz.com/mcp` |

If the user does not specify, default to **Production** (`https://api.subotiz.com/mcp`) and tell them which one you chose.

### 1b. Subotiz API Key (token)

The `Authorization` header value is `Bearer <API_KEY>`, where `<API_KEY>` is the user's Subotiz API Key.

- If the user has already provided a key, use it as-is.
- If they have not, **stop and ask** them for it. Do NOT fabricate a token or commit a placeholder as if it were real.
- Key creation and authentication details: https://docs.subotiz.com/en/api/authentication-1

> Security: Treat the API Key as a secret. Never print it in chat logs, never commit it to git, and prefer host-native secret/`${env:...}` mechanisms when the host supports them.

---

## Step 2 — Write the configuration for the user's host

Add the entry below to the host's MCP config. **Replace the two placeholders:**

- `{{MCP_URL}}` → the URL chosen in Step 1a
- `{{YOUR_TOKEN_HERE}}` → the API Key from Step 1b (keep the `Bearer ` prefix)

When connecting to the official hosted service you only need the URL and the `Authorization: Bearer` header — **no other headers are required.**

### Cursor — `~/.cursor/mcp.json` (or project `.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "subotiz": {
      "url": "{{MCP_URL}}",
      "headers": {
        "Authorization": "Bearer {{YOUR_TOKEN_HERE}}"
      }
    }
  }
}
```

### Claude Desktop — `claude_desktop_config.json`

> Important: Claude Desktop's config file validates **stdio (command-based) servers only**. It does **NOT** support a remote `url` + `headers` entry — pasting one will be silently ignored or may drop your whole `mcpServers` block on the next save. Use one of the two supported paths below.

Config file location:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

**Option A (recommended for config-file setup) — bridge via `mcp-remote`** (requires Node.js 18+):

```json
{
  "mcpServers": {
    "subotiz": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "{{MCP_URL}}",
        "--header",
        "Authorization: Bearer ${SUBOTIZ_TOKEN}"
      ],
      "env": {
        "SUBOTIZ_TOKEN": "{{YOUR_TOKEN_HERE}}"
      }
    }
  }
}
```

Here `mcp-remote` runs as a local stdio process and speaks Streamable HTTP to Subotiz. Keep the token in the `env` block (referenced as `${SUBOTIZ_TOKEN}`) rather than baking it into the URL. After editing, fully quit Claude Desktop (Cmd-Q / File → Quit) and relaunch — it reads the file only at startup.

**Option B (no config file, Pro/Team/Enterprise plans) — Custom Connector UI:** Settings → Connectors → **Add custom connector**, paste `{{MCP_URL}}`, and provide the token when prompted.

### Cline (VS Code extension) — `cline_mcp_settings.json`

Open it via the Cline panel → **MCP Servers → Configure MCP Servers**. Cline uses `mcpServers` and the type `streamableHttp` for remote servers:

```json
{
  "mcpServers": {
    "subotiz": {
      "type": "streamableHttp",
      "url": "{{MCP_URL}}",
      "headers": {
        "Authorization": "Bearer {{YOUR_TOKEN_HERE}}"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

> Critical: `type` MUST be the camelCase value `"streamableHttp"` — not `"streamable-http"`, and do not omit it. A wrong/missing value makes Cline fall back to legacy SSE and fail with a `405` error.
>
> `autoApprove` lists tool names that run without per-call confirmation; leave it empty (`[]`) for safe defaults, especially for write tools like `create_*`.

### VS Code (1.101+) — `.vscode/mcp.json` or user `settings.json` under `"servers"`

VS Code uses the key `servers` (not `mcpServers`) and supports `type`. Prefer prompting for the token as an input instead of hardcoding it:

```json
{
  "inputs": [
    {
      "id": "subotiz_token",
      "type": "promptString",
      "description": "Subotiz API Key",
      "password": true
    }
  ],
  "servers": {
    "subotiz": {
      "type": "http",
      "url": "{{MCP_URL}}",
      "headers": {
        "Authorization": "Bearer ${input:subotiz_token}"
      }
    }
  }
}
```

> If you are merging into an existing config file, do NOT overwrite the whole file. Insert the `subotiz` server entry into the existing `mcpServers` / `servers` object and keep all other entries intact.

---

## Step 3 — Adjusting an existing configuration

If a `subotiz` (or equivalent) entry already exists, adjust rather than duplicate:

- **Switch environment** → change only the `url` value between the Production and Sandbox URLs from Step 1a.
- **Rotate / update the token** → change only the part after `Bearer ` in the `Authorization` header. Keep the `Bearer ` prefix.
- **Wrong host key** → ensure the key is `mcpServers` for Cursor / Claude Desktop / Cline, and `servers` for VS Code.
- Do not change the server name unless the user asks; renaming may break references elsewhere.

---

## Step 4 — Verify the connection

After writing the config:

1. Tell the user to **restart / reload** the MCP server in their host (e.g. toggle it off and on in Cursor's MCP settings, or reload the VS Code window).
2. Confirm the server shows as **connected** and that tools are listed.
3. As a smoke test, invoke a read-only tool such as `list_products` or `list_customer`. A successful response confirms both the URL and token are correct.

### Troubleshooting

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| `401 Unauthorized` | Wrong/expired token, or missing `Bearer ` prefix | Re-check Step 1b; ensure header is exactly `Bearer <key>` |
| Connection fails / no tools | Wrong URL or unsupported host | Verify the URL from Step 1a; ensure host supports Streamable HTTP MCP |
| Sandbox data instead of prod (or vice-versa) | Wrong environment URL | Swap the `url` per Step 3 |
| Config ignored | Wrong config key for the host | `mcpServers` (Cursor/Claude) vs `servers` (VS Code) |

---

## Reference

- Available tools: see [`TOOLS.md`](TOOLS.md)
- Server metadata: see [`server.json`](server.json)
- Developer documentation: https://docs.subotiz.com/en/quick-start/overview
- Authentication guide: https://docs.subotiz.com/en/api/authentication-1
