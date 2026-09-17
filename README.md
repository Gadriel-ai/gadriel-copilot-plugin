# Gadriel — AI Security Harness for GitHub Copilot

Security scanning for the code Copilot writes: SAST, secrets, dependencies
(SCA/SBOM), containers and configuration, including AI-specific risks like
prompt injection and the OWASP LLM Top 10 — 3,000+ rules, scanned on your
machine. Gadriel plugs into Copilot as an [MCP](https://modelcontextprotocol.io)
server plus repository instructions, prompts, and reviewer agents.

Part of the [Gadriel](https://gadriel.ai) AI Security Harness, alongside
[VS Code](https://marketplace.visualstudio.com/items?itemName=Gadriel.gadriel-security-harness),
[Claude Code](https://github.com/Gadriel-ai/gadriel-claude-plugin),
[Codex](https://github.com/Gadriel-ai/gadriel-codex-plugin), and
[Cursor](https://github.com/Gadriel-ai/gadriel-cursor-plugin).

## Get it

**Easiest — the VS Code extension.** Install
[**Gadriel AI Security Harness**](https://marketplace.visualstudio.com/items?itemName=Gadriel.gadriel-security-harness)
from the Marketplace. It registers the `gadriel` MCP server for Copilot
automatically and adds a **Gadriel: Scan Repository** command — no config to
edit.

**Or add the MCP server yourself.** The server is published to the
[GitHub MCP Registry](https://registry.modelcontextprotocol.io) as
`io.github.Gadriel-ai/gadriel`, so it shows up in VS Code's **MCP: Browse
Servers** and Copilot Chat's **`@mcp`** search — add it in a click. To wire it
per-repo instead, drop this `.vscode/mcp.json` into your project (needs Node for
`npx`, or `npm install -g gadriel`):

```json
{ "servers": { "gadriel": { "type": "stdio", "command": "npx", "args": ["-y", "gadriel@1.4.1", "code", "mcp"] } } }
```

**Add the Copilot guidance (optional).** Copy the `.github/` directory into your
repo so Copilot knows how to use Gadriel:

| Path | What |
|---|---|
| `.github/copilot-instructions.md` | repo-wide guidance, auto-applied |
| `.github/instructions/*.instructions.md` | 17 topic rules, scoped by `applyTo` |
| `.github/prompts/*.prompt.md` | `/gadriel-scan`, `/gadriel-fix`, `/gadriel-status`, … |
| `.github/agents/*.agent.md` | 8 reviewer agents |

## Use

In Copilot Chat (agent mode), just ask: **"Run a Gadriel security scan on this
repo and summarize the findings,"** or invoke a prompt like `/gadriel-scan`. The
`gadriel` MCP tools — `validate_file`, `findings_for_path`, `fix_finding`,
`validate_buffer`, and more — are available to Copilot directly.

## Good to know

- **No automatic edit guardrail here.** Copilot/VS Code has no edit-time hook
  (Cursor and Codex do), so scanning runs on request via the MCP tools and
  prompts rather than blocking each edit.
- **Enterprise:** Copilot Business/Enterprise can allowlist the `gadriel` MCP
  server through managed MCP policy.
- **Privacy:** code is scanned locally and never uploaded. First run registers
  an anonymous device credential with `app.gadriel.ai` (a random device id — no
  hostname, username, or keys); set `GADRIEL_NO_ANONYMOUS_AUTH=1` to skip. See
  the [privacy policy](https://gadriel.ai/privacy).

## Maintainers

The registry listing is (re)published by `.github/workflows/publish-mcp.yml`
(GitHub OIDC — an org namespace can only be published from CI in a Gadriel-ai
repo). After a new `gadriel` npm release, bump `server.json` and the
`.vscode/mcp.json` pin, then re-run that workflow.

## License

This repository is [Apache-2.0](LICENSE). The `gadriel` scanner it runs is
proprietary, under the [Gadriel terms](https://gadriel.ai/terms).
