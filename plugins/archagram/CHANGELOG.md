# Changelog

Every release raises `version` in `.claude-plugin/plugin.json` and names the Archagram MCP server version, and the tool changes, its skills follow.

## 0.3.0 (2026-11-15)

- Skill: journey-screens. It prepares the screens of a journey, what a person sees at its steps inside a browser, a phone, a kiosk or another device: it reads the journey with `read_journey_screens`, adds the steps and handoffs the screens need, takes the words and the data of each screen from the code when the flow is built, writes the data a screen shows under `payloads/` with made-up values, then validates and writes with `validate_journey_screens` and `write_journey_screens`.
- Skill: record-investigation. It records what a running system was observed to do, from network captures, logs, traces or code: it counts on the machine, sends only the map of what calls what with its counts, protocols and results, links objects to components and data models, and leaves notes for the work that follows. The capture itself, with its cookies, tokens and personal data, never leaves the machine.
- Skill: write-data-model. It writes a data model from a database's create scripts, migrations or mapping code, or a concept model of a domain, with relations and the state machines of a status. Only the structure is sent, never rows of data.
- Commands: `/archagram:screens [journey]`, `/archagram:investigate [what to investigate]` and `/archagram:data-model [database or domain]`.
- Guides. Where the server offers `list_guides` and `get_guide`, the skills read the guide for the job (`journeys`, `journey-screens`, `targets-and-roadmaps`, `investigations`, `data-models`) and follow it where it differs. On a server without them the skills work as written.
- File hashes. A write to one notation file passes that file's own `hash` as the `base_hash`, so a save elsewhere in the project no longer refuses it; the modeling, plan-target and draft-journey skills and the operator agent do so, and chain the `file_hash` a write returns.
- The explain-architecture skill reads investigations and data models, and starts at `list_enterprises` for a question that spans projects. The modeling skill and the operator agent name the documents kept beside the notation and their grammar topics.
- Follows MCP server 0.17.0 and its 51 tools: the journey screens, investigation, data model and enterprise tools, and a hash per notation file. The guides are those of 0.18.0 (53 tools).

## 0.2.0 (2026-11-01)

- Skill: draft-journey. It drafts a journey for one outcome from the code: it finds the entry point, follows the calls across components that exist in the architecture or a target, names each step's action, writes each payload under `payloads/` and each field mapping under `mappings/` with `write_file`, using made-up values and stripping anything that looks like real customer data, credentials or tokens, then writes the journey notation, validates, applies and reads it back. A journey keeps to about 5 to 15 activities; deeper detail goes into payloads and mappings.
- Command: `/archagram:journey <outcome>`.
- The modeling skill points to draft-journey for journeys.
- Notation files. A default `read_notation` now returns the root file, with one `include` line per other notation file, and lists every file in `files`. The skills read the whole text with `full: true` before a removal, an id change or a large rewrite, the plan-target skill reworks a target through its `targets/` and `roadmaps/` files with `file`, and the modeling skill and the operator agent describe reading and changing one file.
- Follows MCP server 0.12.0 and its 27 tools. The journey grammar (actions, handoff styles, `payload`, `reply`, `timing`, `delivers` and `mapping` lines, journeys told over a target) comes from `get_notation_reference`.

## 0.1.0 (2026-10-09)

First release.

- Connects Claude to the Archagram MCP server at https://mcp.archagram.com/mcp over HTTP, with sign-in on archagram.com.
- Skills: modeling, code-to-architecture, update-after-change, write-adr, plan-target and explain-architecture.
- Commands: `/archagram:diagram-repo`, `/archagram:sync`, `/archagram:explain` and `/archagram:adr`.
- Agent: operator, for long read, validate and apply loops.
- Follows MCP server 0.11.0 and its 27 tools. `apply_notation`, `apply_notation_edits`, `write_thinking` and `apply_thinking_edits` require the `base_hash` of the read they were built from; the skills pass it on every write.
