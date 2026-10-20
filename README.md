# ISO 20022 MCP

Query the complete ISO 20022 financial messaging specification from your AI coding agent.

## What it does

Exposes the full ISO 20022 eRepository as MCP tools — 2,600+ message definitions,
12,000+ components, code sets, and migration paths. Built for developers implementing
ISO 20022 messages in financial systems.

**Example queries:**
- "Show me the elements of pain.001.001.12"
- "What code values does `ChargeBearerType1Code` define?"
- "What changed between pacs.008.001.08 and pacs.008.001.11?"
- "Find all message definitions in the payments clearing and settlement business area"

## Install the plugin (Claude Code)

This repository is a plugin marketplace, `beneficial-strategies`, and the ISO 20022 MCP is the plugin `iso20022-mcp` in it:

```
claude plugin marketplace add Beneficial-Strategies/claude-plugins
claude plugin install iso20022-mcp@beneficial-strategies
```

(On Claude Code 2.1.275 or later one step does both: `/plugin install iso20022-mcp --marketplace Beneficial-Strategies/claude-plugins`.) Then sign up as described below.

**Already installed it before the marketplace was renamed?** The plugin used to be installed as `iso20022-mcp@iso20022-mcp` from the repository `Beneficial-Strategies/iso20022-mcp`. A registered marketplace is keyed by its name, so it cannot be renamed in place: remove the old one, which also removes the plugin installed from it and its saved options, add the new one and install again:

```
claude plugin marketplace remove iso20022-mcp
claude plugin marketplace add Beneficial-Strategies/claude-plugins
claude plugin install iso20022-mcp@beneficial-strategies
```

If your settings name the old marketplace (`extraKnownMarketplaces`, `enabledPlugins`), change them to `beneficial-strategies` and `iso20022-mcp@beneficial-strategies`. GitHub redirects the old repository address, but the commands above use the new one.

## Connect

**Step 1:** Sign up at [iso20022-mcp.beneficialstrategies.com/connect](https://iso20022-mcp.beneficialstrategies.com/connect) — 30-day free trial, no credit card required.

**Step 2 (option A — OAuth):** Add to `claude_desktop_config.json` and sign in when prompted:

```json
{
  "mcpServers": {
    "iso20022": {
      "type": "http",
      "url": "https://iso20022-mcp.beneficialstrategies.com/mcp"
    }
  }
}
```

**Step 2 (option B — API key):** Generate a key from your dashboard, then add it as a header:

```json
{
  "mcpServers": {
    "iso20022": {
      "type": "http",
      "url": "https://iso20022-mcp.beneficialstrategies.com/mcp",
      "headers": {
        "Authorization": "ApiKey YOUR_API_KEY"
      }
    }
  }
}
```

## Plans

| Tier | Highlights |
|------|------------|
| Trial (30 days free) | Full Pro access — no credit card required |
| Pro | Message details, component exploration, migration paths, validation |
| Enterprise | Bulk operations, custom code sets, organization management |

## Support

[support@beneficialstrategies.com](mailto:support@beneficialstrategies.com)

## ISO 20022 material

**ISO 20022 material.** The ISO 20022 specification, its schemas, code sets and their text are ISO's. ISO and its Registration Authority publish them to be used and reproduced freely under the [ISO 20022 Terms of use](https://www.iso20022.org/terms-use) and the [Intellectual Property Rights Policy](https://www.iso20022.org/intellectual-property-rights). **This is not the official ISO 20022 site. The sole source of up-to-date material is <https://www.iso20022.org/>.** ISO's material is provided "as is", without warranty. The license of this repository covers what we wrote, not ISO's material. This is not legal advice.
