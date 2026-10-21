# GitHub MCP Server Issue Triage Briefing — Windows-Related Reports

> Note: this file is a stand-in for the secret gist `mcp-triage-briefing.md`. The access token in use does not have the `gist` scope, so the gist could not be created programmatically. If you want the gist, the full content is below to paste in manually.

Prepared from research on open issues in `github/github-mcp-server` matching "Windows".

## Findings

A full-text search for "Windows" over open issues surfaced 7 hits, but only three are genuinely about Windows-related trouble; the rest matched incidentally (a report about changing chat *windows*, benchmark numbers produced "on my Windows amd64 machine", and a search-syntax test that used the word).

### Issue #2823 — Insiders Feedback: MCP Apps: issue_write form widget collapses immediately in Claude clients — parameters never hydrate, submit impossible
- Opened by M-H-H-B; 10 comments; open since 2026-07-06.
- MCP Apps interactive form widgets (`issue_write`, `create_pull_request`) flash blank and collapse to an error state; parameters never hydrate and the write never executes.
- Windows relevance: reproduced locally with the official v1.5.0 native binary on Windows; later repro on Claude Desktop on Windows 11.
- Latest comment (nickytonline, 2026-10-20): provides iframe-level DevTools evidence that the failing form never receives the final `tool-input` and never sends `tools/call`, hypothesizes that claude.ai treats the server's `IsError: true` deferral result as terminal, and asks whether keeping the app mounted on an error result is a documented host requirement.

### Issue #1343 — GitHub MCP Server Tools Not Working in Copilot Spaces When Used in VS Code
- Opened by HelenLyuRey; 2 comments; open since 2025-10-31.
- GitHub MCP search tools fail inside a Copilot Space in VS Code with "The GitHub code search index is not yet available," while working directly in Copilot Chat and on the web.
- Windows relevance: reported on Windows 11 Pro 24H2 (VS Code 1.105.1, Copilot extension 1.388.0).
- Latest comment (github-actions[bot], 2026-10-04): the stale bot warns the issue will be closed in 60 days without activity — the report is idle and at risk of auto-closing.

### Issue #3455 — Keep an opt-out for MCP Apps form deferral on the hosted remote server
- Opened by amunoz-ldr; 1 comment; open since 2026-10-08.
- After the `remote_mcp_ui_apps` flag was removed and form deferral made unconditional, clients whose forms never render have no path back to direct execution; asks for a documented opt-out such as honoring `mcp_apps_disable_form_deferral` on the hosted endpoint.
- Windows relevance: the lone comment documents the same failure in the Windows GitHub Copilot App.
- Latest comment (Omzig, 2026-10-09): reports the identical symptom in the Windows GitHub Copilot App 1.0.93-1 — `issue_write` returned `awaiting_user_submission` with no form visible and the update never performed, while `gh.exe` succeeded — showing the issue is not limited to Claude clients.

## Briefing Outline

- Scope & method: why we looked at this repo, and how the Windows-related issues were identified
- What "Windows trouble reports" actually means here: some hits were chat *windows*, others were real Windows OS / Windows Copilot App reports
- Issue #2823 — MCP Apps form widgets collapse and never hydrate (reproduced on Windows and Windows 11 Claude Desktop)
- Issue #1343 — Copilot Spaces + VS Code tool failures on Windows 11 (currently stale, at risk of auto-close)
- Issue #3455 — request for a form-deferral opt-out, confirmed in the Windows GitHub Copilot App
- Common thread: MCP Apps form deferral breaking writes on clients that don't render the form (including Windows clients)
- Workarounds in the threads worth passing to the team: the `?features=mcp_apps_disable_form_deferral` connector URL and `--features mcp_apps_disable_form_deferral` local flag
- Next steps: track upstream fixes, comment on #1343 to keep it alive, keep the briefing notes in this repo updated

## References

- Notes: https://github.com/mcpmark-eval-liuhezi/mcp-research-notes/blob/main/research/findings.md
- Tracking issue: https://github.com/mcpmark-eval-liuhezi/mcp-research-notes/issues/4
- Pull request: https://github.com/mcpmark-eval-liuhezi/mcp-research-notes/pull/5
- #2823: https://github.com/github/github-mcp-server/issues/2823
- #1343: https://github.com/github/github-mcp-server/issues/1343
- #3455: https://github.com/github/github-mcp-server/issues/3455
