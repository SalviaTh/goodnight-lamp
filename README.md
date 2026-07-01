# Goodnight Lamp

A cute desk lamp that drifts off to sleep and wakes up - drawn in **SVG**, lit by **CSS**,
and switched by a **draggable pull-chain**. Grab the hanging cord, drag it down until it
*clicks*, and the lamp opens its eyes and turns its light on. Built with plain HTML, CSS,
and vanilla JavaScript - **zero runtime dependencies**.
interactive web page.

## Features

- Hand-drawn SVG lamp with a sleeping/awake face that crossfades on toggle
- The pull-chain is a real switch: drag it past a detent to click, release to spring back
- Tap or keyboard (`Enter` / `Space`) also toggles, with a scripted yank animation
- Optional click sound via the Web Audio API (silently no-ops if the file is missing)
- Respects `prefers-reduced-motion` (renders the lit state with no animation)
- Fully accessible: the cord is a focusable `button` with `aria-pressed`

## Quick start

**Prerequisites:** [Node.js](https://nodejs.org/) 18 or newer (ships with npm).

```bash
git clone <your-repo-url> cute-lamp
cd cute-lamp
npm install
npm run dev
```

Vite prints a local URL (default http://localhost:5173) - open it and pull the cord.

### Build for production

```bash
npm run build     # outputs static files to dist/
npm run preview   # serve the production build locally to verify
```

The `dist/` folder is a fully static site - drop it on any static host.

### No-build alternative

The site has no bundling step, so you can also serve the files directly without installing
anything (handy for a quick look). ES modules need HTTP, so don't open `index.html` over
`file://` - use a static server from the project root instead:

```bash
npx serve .
# or, with Python:
python3 -m http.server 8000   # then open http://localhost:8000
```

> Note: served this way, the favicon resolves from `public/favicon.png` only under Vite.
> With a plain static server the root-relative `/favicon.png` and `/sounds/...` paths
> resolve against the served root - `npm run dev`/`npm run build` handle this automatically
> by serving `public/` at `/`.

## How it works

| File         | Role                                                              |
| ------------ | ---------------------------------------------------------------- |
| `index.html` | The SVG lamp, the title, and the two CSS/JS source-code panes    |
| `lamp.css`   | All visuals and animation: the lamp, the on/off states, the glow |
| `lamp.js`    | Drag/tap/keyboard interaction, the detent click, optional sound  |

The cord is two nested SVG groups so it whips like a real pendulum. On `pointermove` the
script maps screen pixels to SVG units (so the rope tracks your pointer 1:1 at any size) and
fires the toggle the moment you cross the `DETENT`. On release it springs back to rest.

### URL parameters

- `?on` - hold the lamp lit (useful for a clean screenshot)
- `?record` - hold the intro until `window.start()` is called (used by a Shorts recorder)

## Customization

- **Sound:** replace `public/sounds/onOffSwitch.mp3`, or point the `SFX` object near the top
  of `lamp.js` at different files for distinct on/off clicks. The page works with no sound at
  all - a missing file just stays silent.
- **Favicon:** swap `public/favicon.png`.
- **Colors / glow:** the lamp's palette lives in the SVG gradients in `index.html` and the
  `.lamp` / `.is-on` rules in `lamp.css`.

## Deploy

Any static host works. For [Vercel](https://vercel.com/): import the repo, set the build
command to `npm run build` and the output directory to `dist` (Vite is auto-detected).

## License

[MIT](./LICENSE)
