---
name: setup
description: Check that Dovey is connected and signed in, then set up the brand (website, product, style, real claims). Run when the person asks to set up or connect Dovey, or right after installing the plugin.
disable-model-invocation: true
---

{{generated}}

# Set up Dovey

Dovey is one MCP server, `https://api.makedovey.com/mcp`, which this plugin connects. Sign-in is OAuth in the browser; there is no API key.

## 1. Check the connection

Call @get_me. It is read-only and free. A result with a `user` and an `org` is the only proof that Dovey is connected and signed in; a config entry, a server in a list or a tool list is not. Do not make anything, spend credits or change the brand to test the connection.

If there are no Dovey tools, or the call fails:

- **Claude Code:** the plugin's server loads at the start of a session. Ask the person to start a new session, type `/mcp`, choose `dovey` and Authenticate.
- **Claude (web and desktop):** Customize, Plugins, Dovey, then its Connectors tab: add Dovey and connect it. Then switch it on in a new chat.
- **Codex:** the plugin loads in a new session. Ask the person to start one; the first Dovey call opens the sign-in.
- **The sign-in:** it opens in the person's browser. A new email makes a new Dovey account on the Free plan, with no card. Work email or Google. Never ask for a password, a key or a token in the chat.

Then call @get_me again before you say it worked.

## 2. Say what is connected

The workspace name, the plan and the credits left, from @get_me, in one short line.

## 3. Set up the brand

If `brand` is null, set it up now. If there is a brand, ask whether they want to check the product, the style and the claims before the first ads; skip what they do not want.

{{include:brand-setup}}
