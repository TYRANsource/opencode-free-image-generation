# opencode-duck-art

Free image generation in OpenCode chat via [Duck.ai](https://duck.ai), no API key:

```
/gen landscape a ruined castle on a cliff at sunrise
```

## Install

With the OpenCode CLI:

```sh
opencode plugin add github:TYRANsource/opencode-duckai-image-generation
```

Without the CLI — clone into the auto-loaded plugins folder:

```sh
# per-project:
git clone https://github.com/TYRANsource/opencode-duckai-image-generation.git .opencode/plugins/opencode-duckai-image-generation

# ...or global (all projects):
git clone https://github.com/TYRANsource/opencode-duckai-image-generation.git ~/.config/opencode/plugins/opencode-duckai-image-generation
```

Restart OpenCode after either way.

## How it works

Duck.ai has no image API, so the plugin uses a local relay
([DuckAI2API](https://github.com/wuyouseo/DuckAI2API)): it drives a real
browser to get anti-bot tokens and serves an OpenAI-compatible endpoint.
The first `gen` installs the relay itself
(clone → venv → pip → Chromium → start); `duck_setup` does the same on demand.
Needs `git` and Python 3.11+ on `PATH`.
