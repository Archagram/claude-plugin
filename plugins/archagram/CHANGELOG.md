# Changelog

Every release raises `version` in `.claude-plugin/plugin.json` and names the Archagram MCP server version, and the tool changes, its skills follow.

## 0.1.0 (2026-10-09)

First release.

- Connects Claude to the Archagram MCP server at https://mcp.archagram.com/mcp over HTTP, with sign-in on archagram.com.
- Skills: modeling, code-to-architecture, update-after-change, write-adr, plan-target and explain-architecture.
- Commands: `/archagram:diagram-repo`, `/archagram:sync`, `/archagram:explain` and `/archagram:adr`.
- Agent: operator, for long read, validate and apply loops.
- Follows MCP server 0.11.0 and its 27 tools. `apply_notation`, `apply_notation_edits`, `write_thinking` and `apply_thinking_edits` require the `base_hash` of the read they were built from; the skills pass it on every write.
