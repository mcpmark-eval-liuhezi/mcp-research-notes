# github/github-mcp-server — Windows-Related Open Issues

Research notes for the team briefing on how the `github/github-mcp-server` project is dealing with Windows-related trouble reports.

Method: searched open issues in `github/github-mcp-server` for "Windows" and read each candidate issue plus its full comment thread. Several search hits were false positives (e.g., "changing chat *windows*", benchmark runs "on my Windows amd64 machine", and a search-syntax test query that happened to use the word). The three issues below are the ones with substantive Windows relevance.

## Issue #2823 — Insiders Feedback: MCP Apps: issue_write form widget collapses immediately in Claude clients — parameters never hydrate, submit impossible

- **Opened by:** M-H-H-B
- **Comments:** 10
- **Windows relevance:** The issue author reproduced the bug locally with the official v1.5.0 native binary on **Windows** (stdio, `GITHUB_INSIDERS=1`), and a later commenter reproduces it on Claude Desktop on **Windows 11**.
- **Summary:** MCP Apps interactive form widgets (`issue_write`, `create_pull_request`) render for a fraction of a second and then collapse into an error state; tool parameters never hydrate, the write never executes, and there is no fallback.
- **Latest comment note:** A commenter (nickytonline, 2026-10-20) supplies iframe-level evidence of where the form breaks on claude.ai web — the failing case never receives the final `tool-input` and the form iframe never sends `tools/call` — and hypothesizes that claude.ai treats the server's `IsError: true` deferral result as terminal, asking maintainers whether keeping the app mounted on an error result is a documented host requirement.

## Issue #1343 — GitHub MCP Server Tools Not Working in Copilot Spaces When Used in VS Code

- **Opened by:** HelenLyuRey
- **Comments:** 2
- **Windows relevance:** Reported on **Windows 11 Pro, version 24H2** (VS Code 1.105.1, Copilot extension 1.388.0).
- **Summary:** GitHub MCP Server search tools (`#search_repositories`, `#githubRepo`, code search) fail inside a Copilot Space in VS Code with "The GitHub code search index is not yet available," while the same tools work directly in Copilot Chat and in the web version of Copilot.
- **Latest comment note:** The stale bot (github-actions[bot], 2026-10-04) posted that the issue is stale after 30 days without activity and will be closed in 60 days unless someone comments — so this report is currently idle and at risk of auto-closing.

## Issue #3455 — Keep an opt-out for MCP Apps form deferral on the hosted remote server

- **Opened by:** amunoz-ldr
- **Comments:** 1
- **Windows relevance:** The single comment documents the same form-deferral failure inside the **Windows GitHub Copilot App** (version 1.0.93-1).
- **Summary:** After the `remote_mcp_ui_apps` flag was removed and form deferral made unconditional, clients whose forms never render have no path back to direct execution; the issue asks for a documented opt-out (e.g., honoring `mcp_apps_disable_form_deferral` on the hosted endpoint).
- **Latest comment note:** A commenter (Omzig, 2026-10-09) reports the identical symptom in the Windows GitHub Copilot App — the `issue_write` update returned `awaiting_user_submission` with no form visible and the update never performed — while the equivalent `gh.exe` call succeeded, showing the problem is not limited to Claude clients.
