# dsh-cowart — Cowart infinite canvas for DeepSeek Harness

A DeepSeek Harness plugin that embeds a [tldraw](https://github.com/tldraw/tldraw) infinite canvas in the DSH Web GUI (adapted from [zhongerxin/Cowart](https://github.com/zhongerxin/Cowart), MIT licensed):

- **Canvas**: the tldraw canvas is served same-origin by DSH under `/cowart/`, with a project-backed storage API and SSE live refresh.
- **Storage**: canvas data lives in the session workspace's `canvas/` directory (`canvas/pages/<page-id>/cowart-canvas.json` + image assets), so it versions with your project.
- **Agent tools**: 11 `cowart_*` tools (open canvas, read/write state, insert image, insert HTML draft, selection, reference images, download). Combined with any OpenAI-compatible image tools (`generate_image` / `edit_image`, e.g. the `dsh-image-tools` plugin in this repo) and a vision bridge (`modlens_read_image`), it powers a full **generate → annotate → refine** loop.
- **Persistent window**: the canvas opens in a floating window (draggable, resizable, pinnable to the right edge as a sidebar) that does NOT scroll away with the conversation.

## Layout

```
dsh-cowart/
  index.js              host plugin entry (tools, routes, skill)
  lib/                  storage + insert logic + webServer routes + skill
  client/client.js      web client: floating canvas window + message bridge
  canvas/               vendored Cowart tldraw app (patched for DSH), built lazily
  scripts/              build / verify / restart helpers
```

## Installation

Requires Node.js ≥ 20 and network access for the first build (the canvas app and `tldraw` are installed on demand).

```bash
# 1) install the plugin into your DSH web profile (client half must be a package)
cd /path/to/your/workspace
dsh plugin --profile web add link:/absolute/path/to/dsh-cowart

# 2) restart dsh web completely, then refresh http://127.0.0.1:3080
```

The bundle's `cordis.patch.yml` inserts the `cowart` plugin row automatically; `dsh.client.platform: web` + `exports["./client"]` make the web app load the client half.

On first use the host plugin builds the canvas app (`npm install` in `canvas/` + `vite build`) into `dist/cowart/`. You can also prebuild:

```bash
cd dsh-cowart && npm run build:canvas
```

## Usage

1. Say "Open the Cowart canvas" — the agent calls `cowart_open_canvas`; the canvas appears in a floating window (drag the title bar, resize via the bottom-right grip, click 📌 to pin it to the right edge like a sidebar).
2. Create an "AI image" frame and send a prompt — the request reaches the agent as `[cowart-request:ai_image]`; the agent generates an image matching the frame's aspect ratio (via `generate_image`) and calls `cowart_insert_image` to replace the frame; the canvas auto-refreshes via SSE.
3. Annotate an image (arrows/text) and click "按标注修改" — the annotated screenshot is saved into the canvas assets; the agent reads it (vision), edits it (`edit_image`), and inserts the result beside the original.
4. AI HTML frames and AI Slides use `cowart_insert_html_draft`.

All canvas data stays under `<workspace>/canvas/`.

## Security boundaries

- Canvas storage paths are validated to stay inside `<projectDir>/canvas`; page-asset URLs are path-checked.
- The canvas iframe is same-origin; the message bridge only forwards `dsh-cowart` branded messages to the current session's conversation.
- No API keys or credentials are handled by this plugin; image generation is delegated to your image tool provider.
- Analytics from the upstream canvas are disabled in embedded (DSH) mode.

## Verification

```bash
cd dsh-cowart && npm run check          # syntax checks
node scripts/verify.mjs --port=3080     # pre-flight check against a running instance
```

## Acknowledgements

Canvas capability based on [tldraw/tldraw](https://github.com/tldraw/tldraw); the canvas app is adapted from [zhongerxin/Cowart](https://github.com/zhongerxin/Cowart) (MIT).
