# Waldo Claude Plugin

The `waldo-fyi` Claude Code plugin — strategy skills for ad intelligence, social listening, trends research, and archival knowledge — powered by the Waldo MCP connector at `https://mcp.waldo.fyi/strategy`. Open-web research and data analysis are left to your assistant's own tools.

Distributed via the public mirror at https://github.com/waldo-fyi/waldo-claude-plugin.

## This is a one-way mirror

The public repository is a **read-only mirror** of this directory in the private `waldo-fyi/waldo-agentic` monorepo. A GitHub Actions workflow rsyncs this directory to the public repo on every push to `main` that touches it.

**Direct edits to the public repo will be wiped on the next sync.** Pull requests, files added through the GitHub UI, and any other changes made on the mirror side are not preserved.

If you found this through the public repo and want to contribute or report an issue, please reach out via the channels listed in `SECURITY.md` — we'll route it back to the upstream source.

## Installation

The skills call the Waldo MCP connector, which the plugin brings with it. Until you sign in to it with your Waldo account, the skills load but have no tools to call.

### Claude Code

```
/plugin marketplace add waldo-fyi/waldo-claude-plugin
/plugin install waldo-fyi@waldo-fyi
```

Then sign in: `/mcp`, select `plugin:waldo-fyi:waldo`, and authenticate.

### claude.ai, Claude Desktop and Cowork

Plugins need a paid plan (Pro, Max, Ultra, Team or Enterprise).

1. Customize → Plugins → Add → Add marketplace, and enter `waldo-fyi/waldo-claude-plugin`.
2. Discover → select Waldo fyi → Install (labelled Add in some versions).
3. Open the plugin's Connectors tab, connect Waldo, and sign in.

A plugin installed on your claude.ai account also appears in Claude Code when you sign in there with the same account.

### For everyone working in a repository (Claude Code)

Commit this to the repository's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "waldo-fyi": { "source": { "source": "github", "repo": "waldo-fyi/waldo-claude-plugin" } }
  },
  "enabledPlugins": { "waldo-fyi@waldo-fyi": true }
}
```

This registers the marketplace for each person once they trust the folder; each person still installs it (`claude plugin install waldo-fyi@waldo-fyi --scope project`) and signs in.

### For an organization

**Claude Code.** An admin can add it to the organization's managed settings, which registers and enables it for every member and keeps it updated:

```json
{
  "extraKnownMarketplaces": {
    "waldo-fyi": {
      "source": { "source": "github", "repo": "waldo-fyi/waldo-claude-plugin" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": { "waldo-fyi@waldo-fyi": true }
}
```

If your organization restricts marketplaces with `strictKnownMarketplaces`, allow `{ "source": "github", "repo": "waldo-fyi/waldo-claude-plugin" }`. The entry must match exactly: a URL form or an added `ref` doesn't cover it.

**claude.ai Team and Enterprise.** Organization sync only accepts private or internal repositories, so it can't sync this one directly. Create a private repository holding only `.claude-plugin/marketplace.json`, with the plugin's source pointing here:

```json
{
  "name": "waldo",
  "owner": { "name": "<your organization>" },
  "plugins": [
    {
      "name": "waldo-fyi",
      "source": { "source": "github", "repo": "waldo-fyi/waldo-claude-plugin" },
      "description": "Waldo strategy skills for ad intelligence, social listening, trends research, and archival knowledge"
    }
  ]
}
```

Sync it from Organization settings → Plugins & skills, set the Waldo fyi plugin's access, and add the Waldo connector (`https://mcp.waldo.fyi/strategy`) under Organization settings → Connectors; each member then connects it with their own Waldo account. New Waldo releases reach the organization when that marketplace is re-synced.

## Getting updates

New versions arrive when you refresh the marketplace. In Claude Code:

```
/plugin marketplace update waldo-fyi
/plugin update waldo-fyi@waldo-fyi
```

Or turn on auto-update for the `waldo-fyi` marketplace in `/plugin`, under Marketplaces; it's off by default for marketplaces outside Anthropic's own. In claude.ai, use Check for updates on the marketplace, or turn on Sync automatically.

## License

Apache License 2.0 — see `LICENSE` and `NOTICE`.
