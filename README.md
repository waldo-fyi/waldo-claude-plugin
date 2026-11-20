# Waldo Claude Plugin

The `waldo-fyi` Claude Code plugin — strategy-agent skills for ad intelligence, social listening, web research, trends research, data analysis, and archival knowledge — powered by the Waldo MCP connector at `https://mcp.waldo.fyi/strategy`.

Distributed via the public mirror at https://github.com/waldo-fyi/waldo-claude-plugin.

## This is a one-way mirror

The public repository is a **read-only mirror** of this directory in the private `waldo-fyi/waldo-agentic` monorepo. A GitHub Actions workflow rsyncs this directory to the public repo on every push to `main` that touches it.

**Direct edits to the public repo will be wiped on the next sync.** Pull requests, files added through the GitHub UI, and any other changes made on the mirror side are not preserved.

If you found this through the public repo and want to contribute or report an issue, please reach out via the channels listed in `SECURITY.md` — we'll route it back to the upstream source.

## Installation

Install via the Claude Code plugin marketplace:

```
/plugin marketplace add waldo-fyi/waldo-claude-plugin
/plugin install waldo-fyi@waldo-fyi
```

The skills call the Waldo MCP connector, which the plugin adds for you. Sign in to it with your Waldo account through `/mcp` (select `plugin:waldo-fyi:waldo`, then authenticate); until you do, the skills load but have no tools to call.

To offer the plugin to everyone working in a repository, commit this to its `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "waldo-fyi": { "source": { "source": "github", "repo": "waldo-fyi/waldo-claude-plugin" } }
  },
  "enabledPlugins": { "waldo-fyi@waldo-fyi": true }
}
```

## Getting updates

New versions arrive when you refresh the marketplace:

```
/plugin marketplace update waldo-fyi
/plugin update waldo-fyi@waldo-fyi
```

Or turn on auto-update for the `waldo-fyi` marketplace in `/plugin`, under Marketplaces; it's off by default for marketplaces outside Anthropic's own.

## License

Apache License 2.0 — see `LICENSE` and `NOTICE`.
