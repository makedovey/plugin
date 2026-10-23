# Dovey for Claude and Codex

[Dovey](https://makedovey.com) is an AI that makes and runs Meta ads. It reads your brand from its website, finds the winning ads in your category and from your competitors, writes a brief, and makes new Facebook and Instagram ads in your brand's colours, type and voice. Every ad is an editable design on a Dovey canvas.

This plugin brings Dovey into Claude (web, desktop and Claude Code) and Codex. It adds two things:

- **The Dovey server**, `https://api.makedovey.com/mcp`, with its tools: research, briefs, making ads, the canvas, the brand.
- **Three skills.** `dovey` is the know-how: start with `get_me`, set up the brand, research then brief then make, design an ad on the canvas, judge an ad, and what never to do. `setup` checks the connection and sets up the brand. `make-ads` runs research, brief and make for a number of ads.

## What you need

A Dovey account. Signing in for the first time makes one: the Free plan has 10 credits a month and needs no card. Research and new ads spend credits; reading what is there is free. Plans and prices are at [makedovey.com/pricing](https://makedovey.com/pricing). Your assistant's own plan is separate.

There is no API key. The first Dovey call opens Dovey's sign-in in your browser (work email or Google), and you approve the connection there.

## Install

### Claude (web and desktop)

1. Open [Customize, then Plugins](https://claude.ai/customize/plugins).
2. Choose Add, then Add marketplace, and enter `makedovey/plugin`.
3. Find Dovey in the list and choose Add.
4. Open Dovey, go to its Connectors tab, add Dovey and connect it. Dovey's sign-in opens.
5. In a new chat, ask "Use Dovey to set up my brand and make my first 3 ads." or type `/` and choose `dovey:setup`.

A plugin added on claude.ai is on your account, so it is in the desktop app, Cowork and Claude Code too.

### Claude Code

```sh
claude plugin marketplace add makedovey/plugin
claude plugin install dovey@dovey
```

Claude Code 2.1.292 or later does both in one command: `claude plugin install dovey --marketplace makedovey/plugin`. Inside a session (2.1.275 or later): `/plugin install dovey --marketplace makedovey/plugin`.

Then start a new session, type `/mcp`, choose `plugin:dovey:dovey` and Authenticate. Run `/dovey:setup` to check the connection and set up your brand, or `/dovey:make-ads 5 for the summer sale`.

### Codex

```sh
codex plugin marketplace add makedovey/plugin
```

Then in Codex, type `/plugins`, open the Dovey marketplace and install Dovey. Start a new session; the first Dovey call opens the sign-in. Type `$` to pick a Dovey skill by hand, or just ask for ads. In the ChatGPT desktop app, the marketplace shows as a source in the Plugins Directory, and the plugin's setup runs the `setup` skill.

### Any other MCP client (no plugin)

Dovey is one remote MCP server. Add it the way your client adds one:

- Address: `https://api.makedovey.com/mcp`
- Transport: Streamable HTTP
- Authentication: OAuth on the first call. No API key.

```sh
claude mcp add --transport http dovey https://api.makedovey.com/mcp
```

```toml
# ~/.codex/config.toml
[mcp_servers.dovey]
url = "https://api.makedovey.com/mcp"
```

Most other clients take `{"mcpServers": {"dovey": {"url": "https://api.makedovey.com/mcp"}}}`. Step by step guides for each app are at [makedovey.com/resources](https://makedovey.com/resources). Without the plugin you get the tools and Dovey's own instructions, but not the skills.

## What it runs, sends and fetches

The plugin is text: a server address and three skills. It installs no local server, no hooks, no scripts and no packages.

Your assistant sends the tool calls you ask for, and your sign-in, to `https://api.makedovey.com/mcp`. The calls can carry your brand's website, product names, photos you upload, your words for a brief or an edit, and ids of ads and canvases. Dovey answers with ads, briefs, pictures and links to your canvases on makedovey.com. Sign-in is OAuth through makedovey.com. In apps that show cards (Claude, ChatGPT), the cards load from Dovey's server and show pictures from Dovey's storage and from Facebook and Instagram.

Dovey never publishes to Meta, changes a plan or deletes anything on its own. Publishing is your own press on Dovey's Publish card.

[Privacy policy](https://makedovey.com/privacy) and [terms](https://makedovey.com/terms).

## Help

Tools missing: check the plugin is on, then the Dovey connector (Claude) or `/mcp` (Claude Code), and start a new session. Sign-in failed or expired: connect Dovey again. Anything else: daryl@makedovey.com with the app you use, what you asked and the error.

## How this folder is made

This folder is published from Dovey's own repository. The skills are generated from the same code the Dovey server speaks from: its instructions to agents (`api/mcp_srv/server.py`), the canvas design method and checklist (`api/core/design_guide.py`) and the tools' own descriptions, with short hand-written parts in `src/`. A test there fails when the two drift. Changes made directly here are replaced by the next release.

| File | For |
|---|---|
| `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Claude: the plugin and its marketplace |
| `.codex-plugin/plugin.json`, `.agents/plugins/marketplace.json` | Codex and ChatGPT: the plugin and its marketplace |
| `.mcp.json` | The Dovey server, for both |
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

The plugin's files are under the [MIT License](LICENSE). Dovey itself and its server are covered by the [terms](https://makedovey.com/terms).
