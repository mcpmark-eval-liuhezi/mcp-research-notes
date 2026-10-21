# Briefing outline — github-mcp-server Windows-related issue triage

- Scope & method: why we looked at this repo, and how the Windows-related issues were identified
- What "Windows trouble reports" actually means here: some hits were chat *windows*, others were real Windows OS / Windows Copilot App reports
- Issue #2823 — MCP Apps form widgets collapse and never hydrate (reproduced on Windows and Windows 11 Claude Desktop)
- Issue #1343 — Copilot Spaces + VS Code tool failures on Windows 11 (currently stale, at risk of auto-close)
- Issue #3455 — request for a form-deferral opt-out, confirmed in the Windows GitHub Copilot App
- Common thread: MCP Apps form deferral breaking writes on clients that don't render the form (including Windows clients)
- Workarounds in the threads worth passing to the team: the `?features=mcp_apps_disable_form_deferral` connector URL and `--features mcp_apps_disable_form_deferral` local flag
- Next steps: track upstream fixes, comment on #1343 to keep it alive, keep the briefing notes in this repo updated
