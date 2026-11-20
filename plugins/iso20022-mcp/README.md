# ISO 20022 MCP (plugin)

Query the complete ISO 20022 financial messaging specification from your AI coding agent: the messages, their components and fields, code sets, business concepts, and what changed between versions. The plugin connects your agent to the hosted ISO 20022 MCP server and adds skills that teach the agent how to use it.

The server is a separate service of Beneficial Strategies, free to start (a time-limited trial, then a free plan) with paid plans for more. This plugin is open source under the [MIT License](../../LICENSE); the service has its own terms.

## Install

In Claude Code:

```
claude plugin marketplace add Beneficial-Strategies/claude-plugins
claude plugin install iso20022-mcp@beneficial-strategies
```

In Claude Desktop: Customize, Plugins, Add, Add marketplace, `Beneficial-Strategies/claude-plugins`, then Discover and install `iso20022-mcp`.

## Sign in

The first time the agent uses the server, your client opens a sign-in (Google or GitHub). No credit card is needed to start. You accept the service's agreement once, and some plans ask you to confirm a billing email address; the agent will tell you when a step is pending. If you cannot use OAuth, the server also accepts an API key (see the [connect page](https://iso20022-mcp.beneficialstrategies.com/connect)).

## First things to try

Type `/iso20022-mcp:start`, or just ask:

- "What can I do with the ISO 20022 server, and which plan am I on?"
- "Show me the fields of pain.001.001.12."
- "What code values does `ChargeBearerType1Code` define?"
- "What changed between pacs.008.001.08 and pacs.008.001.11?"
- "Which messages does FedNow use, and which should I implement first?"
- "Everything I have to implement to handle camt.053."

## Skills

| Skill | When it applies |
|---|---|
| `start` | what to do first, which plan applies, a license or billing step that holds a read, what to ask |
| `repository-guide` | how the specification is organized: layers, identifiers, choices, code sets |
| `popular-messages` | which messages are widely used, by scheme |

Skills are plain text of ours and contain none of ISO's material.

## Privacy

The plugin itself collects nothing and runs no code on your machine: it points your client at the hosted server. The server needs you to sign in with Google or GitHub, and receives your email address, profile name and the provider's account identifier. It logs which tools you call and which resources you read (by address), with the time and your plan, and keeps your account, subscription and agreements; it does not log the content of your conversations with your AI assistant, does not sell your data and does not use it to train AI models. Payments are handled by Stripe, so we never see your card details; email is sent through Resend; the contact form and billing contact are kept in HubSpot. How long data is kept and how to have it deleted are in the policy: <https://beneficialstrategies.com/privacy/>. Questions: [contact@beneficialstrategies.com](mailto:contact@beneficialstrategies.com).

## Help

[support@beneficialstrategies.com](mailto:support@beneficialstrategies.com), or the [product page](https://beneficialstrategies.com/product/mcp-server/). Privacy: <https://beneficialstrategies.com/privacy/>.

## Versions

The plugin follows [semantic versioning](https://semver.org/): a new skill or a new capability raises the minor number, a correction the patch number, and a change that removes or renames something people use the major number. The version in `.claude-plugin/plugin.json` is raised in the pull request that changes what the plugin gives its users (Claude Code updates a plugin only when its version changes), and not for a change to this repository's workflows or documents.
