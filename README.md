# Propaired plugins

Connect your AI assistant to your own Propaired workspace, so you can run your real estate
business from the chat tool you already use.

This repository is a plugin marketplace. It currently holds one plugin:

| Plugin | What it does |
|---|---|
| [`propaired`](./propaired) | Your clients, leads, day plan, past sales, listings, market data and branded content — readable and draftable from your assistant, with every message, post and transaction change waiting for your approval inside Propaired. |

## Install

**Claude** (Claude Code, Claude Desktop, claude.ai)

```
/plugin marketplace add Propaired-ai/propaired-plugins
/plugin install propaired@propaired
```

**ChatGPT and Codex** — add this repository as a plugin source, then enable `propaired`.

Either way you will be asked to sign in to Propaired and choose an access level. Choose
**Governed authoring** if you want your assistant to draft emails, posts and presentations for
you. **Inspect** is read-only: your assistant can see your workspace but cannot prepare anything.

## What your assistant can and cannot do

It can read anything you can see in Propaired, and draft anything you would write.

It cannot send, publish, schedule, book or change a transaction on its own. Those come back to you
as an approval in Propaired with the exact recipient and the exact content, and nothing happens
until you tap approve. Your assistant cannot approve on your behalf.

## Where your data goes

Your assistant calls the Propaired MCP server at `https://www.propaired.ai/api/mcp` using your own
authenticated connection. It sees only your workspace — your clients, your leads, your listings.
Whatever your assistant reads is handled by whichever AI provider you are using, under their terms,
so treat it the way you would treat pasting the same information into that assistant yourself.

Revoke access any time from Settings → Integrations in Propaired.

## Development

The skills and manifests here are generated and verified from the Propaired application
repository, which holds the contract tests that check every capability a skill names is real,
reachable on the published catalog, and paired with a way to resume an approval. Edit them there.
