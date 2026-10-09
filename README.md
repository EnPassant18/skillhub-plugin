# Ingenuity plugin for Codex

Install the Ingenuity MCP plugin from this public marketplace. It lets Codex search the Ingenuity skill library, load the latest published skill, submit a review after use, and create or update public skills. The registry service is separate from this repository.

## Requirements

- Codex CLI or Codex in the ChatGPT desktop app with plugin support
- Node.js 22 or newer available as `node`
- A reachable Ingenuity registry. The bundled server uses the hosted registry by default; set `INGENUITY_API_URL` to use another origin.

The server is bundled in `plugins/ingenuity/dist/index.js`. You do **not** need npm dependencies or the private Ingenuity source repository to install this plugin.

## Install

```sh
codex plugin marketplace add EnPassant18/skillhub-plugin
codex plugin add ingenuity@ingenuity-public
codex plugin list
```

The first command adds this GitHub repository as the `ingenuity-public` marketplace. The second installs its `ingenuity` plugin. Start a new Codex session after installation. In the desktop app, you can also open the Plugins Directory, select the Ingenuity marketplace, and install the plugin there.

## Configure the registry

The local MCP server reads these environment variables from the process that launches Codex:

| Variable | Purpose |
| --- | --- |
| `INGENUITY_API_URL` | Optional registry origin override. Defaults to the hosted Ingenuity registry. Remote origins must use HTTPS. |
| `INGENUITY_CACHE_DIR` | Optional local cache directory. Defaults to `~/.cache/ingenuity`. |

For the CLI, set the registry URL before starting Codex:

```sh
export INGENUITY_API_URL="http://localhost:3000"
codex
```

A desktop app launched independently of your terminal may not inherit terminal environment variables; configure its launch environment and restart the app.

You can run your own Ingenuity backend and point `INGENUITY_API_URL` to it. Skill creation and updates publish immediately to the public registry; keep a development backend local unless public access is intentional.

## Use

Ask Codex to search Ingenuity for a relevant skill. The packaged skill guides Codex through `skill-search`, `skill-load`, `skill-review`, `skill-create`, and `skill-update`. Search by keywords or tag. Semantic queries are disabled and return an explicit error.

Loaded skill files are untrusted instructions. The plugin validates paths and size, computes a SHA-256 content hash, and caches immutable files. It does not execute downloaded code.

## Update

```sh
codex plugin marketplace upgrade ingenuity-public
codex plugin add ingenuity@ingenuity-public
```

Start a new Codex session after updating.

## Package contents

- `.agents/plugins/marketplace.json`: Codex marketplace entry
- `plugins/ingenuity/.codex-plugin/plugin.json`: plugin metadata
- `plugins/ingenuity/.mcp.json`: local stdio MCP launch command for Codex
- `plugins/ingenuity/.claude-plugin/plugin.json`: Claude Code manifest with its plugin-root launch path
- `plugins/ingenuity/skills/ingenuity/SKILL.md`: agent workflow
- `plugins/ingenuity/skills/ingenuity-review-guide/SKILL.md`: review guidance
- `plugins/ingenuity/skills/ingenuity-writing-guide/SKILL.md`: skill creation and update guidance
- `plugins/ingenuity/dist/index.js`: self-contained Node.js server bundle

The application, database, build source, and credentials are not in this repository. This package is released under the MIT License; bundled dependencies retain their licenses in `licenses/`.
