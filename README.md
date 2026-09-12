# Gadriel — AI Security Harness for GitHub Copilot

The GitHub Copilot build of the [Gadriel](https://gadriel.ai) AI Security
Harness (siblings: [Claude Code](https://github.com/Gadriel-ai/gadriel-claude-plugin),
[Codex](https://github.com/Gadriel-ai/gadriel-codex-plugin),
[Cursor](https://github.com/Gadriel-ai/gadriel-cursor-plugin)).

Copilot integrates security tools through **MCP** — GitHub App "Copilot
Extensions" were sunset in Nov 2025, so this ships as an MCP server plus
Copilot's repository instructions, prompts, and agents. Gadriel covers SAST,
secrets, dependencies (SCA/SBOM), containers and configuration, including
AI-specific risks such as the OWASP LLM Top 10, with 3,000+ rules, scanned
locally.

## Install

**MCP server (per repo):** copy `.vscode/mcp.json` into your project. It runs
`npx gadriel code mcp` (needs Node; or `npm install -g gadriel`). VS Code /
Copilot picks up the `gadriel` server and its tools.

**Discovery (recommended):** the server is also published to the **GitHub MCP
Registry** (`server.json`), so it appears in VS Code's MCP gallery and Copilot's
`@mcp` search. To (re)publish it:

```
mcp-publisher login          # GitHub OIDC for the io.github.Gadriel-ai namespace
mcp-publisher publish        # reads server.json
```

**Instructions, prompts, agents:** copy the `.github/` directory into your repo:
- `.github/copilot-instructions.md` — repo-wide guidance (auto-applied)
- `.github/instructions/*.instructions.md` — 17 topic rules, scoped by `applyTo`
- `.github/prompts/*.prompt.md` — `/gadriel-scan`, `/gadriel-fix`, etc.
- `.github/agents/*.agent.md` — 8 reviewer agents

## Use

Ask Copilot Chat: **"Run a Gadriel security scan on this repo,"** or invoke a
prompt like `/gadriel-scan`. The `gadriel` MCP tools (`validate_file`,
`findings_for_path`, `fix_finding`, …) are available to Copilot in agent mode.

## Notes

- **No native edit/tool-use hook** exists in Copilot/VS Code (unlike Cursor and
  Codex), so the guardrail is not automatic here — enforcement runs through the
  MCP tools and prompts. For an always-on save-scan in VS Code, use the
  companion VS Code extension.
- **Enterprise:** Copilot Business/Enterprise can allowlist this MCP server via
  managed MCP policy.

## Network and data

Code is scanned locally and not uploaded. First run registers an anonymous
device credential with `app.gadriel.ai` (a random device id; no
hostname/username/keys) — set `GADRIEL_NO_ANONYMOUS_AUTH=1` to skip. See the
[privacy policy](https://gadriel.ai/privacy).

## License

This repository is [Apache-2.0](LICENSE). The `gadriel` scanner it runs is
proprietary, under the [Gadriel terms](https://gadriel.ai/terms).
