# Connecting Obsidian as the storage backend

One-time setup, done outside this conversation. Once complete, the
second-brain skill auto-detects the MCP connection and switches backends —
no further configuration needed.

## 1. Install Claude Desktop

Download from claude.com/download. Requires a paid plan (Pro, $20/month) —
the free tier can't run Claude Code. Sign in, click the **Code** tab.

## 2. Install Obsidian and create a vault

Download from obsidian.md (free). Open it, click **Create new vault**, name
it (e.g. "brain"), pick a folder, click **Create**. That folder is now the
vault — every note lands here as a plain text file.

## 3. Enable the Local REST API plugin

In Obsidian: **Settings → Community plugins → Turn on community plugins →
Browse** → search "Local REST API" → **Install → Enable**.

Click into the plugin's settings and copy the **API Key** shown there.
Obsidian must stay open for the connection to work.

## 4. Connect Claude to the vault via MCP

In the Claude Code terminal, run (replacing the placeholder with the copied
key — omit the word "Bearer" if it's shown with the key):

```bash
claude mcp add-json obsidian-vault '{ "type": "stdio", "command": "uvx", "args": ["mcp-obsidian"], "env": { "OBSIDIAN_API_KEY": "PASTE-YOUR-KEY-HERE", "OBSIDIAN_HOST": "127.0.0.1", "OBSIDIAN_PORT": "27124" } }'
```

Test it: ask Claude "list every file in my Obsidian vault." If it reads the
vault back, the connection works. If not, confirm Obsidian is still open and
the plugin is enabled.

## Notes

- Read-only isn't enforced by this MCP tool — the second-brain skill only
  writes notes and reads existing ones, but access control ultimately lives
  in what the MCP server and Obsidian plugin permit, not in prompt
  instructions.
- Everything is plain text underneath. Nothing here locks the vault to
  Claude specifically — any tool that can read/write Obsidian's REST API or
  the raw files works too.
