# UseMyContext

Your personal context for every AI. Keep one profile of who you are and what you are working on, plus
the files that matter, at [usemycontext.ai](https://usemycontext.ai), and the AI you use reads it over
MCP. **You approve every update:** an AI can only suggest a change, and nothing lands until you say yes.

## One-click install

- **ChatGPT:** [Install in ChatGPT](https://chatgpt.com/plugins/plugin_asdk_app_6a62cbc0ff488191890ba6370456e73e), then sign in with your email and the code we send.
- **Claude:** [Open in the Claude Directory](https://claude.ai/directory/connectors/usemycontext) and click Connect.

## Coding tools

- **[Claude Code](https://github.com/anthropics/claude-code):** `/plugin marketplace add usemycontext/claude-code-plugin`, then `/plugin install usemycontext@usemycontext` ([plugin](https://github.com/usemycontext/claude-code-plugin))
- **[Cursor](https://cursor.com):** add the server below to `~/.cursor/mcp.json` ([plugin](https://github.com/usemycontext/cursor-plugin))
- **[OpenCode](https://github.com/anomalyco/opencode):** a `remote` server in `opencode.json` ([plugin](https://github.com/usemycontext/opencode-plugin))
- **[Gemini CLI](https://github.com/google-gemini/gemini-cli):** `gemini extensions install https://github.com/usemycontext/gemini-cli-extension`

Any other MCP client: add `https://mcp.usemycontext.ai/mcp` (Streamable HTTP). Sign-in is OAuth in
the browser, no API key to copy. Every client step is in the [docs](https://usemycontext.ai/docs/connect).

## What you get

- **Projects with their own @handle.** Each project has its own profile, facts and files, and a
  permanent @handle any AI can use to address it, private or public.
- **Share My Context.** Share one project's curated profile with someone else, and revoke it any time.
- **You stay in control.** AI access leaves a record in your activity feed, and you can revoke any AI
  from one page; revoking is enforced on the server.

## Plans

| Plan | Projects | Storage per project |
|---|---|---|
| Free | 2 | 20MB |
| Premium, $20/month | 10 | 100MB |

**Try it first, no signup:** sign in at [usemycontext.ai](https://usemycontext.ai) as
`demo@usemycontext.ai` with code `424242`, a shared, read-only demo account.

[Docs](https://usemycontext.ai/docs) | [SDK on npm](https://www.npmjs.com/package/usemycontext) | [What is a context layer?](https://usemycontext.ai/blog/what-is-a-context-layer)
