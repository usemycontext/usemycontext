# UseMyContext

UseMyContext is the personal context layer for AI. You keep one profile of who you are and what you
are working on, plus the files that matter, owned by you at [usemycontext.ai](https://usemycontext.ai),
and any MCP-capable AI client reads it. Sign in once and every AI you use already knows your context;
you never re-introduce yourself, and you can see and revoke every connection from one page.

**Try it in 30 seconds, no signup.** The demo account works two ways: sign in on the web at
[usemycontext.ai](https://usemycontext.ai) (the "Try the live demo" button), or connect any MCP client
to `https://mcp.usemycontext.ai/mcp` and use it at the OAuth sign-in. Login: `demo@usemycontext.ai`,
code `424242` (fixed - no mailbox needed). A shared, read-only account with a full profile, files, and a
shared context. Docs: [usemycontext.ai/docs](https://usemycontext.ai/docs)

## Connect any MCP client

The server is a standard remote MCP server:

```
https://mcp.usemycontext.ai/mcp
```

Streamable HTTP, OAuth 2.1. Any MCP-capable client can add it; sign-in is OAuth in the browser, no
API key to copy. You need a UseMyContext account first - the free tier at
[usemycontext.ai](https://usemycontext.ai) is enough.

### Claude Code

```
/plugin marketplace add usemycontext/claude-code-plugin
/plugin install usemycontext@usemycontext
```

Then run `/mcp`, select `usemycontext`, and complete the browser sign-in. The plugin adds a `/umc`
skill and per-folder profile mapping; details in
[claude-code-plugin](https://github.com/usemycontext/claude-code-plugin). Or skip the plugin and add
the raw server:

```
claude mcp add --transport http usemycontext https://mcp.usemycontext.ai/mcp
```

### Cursor

Add to `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project), then connect under Cursor
Settings, Tools and MCP Servers:

```json
{
  "mcpServers": {
    "usemycontext": {
      "url": "https://mcp.usemycontext.ai/mcp"
    }
  }
}
```

There is also a [cursor-plugin](https://github.com/usemycontext/cursor-plugin) with folder-to-profile
mapping.

### Codex CLI

```
codex mcp add usemycontext --url https://mcp.usemycontext.ai/mcp
codex mcp login usemycontext
```

Or in `~/.codex/config.toml`:

```toml
[mcp_servers.usemycontext]
url = "https://mcp.usemycontext.ai/mcp"
```

On older Codex versions, remote servers need `experimental_use_rmcp_client = true` under
`[features]`.

### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "usemycontext": {
      "serverUrl": "https://mcp.usemycontext.ai/mcp"
    }
  }
}
```

### Any other MCP client

Any client that speaks Streamable HTTP with OAuth can connect. The common JSON shape:

```json
{
  "mcpServers": {
    "usemycontext": {
      "type": "http",
      "url": "https://mcp.usemycontext.ai/mcp"
    }
  }
}
```

Some clients name the fields differently (`serverUrl`, `type: "streamable-http"`); check your
client's MCP docs for the exact key names.

Once connected, the client gets thirteen tools - `profile`, `list_profiles`, `info`, `account`, `list_files`, `search_files`,
`get_file`, `search`, `fetch` (thin aliases of `search_files`/`get_file` for ChatGPT deep-research compatibility),
`ask_docs`, `query_table`, `suggest_update`, `shared_context` - all read-only against your files.

## Session starter for file-driven tools: `npx usemycontext pull`

Not every AI tool speaks MCP. For the ones that read a local file at startup - Claude Code, agent
frameworks, your own scripts - one command writes your curated context to disk:

```bash
export UMC_TOKEN=<your token from the Connect page at usemycontext.ai>
npx -y usemycontext pull                # writes ./UMC-CONTEXT.md
npx -y usemycontext pull --out me.md    # or any path
```

The file holds your compiled profile only, never raw facts, files, or audit, wrapped in a short fixed
preamble telling the reading AI that this is your human-curated ground truth and must not be rewritten
or appended to, plus a date stamp and a pointer back to your live profile. Re-running overwrites it, so
it stays fresh, and a failure exits non-zero with one plain-English line rather than leaving a partial
file. Pulls appear as their own client, `umc-cli`, in your activity feed, separate from Claude or
ChatGPT.

Wire it into Claude Code as a SessionStart hook, in `.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [
      { "hooks": [{ "type": "command", "command": "npx -y usemycontext pull" }] }
    ]
  }
}
```

Then one line in your `CLAUDE.md` tells the session to read it:

```
Read UMC-CONTEXT.md at session start: it is my curated personal context.
```

**Handling the token.** It is your session token, and it is long-lived (a year), so a leak is not
self-limiting - treat it like a password. The CLI deliberately never accepts it as a command-line flag,
because flags land in your shell history, and it never writes the token anywhere itself. Put it in
`UMC_TOKEN` in your shell profile, or in a `token=<value>` line in `~/.usemycontext` that you
`chmod 600`. If it does leak, open the Connect page at [usemycontext.ai](https://usemycontext.ai) and use
"Disconnect everything": revoking is enforced on the server, so every token dies immediately, everywhere.

More recipes, including pointing an agent framework's bootstrap file at the pulled file, are in the
[`usemycontext` package README](https://www.npmjs.com/package/usemycontext).

## Your notes become context

If your notes live in Notion, Google Drive, or Infomaniak kDrive, UseMyContext Premium can copy in
the pages and files you pick, on your command. Your Notion notes become readable by every AI you use:
Claude answers from them in the terminal, Cursor cites them in your editor, ChatGPT reads the same
profile. Honest scope: this is Premium, it copies only what you explicitly pick, the connectors are
read-only against your Notion and Drive, and disconnecting deletes the copies from our storage.

## Trust

- Every read is scoped to a project you chose, and access leaves a record in your audit trail.
- One tool, `suggest_update`, can write: it files a pending suggestion that you review and approve
  in the web app. Nothing the AI does ever edits your profile or your files directly.
- Revoke any AI in one click, from `/mcp` in your client or the Connect page in the web app.
  Revoking is enforced on the server: the token dies immediately, everywhere.
- The free tier is enough to try all of this.

## Links

| | |
|---|---|
| The service | [usemycontext.ai](https://usemycontext.ai) |
| Docs | [usemycontext.ai/docs](https://usemycontext.ai/docs) |
| Claude Code plugin | [github.com/usemycontext/claude-code-plugin](https://github.com/usemycontext/claude-code-plugin) |
| Cursor plugin | [github.com/usemycontext/cursor-plugin](https://github.com/usemycontext/cursor-plugin) |
| SDK for app builders | [`usemycontext` on npm](https://www.npmjs.com/package/usemycontext) |
| What is a context layer? | [usemycontext.ai/blog/what-is-a-context-layer](https://usemycontext.ai/blog/what-is-a-context-layer) |
