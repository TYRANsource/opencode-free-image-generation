# opencode-duck-art

OpenCode plugin: **`/gen` command**, **`gen`** and **`duck_setup`** tools — artwork through
[Duck.ai](https://duck.ai) (free, no API key), saved straight into your project.

```
/gen landscape a ruined castle on a cliff at sunrise
```

## Install

```sh
opencode plugin add github:TYRANsource/opencode-duckai-image-generation
```

Restart OpenCode (config-declared packages load at startup).

## Usage

```
/gen [preset] <what the image should show>
/gen icon a red potion bottle, pixel art
/gen a ruined castle on a cliff at sunrise
```

`gen` tool inputs:

| Param | What it does |
| ----- | ------------ |
| `prompt` | Required. Concrete visual description: subject, style, lighting, framing. |
| `preset` | Size / folder / style recipe. Default `landscape`. Built-ins: `square`, `landscape`, `portrait`, `icon`. |
| `n` | How many variants, 1–4. Defaults to the preset's count. |
| `size` | Overrides preset size: `1024x1024`, `1536x1024`, `1024x1536`. Hint only — Duck.ai may return a nearby size. |
| `out` | Output folder or explicit filename. Relative paths resolve from the project root, overrides the preset folder. |
| `model` | Duck.ai model id. Defaults to the configured model (`gpt-6-luna`). |
| `show` | Attach images to the conversation. Default `true`. |

`duck_setup` tool: installs / repairs / starts the local relay. Idempotent — skips
what is already there. `force: true` recreates the venv and reinstalls deps + Chromium.

## How it works

Duck.ai has no image API: a local relay
([DuckAI2API](https://github.com/wuyouseo/DuckAI2API)) drives a real browser
for the anti-bot tokens and exposes an OpenAI-compatible endpoint. The first
`gen` installs it itself (clone → venv → pip → Chromium → start); `duck_setup`
does the same on demand (`force: true` redoes a broken install).

Needs `git` and Python 3.11+ on `PATH`. The relay lives in
`%LOCALAPPDATA%/opencode/duck-relay` (`~/.local/share/...` elsewhere) and serves
`http://127.0.0.1:8181` by default.

## Settings

`.opencode/duck-art.json`, re-read on every call — no restart needed:

```jsonc
{
  "defaultPreset": "landscape",
  "outputDir": "assets/generated",
  "presets": {
    "concept": { "size": "1536x1024", "n": 4, "dir": "assets/concepts" }
  }
}
```

Presets (`square`, `landscape`, `portrait`, `icon`): `style` is appended to the
prompt. Requested sizes are composition hints — Duck.ai may return nearby ones.
Set `replacePresets: true` to drop the built-ins and keep only yours.

Env overrides: `DUCKAI_RELAY_DIR`, `DUCKAI_RELAY_PORT`, `DUCKAI_RELAY_URL`,
`DUCKAI_AUTO_START`, `DUCKAI_AUTO_SETUP`, `DUCKAI_CHROME_PATH`, `DUCKAI_IMAGE_MODEL`,
`DUCKAI_IMAGE_DIR`.

## Caveats

- `ERR_BN_LIMIT` / `ERR_CHALLENGE`: pause and retry.
- ~25–45 seconds per batch.
- Driving duck.ai through a relay may violate their ToS — a dev tool, not a pipeline.

## License

MIT
