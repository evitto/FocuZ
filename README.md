# FocuZ

A minimal, single-file personal app launcher and dashboard. Add links to the tools and pages you use most, organize them into profiles, and open them from one clean home screen — installable as a PWA, works offline.

![theme](https://img.shields.io/badge/themes-light%20%7C%20dark%20%7C%20pink-informational)

## Features

- **App launcher** — add, edit, delete, and drag-to-reorder a personal list of apps (name, description, link).
- **Profiles** — keep separate sets of apps under different named profiles; switch between them from a dropdown.
- **Folder import** — pick a local folder and FocuZ scans it for `.html` files, adding one launcher entry per file automatically (no manual entry needed). Content is saved to IndexedDB so the links keep working across reloads. Use **Refresh** on a folder row to re-scan it for changes.
- **Themes** — Light, Dark, and Pink, picked from a segmented control in the Appearance tab. Saved and restored automatically.
- **Import / Export** — back up or transfer a profile as a JSON file.
- **Keyboard shortcut** — press `S` anywhere to open the Profile modal.
- **Installable PWA** — add it to your home screen/desktop and use it offline, like a native app.

## Getting Started

### Run it locally

Just open `index.html` in a browser. Everything (profiles, apps, theme, folder content) is stored locally in your browser via `localStorage` and `IndexedDB` — nothing is sent to a server.

> Note: the PWA install prompt and service worker only activate when served over `http(s)://`, not from a plain `file://` path — see below for a quick local server option.

### Serve it locally (for full PWA testing)

From the project folder:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000/`.

### Deploy to GitHub Pages

1. Push the folder structure below to a repo, keeping it intact — the app, manifest, and service worker all reference each other with relative paths.
2. In the repo settings, enable **GitHub Pages** for the branch/folder you pushed to.
3. Visit `https://<username>.github.io/<repo>/` — since the main file is `index.html`, it loads automatically at the root URL.

GitHub Pages serves over HTTPS automatically, which is required for the service worker and install prompt to work.

## File structure

```
index.html              — the app itself (HTML, CSS, and JS in one file)
manifest.json            — PWA manifest (name, icons, colors, display mode)
service-worker.js        — caches the app shell for offline use
LICENSE                  — MIT license
README.md                — this file
icons/
  icon-192.png            — app icon (192×192)
  icon-512.png            — app icon (512×512)
  icon-512-maskable.png   — Android adaptive icon (safe-zone sized)
  apple-touch-icon.png    — iOS home screen icon (180×180)
  favicon-32.png          — browser tab icon
```

## How folder import works

Because browsers don't expose real filesystem paths to web pages, FocuZ reads each picked `.html` file's content directly and stores it in IndexedDB, then generates a link from that saved copy. This means:

- Links keep working after closing and reopening the browser.
- If you edit a file on disk afterward, FocuZ won't see the change until you hit **Refresh** on that folder's row (browsers don't allow a page to silently watch a folder for changes).
- Folder-imported apps aren't included in JSON export, since the underlying content is local browser storage, not something meaningful to move to another device.

## Browser support

Built entirely on standard, widely supported web APIs — no vendor-specific or experimental features:

- `localStorage` / `IndexedDB` for saving state, theme, and folder-imported file content
- `<input type="file" webkitdirectory>` for the folder picker (despite the name, supported in all major browsers)
- Standard Web App Manifest + Service Worker for PWA installability and offline support

## License

MIT — see [LICENSE](./LICENSE) for details. Use and modify freely.
