# Echoes — Web Build

Prebuilt browser export of [Echoes](https://github.com/eternalprogrammer/echoes) (placeholder URL), a 3D castle-explorer game made in Godot 4.6.

This repo holds **only the compiled web artifacts** — the Godot source lives in the parent project, which references this folder as a git submodule. Every push to `main` here triggers a GitHub Pages deploy via `.github/workflows/deploy-pages.yml`.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | Page that boots the Godot WASM runtime |
| `index.js` | Engine bootstrap loader |
| `index.wasm` | Compiled Godot engine (~36 MB) |
| `index.pck` | Game data (scenes, scripts, resources) |
| `index.audio.*.worklet.js` | Audio worklet helpers |
| `index.*.png` | Icons and loading splash |

## Updating the build

The build is regenerated from the parent project. In the parent repo:

```sh
/Applications/Godot.app/Contents/MacOS/Godot \
  --headless \
  --path . \
  --export-release "Web" web/index.html
```

That writes new artifacts into this submodule. Then, inside `web/`:

```sh
git add -A
git commit -m "Rebuild web export"
git push
```

The push triggers the Pages workflow.

## GitHub Pages setup (one-time)

1. Settings → Pages → Build and deployment → Source: **GitHub Actions**.
2. Push to `main`. The workflow uploads the repo root as the Pages artifact and deploys.

## Notes

- The build was exported with `variant/thread_support = false`, so it does not require `Cross-Origin-Opener-Policy` / `Cross-Origin-Embedder-Policy` headers. GitHub Pages doesn't send those by default, so this is intentional. If you enable threading later, you'll need a host that sets those headers.
- The `.gitignore` excludes `*.import` files (Godot editor metadata that is regenerated on every export and isn't needed by the browser).
