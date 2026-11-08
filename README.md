# Shotboard MCP

Use [Shotboard](https://getshotboard.com) from Claude Code, Codex, Gemini CLI, Grok, Cursor or any MCP client.

Shotboard is a Mac app for making marketing content with AI: ads, UGC videos, App Store screenshots and thumbnails, on one canvas. It has 100 image, video and audio models from fal, Replicate, Higgsfield, xAI and Gemini, used with your own keys and no markup. While the app is open, it serves its canvas as MCP tools at `http://127.0.0.1:4317/mcp`, so your agent can search ads, write scripts and generate media on your board.

This repo has the connection setup and the built-in skills. The app itself is at [getshotboard.com/download](https://getshotboard.com/download).

## Requirements

- Shotboard for Mac (macOS 14 or later, Apple Silicon), open while the agent runs.
- Shotboard Pro to use it from your own terminal or editor. Every new account gets Pro free for 7 days, no card.
- A key for at least one media provider (fal, Replicate, Higgsfield, xAI or Gemini) or a ChatGPT sign-in, set in the app.

The server listens on `127.0.0.1` only. Nothing is exposed to the network.

## Connect

Run one command once. Shotboard → Settings → MCP shows the same commands.

**Claude Code**

```sh
claude mcp add --transport http shotboard http://127.0.0.1:4317/mcp
```

Or install this repo as a plugin; it ships `.mcp.json` and the skills.

**Codex**

```sh
codex mcp add shotboard --url http://127.0.0.1:4317/mcp
```

**Gemini CLI**

```sh
gemini mcp add --transport http -s user --trust shotboard http://127.0.0.1:4317/mcp
```

**Grok**

```sh
printf '\n[mcp_servers.shotboard]\nurl = "http://127.0.0.1:4317/mcp"\n' >> ~/.grok/config.toml
```

**Cursor and other clients** — add to your MCP config (e.g. `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "shotboard": { "url": "http://127.0.0.1:4317/mcp" }
  }
}
```

## What the agent can do

Then ask in plain words, for example:

- "Find Cal AI's ads that have run the longest and keep the top three on the board."
- "Copy the script of this TikTok and break it into hook, problem, demo, proof, offer and CTA."
- "Write five new hooks for it and make a 9:16 clip for each with Veo 3.1."
- "Reframe the winner to 4:5 and 1:1."

### Tools

| Area | Tools |
|---|---|
| Boards | `list_projects`, `create_project`, `rename_project`, `get_project`, `find_nodes`, `get_nodes`, `add_text_node`, `update_node`, `arrange_nodes`, `star_nodes`, `duplicate_nodes`, `delete_nodes`, `restore_nodes` |
| Generate | `list_models`, `list_tasks`, `list_task_models`, `set_task_model`, `generate_media`, `wait_for_nodes`, `cancel_nodes`, `regenerate`, `vary_node`, `estimate_cost`, `estimate_variants` |
| Edit | `upscale_node`, `remove_background`, `outpaint_node`, `next_shot`, `trim`, `extract_frame`, `extract_audio`, `compose_video` |
| Lineage | `get_lineage`, `get_prompt`, `list_versions`, `use_version`, `refresh_downstream`, `list_recent_prompts` |
| Ads and links | `search_ads`, `suggest_advertisers`, `keep_ad`, `import_link`, `resolve_link`, `import_file`, `retry_import` |
| Read media | `transcribe_speech`, `read_on_screen_text` (Apple on-device speech and text recognition) |
| Brand | `get_brand`, `save_brand`, `list_brands`, `scrape_page` |
| Skills | `list_skills`, `get_skill`, `save_skill` |
| Providers | `list_providers`, `get_plan` |

The server's own instructions explain the request shape (`generate_media` with `kind`, `model` or `task`, `prompt`, `inputs`) and how to chain steps with `{"$node": "<id>"}` references.

## Skills

Skills are recipes the agent follows with these tools. In Shotboard you run one with `$name` or `/name`; save your own with `save_skill`.

- [`talking-avatar`](skills/talking-avatar/SKILL.md) — a presenter on a green screen who speaks your script.
- [`shot-with-sound`](skills/shot-with-sound/SKILL.md) — a still, animated, with matching sound effects, merged into one clip.

## Pricing

Free: every model and provider, one board with 150 items. Pro: $19 a month or $190 a year, unlimited boards and items, all ad results and history, your own skills, and this terminal access. You pay your AI provider directly for what you make.

## License

The files in this repo (docs, config and skills) are MIT licensed. The Shotboard app is proprietary; see [getshotboard.com/terms](https://getshotboard.com/terms).
