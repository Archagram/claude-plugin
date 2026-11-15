# Archagram for Claude

[Archagram](https://archagram.com) is a workspace for software architecture: isometric diagrams backed by a text notation, with documents, decisions, future-state targets and delivery roadmaps in one project. This plugin connects Claude to your Archagram workspace and gives it the skills to model architecture well. Claude can diagram a repository, keep the diagram in step with code changes, explain a system to a newcomer, write architecture decision records, plan a target architecture with its roadmap, draft a journey from the code and prepare its screens, record what a running system was observed to do, and write a data model from a database's scripts.

It works in Claude Code, in Cowork, and in chat on claude.ai and the Claude apps.

## What it adds

- **The Archagram connector.** The Archagram MCP server at https://mcp.archagram.com/mcp. Claude reads and changes your Archagram projects through it, as you.
- **Commands.** In Claude Code and Cowork you type them; in chat Claude applies them when your request matches.
  - `/archagram:diagram-repo` diagrams the current repository as a new Archagram project.
  - `/archagram:sync` updates the diagram after a code change, from the git diff.
  - `/archagram:explain` explains a project's architecture to a newcomer.
  - `/archagram:adr` writes an architecture decision record.
  - `/archagram:journey` drafts a journey for one outcome from the code: its steps, the example payloads with made-up values and the field mappings.
  - `/archagram:screens` prepares the screens of a journey: what a person sees at its steps, inside a browser, a phone or another device.
  - `/archagram:investigate` records an investigation from network captures, logs, traces or code: what calls what, how often and with which results.
  - `/archagram:data-model` writes a data model from a database's create scripts, migrations or mapping code, or a concept model of a domain.
- **Skills** that Claude loads when a task matches: modeling in Archagram notation, diagramming a repository, updating a diagram after a change, writing an ADR, planning a target architecture and its roadmap (also available as `/archagram:plan-target`), drafting a journey from code, preparing its screens, recording an investigation, writing a data model, and explaining an architecture.
- **An operator agent** for Claude Code and Cowork. It runs long read, validate and apply loops on large projects in its own context.

The skills read the notation grammar from the Archagram server each time, so they follow the current notation. Where the server offers its guides, the skills read the guide for the job as well.

## Requirements

- An Archagram account on a plan that includes MCP: Pro, Team or Enterprise, or a seat in a Team or Enterprise workspace. While plans are not enforced during the beta, every account includes it. Archagram is in private beta, so a new account needs an invitation.
- Claude Code, or a Claude plan that can add plugins on claude.ai and in Cowork.

## Install

### Claude Code

In a session:

```text
/plugin marketplace add Archagram/claude-plugin
/plugin install archagram@archagram
```

Or from your shell:

```sh
claude plugin marketplace add Archagram/claude-plugin
claude plugin install archagram@archagram
```

Then sign in. Run `/mcp`, select `plugin:archagram:archagram` and choose Authenticate. Your browser opens archagram.com: sign in as usual, check that the consent page names Claude Code, and choose Allow.

### claude.ai, the Claude apps and Cowork

1. Open Customize > Plugins, choose Add > Add marketplace and enter `Archagram/claude-plugin`.
2. Add the Archagram plugin.
3. Open the plugin's Connectors tab and connect Archagram: sign in on archagram.com and choose Allow. On Claude's Team and Enterprise plans, an Owner adds the connector for the organization first, and each member then connects with their own Archagram account.

A plugin you add on claude.ai also loads in Claude Code when you sign in there with the same account.

## Use it

Try these:

- In a repository: `/archagram:diagram-repo`, or "Diagram this repository in Archagram."
- After a change: `/archagram:sync`, or "Update my Archagram diagram for the changes on this branch."
- "Explain how an order travels through Checkout Platform, for someone joining the team."
- `/archagram:adr We are moving session storage from Postgres to Redis.`
- In a repository: `/archagram:journey A customer places an order`, or "Draft the journey of a product change from the PIM to the ERP in Archagram."
- `/archagram:screens place-order`, or "Prepare the screens of the checkout journey, on a phone, for the store managers."
- `/archagram:investigate`, or "Record which endpoints the product page calls, from this HAR file."
- In a repository: `/archagram:data-model`, or "Write the data model of the shop database from its migrations."
- "In Payments, plan a target architecture for replacing our payment provider, with phases and a roadmap."

Before Claude changes a project, it reads the project's context documents and its current notation. Every change is checked against that read: if someone saved the project in the editor in the meantime, nothing is overwritten and Claude reads it again. Changes appear in the Archagram editor within seconds, with the layout you arranged kept in place.

## Data and privacy

- **The plugin itself** holds instructions and the connector's address: Markdown and JSON files, with no scripts, hooks or programs. It collects and sends nothing on its own.
- **Signing in.** You sign in on archagram.com and allow Claude on a consent page; Claude never sees your password. Access lasts an hour at a time and renews for up to 30 days, then Claude asks you to sign in again. You can disconnect at any time: in Claude Code from `/mcp`, in Claude from the connector's page.
- **What goes to Archagram.** Only the tool calls Claude makes for you, to mcp.archagram.com: project ids, notation, and the documents and files it writes. With `/archagram:diagram-repo`, `/archagram:sync`, `/archagram:journey`, `/archagram:screens`, `/archagram:investigate` and `/archagram:data-model`, Claude reads your repository, its git diff and the captures or logs you point it to on your machine and sends only what it derives from them: component names, descriptions, relationships and short documents; for a journey, its steps and screens with example payloads and field mappings written with made-up values; for an investigation, the map of what calls what with its counts and results, never the capture; for a data model, the tables, columns and keys, never rows of data. The skills tell Claude to keep source code, secret values, credentials and personal data out of the project.
- **What Archagram keeps.** The projects, files, thinking sessions, investigations and data models Claude creates or changes, like your own edits; diagram changes keep version history. For the connection itself, Archagram keeps what its privacy policy lists. Request logs hold the path, the result and the duration of a call, never its content. Archagram uses your content only to provide the service, never to train models.
- **Where tool results go.** To Claude, so it can answer you, under Anthropic's terms and privacy policy. Archagram does not receive your conversation.

Privacy policy: https://archagram.com/legal/privacy. Terms of service: https://archagram.com/legal/terms.

## Troubleshooting

- **Claude has no Archagram tools or says it is not connected.** In Claude Code, run `/mcp` and authenticate `plugin:archagram:archagram`. In Claude, open the plugin's Connectors tab and connect.
- **A tool answers with an upgrade link.** Your Archagram plan does not include MCP; the link shows the plans that do.
- **A change was refused with "Stale base_hash".** The project changed after Claude's last read, so nothing was written. Claude reads it again and redoes the change.
- **Claude sees two sets of Archagram tools.** You also added Archagram by hand, for example with `claude mcp add`. Remove one of the two.
- **The commands are missing.** Run `/reload-plugins` or restart Claude Code, and check that `claude plugin list` shows archagram as enabled.
- **Claude asks you to sign in again.** A connection lasts 30 days after you allow it.

## Support

Email support@archagram.com. Say which app you use (Claude Code, claude.ai, the desktop app or Cowork) and roughly when the problem happened. The connector itself is documented at https://archagram.com/docs/claude.

## Version history

- **0.3.0** (2026-11-15): screens for a journey, investigations and data models, with the journey-screens, record-investigation and write-data-model skills and `/archagram:screens`, `/archagram:investigate` and `/archagram:data-model`; a write to one notation file is checked against that file alone. Follows the Archagram MCP server 0.17.0 and its 51 tools, and reads the guides of 0.18.0 where the server offers them. Details in CHANGELOG.md.
- **0.2.0** (2026-11-01): drafting journeys from code, with the draft-journey skill and `/archagram:journey`, and notation files: the skills follow include lines and read the whole text with `full: true` where they need it. Follows the Archagram MCP server 0.12.0 and the journey notation of schema v59. Details in CHANGELOG.md.
- **0.1.0** (2026-10-09): first release. Follows the Archagram MCP server 0.11.0: 27 tools, with `base_hash` required on whole-document writes. Details in CHANGELOG.md.

## License

MIT, see LICENSE.
