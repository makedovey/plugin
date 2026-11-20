# Klayson for Claude and Codex

[Klayson](https://klayson.com) is the studio where you design ads, social posts and other marketing for your brand, with Klayson's chat or with your own AI agent. Every design is an editable frame on a Klayson canvas. Klayson is also your brand's source of truth for agents: each brand has a guide in eight sections (brand, voice, logo, color, typography, imagery, in use, products), read from your website or your PDF guide and edited by you, and whatever your agent designs or builds for the brand, an ad, a website or a deck, follows it.

This plugin brings Klayson into Claude (web, desktop and Claude Code) and Codex. It adds two things:

- **The Klayson server**, `https://api.klayson.com/mcp`, with its tools: the brand guide, research, briefs, making ads and posts, the canvas.
- **Three skills.** `klayson` is the know-how: start with `get_me`, set up the brand, read and follow the brand guide, research then brief then make, design an ad on the canvas, judge an ad, and what never to do. `setup` checks the connection and sets up the brand. `make-ads` runs research, brief and make for a number of ads.

## What you need

A Klayson account. Signing in for the first time makes one: the Free plan has limited usage each month and needs no card. Research and new ads use the plan's monthly usage; reading what is there uses almost none. Plans and prices are at [klayson.com/pricing](https://klayson.com/pricing). Your assistant's own plan is separate.

There is no API key. The first Klayson call opens Klayson's sign-in in your browser (work email or Google), and you approve the connection there.

## Install

### Claude (web and desktop)

1. Open [Customize, then Plugins](https://claude.ai/customize/plugins).
2. Choose Add, then Add marketplace, and enter `makedovey/plugin`.
3. Find Klayson in the list and choose Add.
4. Open Klayson, go to its Connectors tab, add Klayson and connect it. Klayson's sign-in opens.
5. In a new chat, ask "Use Klayson to set up my brand and make my first 3 ads." or type `/` and choose `klayson:setup`.

A plugin added on claude.ai is on your account, so it is in the desktop app, Cowork and Claude Code too.

### Claude Code

```sh
claude plugin marketplace add makedovey/plugin
claude plugin install klayson@klayson
```

Claude Code 2.1.292 or later does both in one command: `claude plugin install klayson --marketplace makedovey/plugin`. Inside a session (2.1.275 or later): `/plugin install klayson --marketplace makedovey/plugin`.

Then start a new session, type `/mcp`, choose `plugin:klayson:klayson` and Authenticate. Run `/klayson:setup` to check the connection and set up your brand, or `/klayson:make-ads 5 for the summer sale`.

### Codex

```sh
codex plugin marketplace add makedovey/plugin
```

Then in Codex, type `/plugins`, open the Klayson marketplace and install Klayson. Start a new session and sign in once with `codex mcp login klayson` (in the Codex app: Settings, MCP servers, Authenticate). Type `$` to pick a Klayson skill by hand, or just ask. In the ChatGPT desktop app, the marketplace shows as a source in the Plugins Directory, and the plugin's setup runs the `setup` skill.

### Any other MCP client (no plugin)

Klayson is one remote MCP server. Add it the way your client adds one:

- Address: `https://api.klayson.com/mcp`
- Transport: Streamable HTTP
- Authentication: OAuth on the first call. No API key.

```sh
claude mcp add --transport http klayson https://api.klayson.com/mcp
```

```toml
# ~/.codex/config.toml
[mcp_servers.klayson]
url = "https://api.klayson.com/mcp"
```

An agent that only needs the brand, to build a website, a deck or code, can connect `https://api.klayson.com/mcp/brand` instead: the same sign-in and brands with only the brand tools (the brand guide, the brand reads, the check, the brand list).

```sh
claude mcp add --transport http klayson-brand https://api.klayson.com/mcp/brand
```

Most other clients take `{"mcpServers": {"klayson": {"url": "https://api.klayson.com/mcp"}}}`. Step by step guides for each app are at [klayson.com/mcp](https://klayson.com/mcp). Without the plugin you get the tools and Klayson's own instructions, but not the skills.

## What it runs, sends and fetches

The plugin is text: a server address and three skills. It installs no local server, no hooks, no scripts and no packages.

Your assistant sends the tool calls you ask for, and your sign-in, to `https://api.klayson.com/mcp`. The calls can carry your brand's website, its guide and the edits you make to it, product names, photos and files you upload, your words for a brief or an edit, and ids of ads and canvases. Klayson answers with ads, briefs, pictures and links to your canvases on klayson.com. Sign-in is OAuth through klayson.com. In apps that show cards (Claude, ChatGPT), the cards load from Klayson's server and show pictures from Klayson's storage and from Facebook and Instagram.

Klayson never publishes to Meta, changes a plan or deletes anything on its own. Publishing is your own press on Klayson's Publish card.

[Privacy policy](https://klayson.com/privacy) and [terms](https://klayson.com/terms).

## Help

Tools missing: check the plugin is on, then the Klayson connector (Claude) or `/mcp` (Claude Code), and start a new session. Sign-in failed or expired: connect Klayson again. Anything else: daryl@klayson.com with the app you use, what you asked and the error.

## How this folder is made

This folder is published from Klayson's own repository. The skills are generated from the same code the Klayson server speaks from: its instructions to agents (`api/mcp_srv/server.py`), the canvas design method and checklist (`api/core/design_guide.py`) and the tools' own descriptions, with short hand-written parts in `src/`. A test there fails when the two drift. Changes made directly here are replaced by the next release.

| File | For |
|---|---|
| `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Claude: the plugin and its marketplace |
| `.codex-plugin/plugin.json`, `.agents/plugins/marketplace.json` | Codex and ChatGPT: the plugin and its marketplace |
| `.mcp.json` | The Klayson server, for both |
| `skills/` | The skills, for both. `agents/openai.yaml` beside a skill is Codex's display and invocation settings |
| `src/` | The hand-written parts the skills are built from |

Formats follow these docs (read 2026-10-23):

- Claude Code plugins reference: https://code.claude.com/docs/en/plugins-reference
- Claude Code marketplaces: https://code.claude.com/docs/en/plugin-marketplaces and https://code.claude.com/docs/en/plugins/marketplace-reference.md
- Claude Code skills: https://code.claude.com/docs/en/skills
- Publishing and the directory: https://code.claude.com/docs/en/plugins/publish.md and https://claude.com/docs/plugins/pre-submission-checklist
- Installing: https://code.claude.com/docs/en/plugins/install.md and https://claude.com/docs/plugins/overview
- OpenAI plugins (Codex and ChatGPT), packaging and marketplaces: https://developers.openai.com/codex/plugins/build.md (also at https://developers.openai.com/plugins/build/plugins)
- Codex plugins for users: https://developers.openai.com/codex/plugins.md
- Codex skills and `agents/openai.yaml`: https://developers.openai.com/codex/skills.md
- A working example of one repository for both: https://github.com/StewdioAI/plugin, and OpenAI's own plugins at https://github.com/openai/plugins

## License

The plugin's files are under the [MIT License](LICENSE). Klayson itself and its server are covered by the [terms](https://klayson.com/terms).
