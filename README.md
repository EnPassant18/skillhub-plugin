# SkillHub plugin for Codex

Install the SkillHub MCP plugin from this public marketplace. It lets Codex search a SkillHub registry, load a checksum-verified skill version, submit a review after use, and create a draft skill. The registry service is separate from this repository.

## Requirements

- Codex CLI or Codex in the ChatGPT desktop app with plugin support
- Node.js 22 or newer available as `node`
- A reachable SkillHub registry; set `SKILLHUB_API_URL` to its HTTPS origin, or run the registry locally at the default `http://localhost:3000`

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
| `SKILLHUB_API_URL` | Registry origin. Defaults to `http://localhost:3000`. Remote origins must use HTTPS. |
| `SKILLHUB_API_TOKEN` | Optional contributor or admin bearer token for reviews and draft submissions. Public search and loads can work without a token. |
| `SKILLHUB_CACHE_DIR` | Optional local cache directory. Defaults to `~/.cache/skillhub`. |

For the CLI, set the registry URL before starting Codex:

```sh
export SKILLHUB_API_URL="https://your-skillhub-registry.example"
codex
```

Supply `SKILLHUB_API_TOKEN` through your host's environment or secret settings if you need write operations. Do not commit tokens to this repository or put them in the plugin manifest. A desktop app launched independently of your terminal may not inherit terminal environment variables; configure its launch environment and restart the app.

This release does not include a hosted public registry URL or user sign-in. If you do not have a hosted registry, run your own SkillHub backend and point `SKILLHUB_API_URL` to it. The development backend uses single-user contributor/admin tokens and is not a public multi-user account service.

## Use

Ask Codex to search SkillHub for a relevant skill. The packaged skill guides Codex through `skill-search`, `skill-load`, `skill-review`, and `skill-create`. Search by keywords works without a semantic-search provider. A semantic query reports an explicit error if that provider is unavailable.

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
- `plugins/skillhub/.mcp.json`: local stdio MCP launch command
- `plugins/skillhub/skills/skillhub/SKILL.md`: agent workflow
- `plugins/skillhub/dist/index.js`: self-contained Node.js server bundle

The application, database, build source, and credentials are not in this repository. This package is released under the MIT License; bundled dependencies retain their licenses in `licenses/`.
