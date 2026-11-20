# Klayson for Claude and Codex

[Klayson](https://klayson.com) is the brand and design source of truth for agents. Each brand is a guide of pages (logo, colors, typography, voice, imagery, layout, components, and one for each medium such as web, app, slides and email), read from your website, your PDF guide and your code, and edited by your team. Your agent reads it before it builds or designs anything for the brand, reads the brand of any other website when the job needs one, and checks what it made against the brand.

This plugin brings Klayson into Claude (web, desktop and Claude Code) and Codex. It adds two things:

- **The Klayson brand server**, `https://api.klayson.com/mcp`, with its tools: the brand guide (`get_brand`, `get_page` and the edit tools), the Brand Extract API (`extract_brand`, `get_brand_extract`, `get_brand_materials`, `check_against_brand`, `get_api_credits`), the check (`check_design`), sending your codebase's design values (`capture_codebase`) and the Library of approved work.
- **Two skills.** `klayson` is the know-how: start with `get_me`, set up the brand, read the brand before you make anything, extract a site, check the result, and what never to do. `brand` is the same flow for a codebase, including a repo with no Klayson connection yet.

## What you need

A Klayson account. Signing in for the first time makes one on the Free plan, with no card. Reading another website's brand and checking a design use API credits: every workspace starts with 25 once, and more are on the API page ($20 for 100, $100 for 600, $500 for 3,500). Plans and prices are at [klayson.com/pricing](https://klayson.com/pricing). Your assistant's own plan is separate.

There is no API key. The first Klayson call opens Klayson's sign-in in your browser (work email or Google), and you approve the connection there.

## Install

### Claude (web and desktop)

1. Open [Customize, then Plugins](https://claude.ai/customize/plugins).
2. Choose Add, then Add marketplace, and enter `makedovey/plugin`.
3. Find Klayson in the list and choose Add.
4. Open Klayson, go to its Connectors tab, add Klayson and connect it. Klayson's sign-in opens.
5. In a new chat, ask "Use Klayson to set up my brand guide from my website." or type `/` and choose `klayson:brand`.

A plugin added on claude.ai is on your account, so it is in the desktop app, Cowork and Claude Code too.

### Claude Code

```sh
claude plugin marketplace add makedovey/plugin
claude plugin install klayson@klayson
```

Claude Code 2.1.292 or later does both in one command: `claude plugin install klayson --marketplace makedovey/plugin`. Inside a session (2.1.275 or later): `/plugin install klayson --marketplace makedovey/plugin`.

Then start a new session, type `/mcp`, choose `plugin:klayson:klayson` and Authenticate. Run `/klayson:brand` to set up your brand guide from your site and code, or just ask your agent to build something in your brand.

### Codex

```sh
codex plugin marketplace add makedovey/plugin
```

Then in Codex, type `/plugins`, open the Klayson marketplace and install Klayson. Start a new session and sign in once with `codex mcp login klayson` (in the Codex app: Settings, MCP servers, Authenticate). Type `$` to pick a Klayson skill by hand, or just ask. In the ChatGPT desktop app, the marketplace shows as a source in the Plugins Directory, and the plugin's setup runs the `brand` skill.

### Any other MCP client (no plugin)

Klayson is a remote MCP server. Add it the way your client adds one:

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

Most other clients take `{"mcpServers": {"klayson": {"url": "https://api.klayson.com/mcp"}}}`. The same sign-in also opens `https://api.klayson.com/mcp/full`, which lists the brand tools first and Klayson's older ad and canvas tools after them. Without the plugin you get the tools and Klayson's own instructions, but not the skills.

## What it runs, sends and fetches

The plugin is text: a server address and two skills. It installs no local server, no hooks, no scripts and no packages.

Your assistant sends the tool calls you ask for, and your sign-in, to `https://api.klayson.com/mcp`. The calls can carry your brand's website, its guide and the edits you make to it, the design values your agent reads from your code (tokens, fonts, components, each with its file and line, never secrets or application code), other websites you ask it to read, the HTML, page addresses or pictures it checks, and files you upload. Klayson answers with your brand's pages, the reads, the checks and links to files on klayson.com. Sign-in is OAuth through klayson.com.

Klayson never changes a plan, buys API credits, invites people or publishes a brand on its own. Those are your own acts in Klayson.

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
