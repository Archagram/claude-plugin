# Changelog

Every release raises `version` in `.claude-plugin/plugin.json` and names the Archagram MCP server version, and the tool changes, its skills follow.

## 0.2.0 (unreleased)

- Skill: draft-journey. It drafts a journey for one outcome from the code: it finds the entry point, follows the calls across components that exist in the architecture or a target, names each step's action, writes each payload under `payloads/` and each field mapping under `mappings/` with `write_file`, using made-up values and stripping anything that looks like real customer data, credentials or tokens, then writes the journey notation, validates, applies and reads it back. A journey keeps to about 5 to 15 activities; deeper detail goes into payloads and mappings.
- Command: `/archagram:journey <outcome>`.
- The modeling skill points to draft-journey for journeys.
- Follows MCP server 0.11.0 and its 27 tools. The journey grammar (actions, handoff styles, `payload`, `reply`, `timing`, `delivers` and `mapping` lines, journeys told over a target) comes from `get_notation_reference`.

## 0.1.0 (2026-10-09)

First release.

- Connects Claude to the Archagram MCP server at https://mcp.archagram.com/mcp over HTTP, with sign-in on archagram.com.
- Skills: modeling, code-to-architecture, update-after-change, write-adr, plan-target and explain-architecture.
- Commands: `/archagram:diagram-repo`, `/archagram:sync`, `/archagram:explain` and `/archagram:adr`.
- Agent: operator, for long read, validate and apply loops.
- Follows MCP server 0.11.0 and its 27 tools. `apply_notation`, `apply_notation_edits`, `write_thinking` and `apply_thinking_edits` require the `base_hash` of the read they were built from; the skills pass it on every write.
