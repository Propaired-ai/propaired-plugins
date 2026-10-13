# Propaired for your AI assistant

Propaired is a real estate platform for agents in Utah. This plugin connects your assistant — Claude,
ChatGPT, Codex or Grok — to **your own** Propaired workspace, and teaches it how you run your
business.

Once it is installed, you can ask your assistant to plan your day, brief you before a listing
appointment or a buyer consult, write up what happened on a call, keep notes on a lead, pull market
numbers for an area, draft a week of social posts, or prepare an MLS listing draft — in the same chat
you already work in.

## What it can and cannot do

Your assistant can read anything you can see in Propaired, and draft anything you would write.

It cannot send, publish, schedule or book anything by itself. Every email, text, social post,
presentation and offer change comes back to you as an approval inside Propaired, showing the exact
recipient and the exact content before anything goes out. If you decline, nothing happens. Approvals
expire after 90 minutes, so nothing can sit around and fire later unnoticed.

It never publishes to the MLS. For a listing, it fills your draft in your own logged-in browser and
stops; you review and publish.

## What you need

- A Propaired account with a realtor profile.
- When you connect, choose **Governed authoring** if you want your assistant to draft and queue
  things for you. **Inspect** is read-only — useful if you only want it to look things up.

## Where your data goes

- The plugin itself contains no credentials and no data. It holds instructions plus the address of
  Propaired's server.
- Your assistant signs in to Propaired with your own account, through Propaired's sign-in page. You
  choose the access level, and you can revoke it at any time from Propaired's integration settings.
- When you ask a question, the parts of your Propaired workspace needed to answer it are sent to
  whichever assistant you are using, and are handled under that provider's terms. Propaired records
  which capability ran, when, and for which connection — not the contents of your messages.
- Nothing is shared with other agents, other brokerages, or Propaired's other customers.

## Installing

In Claude:

```
/plugin install propaired --marketplace Propaired-ai/propaired-plugins
```

On Claude Code older than 2.1.275, add the marketplace first:

```
/plugin marketplace add Propaired-ai/propaired-plugins
/plugin install propaired@propaired
```

Step-by-step instructions for ChatGPT, Codex and Grok are on the setup page:
https://www.propaired.ai/realtors/ai-agents

## Links

- Setup and troubleshooting: https://www.propaired.ai/realtors/ai-agents
- Privacy policy: https://www.propaired.ai/privacy
- Terms of service: https://www.propaired.ai/terms
