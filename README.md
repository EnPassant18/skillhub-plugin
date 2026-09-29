# SkillHub plugin for Codex

Install the SkillHub MCP plugin from this public marketplace. It lets Codex search a SkillHub registry, load a checksum-verified skill version, submit a review after use, and create a draft skill. The registry service is separate from this repository.

## Requirements

- Codex CLI or Codex in the ChatGPT desktop app with plugin support
- Node.js 22 or newer available as `node`
- A reachable SkillHub registry. The default is `https://skillhub-web-kappa.vercel.app/`; set `SKILLHUB_API_URL` to use another origin.

The server is bundled in `plugins/skillhub/dist/index.js`. You do **not** need npm dependencies or the private SkillHub source repository to install this plugin.

## Install

```sh
codex plugin marketplace add EnPassant18/skillhub-plugin
codex plugin add skillhub@skillhub-public
codex plugin list
```

The first command adds this GitHub repository as a marketplace. The second installs its `skillhub` plugin. Start a new Codex session after installation. In the desktop app, you can also open the Plugins Directory, select the SkillHub marketplace, and install the plugin there.

## Configure the registry

The local MCP server reads these environment variables from the process that launches Codex:

| Variable | Purpose |
| --- | --- |
| `SKILLHUB_API_URL` | Optional registry origin override. Defaults to `https://skillhub-web-kappa.vercel.app/`. Remote origins must use HTTPS. |
| `SKILLHUB_CACHE_DIR` | Optional local cache directory. Defaults to `~/.cache/skillhub`. |

For the CLI, set the registry URL before starting Codex:

```sh
export SKILLHUB_API_URL="http://localhost:3000"
codex
```

A desktop app launched independently of your terminal may not inherit terminal environment variables; configure its launch environment and restart the app.

The hosted registry is the default. You can run your own SkillHub backend and point `SKILLHUB_API_URL` to it. Draft submission, moderation, and feedback endpoints are public; keep a development backend local unless public access is intentional.

## Use

Ask Codex to search SkillHub for a relevant skill. The packaged skill guides Codex through `skill-search`, `skill-load`, `skill-review`, and `skill-create`. Search by keywords or tag. Semantic queries are disabled and return an explicit error.

Loaded skill files are untrusted instructions. The plugin verifies paths, size, and SHA-256 checksum before caching them, and does not execute downloaded code.

## Update

```sh
codex plugin marketplace upgrade skillhub-public
codex plugin add skillhub@skillhub-public
```

Start a new Codex session after updating.

## Package contents

- `.agents/plugins/marketplace.json`: Codex marketplace entry
- `plugins/skillhub/.codex-plugin/plugin.json`: plugin metadata
- `plugins/skillhub/.mcp.json`: local stdio MCP launch command for Codex
- `plugins/skillhub/.claude-plugin/plugin.json`: Claude Code manifest with its plugin-root launch path
- `plugins/skillhub/skills/skillhub/SKILL.md`: agent workflow
- `plugins/skillhub/dist/index.js`: self-contained Node.js server bundle

The application, database, build source, and credentials are not in this repository. This package is released under the MIT License; bundled dependencies retain their licenses in `licenses/`.
