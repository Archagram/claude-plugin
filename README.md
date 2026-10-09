# Archagram plugin for Claude

This repository is Archagram's plugin marketplace for Claude. It holds one plugin, `archagram`, in [plugins/archagram](plugins/archagram): the Archagram connector plus skills, commands and an agent for modeling software architecture in [Archagram](https://archagram.com) from Claude Code, Cowork and claude.ai.

What the plugin does, what it needs and how it handles your data: [plugins/archagram/README.md](plugins/archagram/README.md).

## Install

Claude Code, in a session:

```text
/plugin marketplace add Archagram/claude-plugin
/plugin install archagram@archagram
```

claude.ai, the Claude apps and Cowork: open Customize > Plugins, choose Add > Add marketplace, enter `Archagram/claude-plugin`, then add Archagram and connect it on the plugin's Connectors tab.

## Layout

- `.claude-plugin/marketplace.json` is the marketplace, named `archagram`, with one entry for the plugin.
- `plugins/archagram/` is the plugin folder. It is also the folder to submit to Anthropic's directory as a plugin bundle (plugin path `plugins/archagram`).

## Release a version

1. Raise `version` in `plugins/archagram/.claude-plugin/plugin.json`.
2. Add the release to `plugins/archagram/CHANGELOG.md` and to the version history in the plugin's README, naming the Archagram MCP server version and tool changes the skills follow.
3. Run `claude plugin validate --strict plugins/archagram` and `claude plugin validate --strict .`.
4. Push to `main`.

## Support

Email support@archagram.com.

## License

MIT, see [LICENSE](LICENSE).
