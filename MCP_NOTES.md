# Model Context Protocol (MCP) — Onboarding Reference Note

> A short primer for new engineers. Compiled from the official MCP documentation and Anthropic's launch announcement (see Sources).

## What is MCP?

The **Model Context Protocol (MCP)** is an open, open-source standard for connecting AI applications to external systems — data sources (local files, databases), tools (search engines, calculators) and workflows (specialized prompts). The project's own analogy: MCP is like a **USB-C port for AI applications** — one standardized connector instead of a bespoke integration per data source.

Before MCP, every new data source required its own custom connector, which did not scale. MCP replaces those fragmented integrations with a single protocol: build one MCP server, and any MCP-capable client can use it.

## Who introduced it, and when?

- MCP was **introduced and open-sourced by Anthropic on November 25, 2024**.
- It was **created at Anthropic by David Soria Parra and Justin Spahr-Summers**.
- At launch, Anthropic shipped three components: the MCP specification and SDKs, local MCP server support in the Claude Desktop apps, and an open-source repository of MCP servers (with pre-built servers for Google Drive, Slack, GitHub, Git, Postgres, and Puppeteer).
- Early adopters included Block and Apollo; development-tools companies such as Zed, Replit, Codeium, and Sourcegraph were also early participants.
- Today it is a broad open ecosystem: official clients include Claude and ChatGPT, plus development tools such as Visual Studio Code and Cursor.

## The client/server model

MCP uses a **client–server architecture** built on **JSON-RPC 2.0** messages. The three participants:

| Participant | Role |
| --- | --- |
| **MCP Host** | The AI application (e.g., Claude Desktop, Claude Code, VS Code) that coordinates one or more clients |
| **MCP Client** | A connector inside the host; the host creates one client per server, each maintaining a dedicated connection |
| **MCP Server** | A program that provides context and capabilities to clients; can run locally (stdio) or remotely (Streamable HTTP) |

The protocol is organized into two layers:

- **Data layer** — the JSON-RPC 2.0 protocol itself: capability and version discovery, plus the core primitives (tools, resources, prompts). MCP is stateless: every request carries its protocol version and relevant capabilities in its `_meta` field, and servers advertise their versions/capabilities through the mandatory `server/discover` request.
- **Transport layer** — how messages travel: **stdio** for local, same-machine servers (no network overhead), and **Streamable HTTP** for remote servers (HTTP POST, optional Server-Sent Events for streaming; OAuth is recommended for authentication).

### The three core server primitives

Servers expose functionality through three primitives, each with `*/list` methods for discovery and `*/get` (or `tools/call`) methods for retrieval/execution:

1. **Tools** — executable functions the AI application can invoke to perform actions: file operations, API calls, database queries. (`tools/list`, `tools/call`)
2. **Resources** — data sources that provide contextual information to the application: file contents, database records, API responses. (`resources/list`, `resources/get`)
3. **Prompts** — reusable templates that structure interactions with language models: system prompts, few-shot examples. (`prompts/list`, `prompts/get`)

**Concrete example:** a database MCP server can expose a *tool* for querying the database, a *resource* containing the database schema, and a *prompt* with few-shot examples for interacting with the tools.

### Client-side primitives and other capabilities

- **Elicitation** — lets a server request additional information from the user (e.g., confirmation of an action) via `elicitation/create`.
- **Notifications** — servers can push real-time updates (e.g., when their tool list changes); change notifications are opt-in, requested via a subscriptions/listen stream.
- The client-side *sampling* and *logging* primitives are deprecated as of protocol version 2026-07-28 (servers should integrate directly with LLM provider APIs, and log to stderr or OpenTelemetry instead).
- **Extensions** — optional, opt-in modules negotiated during initialization, including Tasks (asynchronous long-running operations), Skills over MCP, and MCP Apps (interactive UI elements rendered inside AI clients).

### Security principles (spec highlights)

- Explicit **user consent and control** over all data access and operations.
- Hosts must obtain user consent before exposing user data to servers, and must not transmit resource data elsewhere without consent.
- **Tools are arbitrary code execution**: hosts must obtain user consent before invoking any tool, and tool descriptions/annotations should be treated as untrusted.

MCP was explicitly inspired by the **Language Server Protocol (LSP)**, which standardized programming-language support across development tools; MCP aims to do the same for context and tools across AI applications.

## Where the spec and docs live

- **Documentation:** <https://modelcontextprotocol.io> — concepts, quickstarts, and guides for building servers, clients, and MCP Apps.
- **Specification:** <https://modelcontextprotocol.io/specification/latest> — the authoritative protocol requirements, derived from the TypeScript schema in `schema.ts`.
- **GitHub (source of the spec & SDKs):** <https://github.com/modelcontextprotocol> — the open-source organization; the spec itself lives in the [modelcontextprotocol/modelcontextprotocol](https://github.com/modelcontextprotocol/modelcontextprotocol) repository.

Note that MCP focuses purely on the context-exchange protocol — it does not dictate how AI applications use LLMs or manage the context they are given.

## Sources

Pages consulted while preparing this note:

1. [What is the Model Context Protocol (MCP)? — modelcontextprotocol.io](https://modelcontextprotocol.io/)
2. [Introducing the Model Context Protocol — Anthropic (Nov 25, 2024)](https://www.anthropic.com/news/model-context-protocol)
3. [Specification (latest) — modelcontextprotocol.io](https://modelcontextprotocol.io/specification/latest)
4. [Architecture overview — modelcontextprotocol.io](https://modelcontextprotocol.io/docs/concepts/architecture)
5. [modelcontextprotocol/modelcontextprotocol — GitHub](https://github.com/modelcontextprotocol/modelcontextprotocol)
