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

## Skills

The plugin also carries skills: guidance the agent loads when your question calls for it.

| Skill | When it applies |
|---|---|
| `start` | what to do first with the server (also the command `/iso20022-mcp:start`): which plan applies, a license or billing step that holds a read, what to ask |
| `repository-guide` | how the ISO 20022 specification is organized: the business and message layers, how a message identifier is built, choice components, code set derivation, where to start |
| `popular-messages` | which ISO 20022 messages are most widely used in production, by scheme (Swift CBPR+, SEPA, T2, Fedwire, FedNow, CHAPS, CHIPS, RTP): which messages a scheme uses, what to implement first, how adoption ranks |

Skills are plain text of ours; they contain none of ISO's material. If you connect the server without this plugin, a short version of the guide is the server's resource `iso20022://instructions/repository-guide`.

## The libraries plugin

A second plugin in this marketplace, `iso20022-libraries`, has no server: it is guidance for the agent about **which of our ISO 20022 libraries to use in a C# or TypeScript project, and when** (and when not). It covers the released NuGet packages (`Primitives`, `Messages`, `Messages.Records`, `Factory`, `Messages.Samples`) and the npm pre-releases (`iso20022-validate`, `-types`, `-serialize`, `-react`, `-react-ui`, which cover a subset of the specification).

```
claude plugin install iso20022-libraries@beneficial-strategies
```

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

## How a change reaches people

This repository is the marketplace, so there is no separate publish step for Claude Code:

- **Claude Code and Claude Desktop** read the marketplace straight from `main`. Merging a pull request is what publishes it. The workflow `Validate plugins` runs `claude plugin validate --strict` on every pull request and on `main`; it is the only automatic check, and it publishes nothing.
- **Anthropic's plugin directory** is a separate, manual submission (a form, then a review). Nothing here submits to it. The plugins are checked with `--profile directory`, so a plugin that passes can be submitted.
- **The MCP Registry** (`server.json`) is also manual for now. The workflow `Publish to MCP Registry` does not publish today: its install step fails, and it skips publishing without a token that is not configured.

A plugin's `version` in its `plugin.json` is raised in the pull request that changes what its users get (Claude Code updates a plugin only when the version changes); see the plugin's README for the rule. A new version of the service itself is deployed separately, from its own repository; the plugin only points to its address.

## License

The contents of this repository (the plugin manifest, the skills and the reference text) are open source under the [MIT License](LICENSE). The license covers this repository only. It does not cover the hosted ISO 20022 MCP service the plugin connects to, which is a separate paid service governed by its own terms, nor the names and logos of Beneficial Strategies. This is not legal advice.

## ISO 20022 material

**ISO 20022 material.** The ISO 20022 specification, its schemas, code sets and their text are ISO's. ISO and its Registration Authority publish them to be used and reproduced freely under the [ISO 20022 Terms of use](https://www.iso20022.org/terms-use) and the [Intellectual Property Rights Policy](https://www.iso20022.org/intellectual-property-rights). **This is not the official ISO 20022 site. The sole source of up-to-date material is <https://www.iso20022.org/>.** ISO's material is provided "as is", without warranty. The license of this repository covers what we wrote, not ISO's material. This is not legal advice.
