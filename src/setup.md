---
name: setup
description: Check that Klayson is connected and signed in, then set up the brand (website, product, style, real claims). Run when the person asks to set up or connect Klayson, or right after installing the plugin.
disable-model-invocation: true
---

{{generated}}

# Set up Klayson

Klayson is one MCP server, `https://api.klayson.com/mcp/full`, which this plugin connects. Sign-in is OAuth in the browser; there is no API key.

## 1. Check the connection

Call @get_me. It is read-only and free. A result with a `user` and an `org` is the only proof that Klayson is connected and signed in; a config entry, a server in a list or a tool list is not. Do not make anything, run anything that uses the plan's usage, or change the brand to test the connection.

If there are no Klayson tools, or the call fails:

- **Claude Code:** the plugin's server loads at the start of a session. Ask the person to start a new session, type `/mcp`, choose `klayson` and Authenticate.
- **Claude (web and desktop):** Customize, Plugins, Klayson, then its Connectors tab: add Klayson and connect it. Then switch it on in a new chat.
- **Codex:** the plugin loads in a new session. Ask the person to start one. Codex signs in to a server once with a command: `codex mcp login klayson` in a shell (in the Codex app: Settings, MCP servers, Authenticate). Without it the Klayson tools do not load.
- **The sign-in:** it opens in the person's browser. A new email makes a new Klayson account on the Free plan, with no card. Work email or Google. Never ask for a password, a key or a token in the chat.

Then call @get_me again before you say it worked.

## 2. Say what is connected

The workspace name, the plan and this month's usage, from @get_me, in one short line.

## 3. Set up the brand

If `brand` is null, set it up now. If there is a brand, ask whether they want to check the product and the style before the first ads; skip what they do not want.

{{include:brand-setup}}
