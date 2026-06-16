# marp-template

A boilerplate for building presentations with [Marp](https://marp.app/), powered by
[Marp CLI **v4**](https://github.com/marp-team/marp-cli). Write slides in Markdown
(`src/slides.md`) and export to HTML, PDF, and PowerPoint in `dist/`.

## Requirements

- **Node.js 18+** (required by Marp CLI v4; Node 16 is no longer supported)
- A Chromium-based browser is used for PDF/PPTX/image export. Marp downloads one
  automatically if needed, or set `--browser` / `CHROME_PATH` to use your own.
  Marp v4 also supports Firefox via `--browser firefox`.

## Getting started

```sh
npm install
npm run build:all
```

## Project structure

```
src/
  slides.md     # Your presentation (Markdown + Marp directives)
  images/       # Image assets referenced from slides.md
dist/           # Build output (generated; not committed)
```

## Build scripts

| Script              | Description                                               |
| ------------------- | -------------------------------------------------------- |
| `npm run build:html`  | Build `dist/slides.html` and copy images into `dist/`.   |
| `npm run build:pdf`   | Build `dist/slides.pdf`.                                  |
| `npm run build:pptx`  | Build `dist/slides.pptx` (PowerPoint).                   |
| `npm run build:all`   | Clean, then build HTML, PDF, and PPTX.                    |
| `npm run watch:html`  | Rebuild HTML on changes.                                  |
| `npm run preview`     | Open the Marp preview window.                             |
| `npm run clean`       | Remove the `dist/` directory.                            |

Marp configuration (input/output directories, local-file access, HTML support)
lives in the `marp` block of `package.json`.

## Authoring notes

- The deck uses Marp's built-in `gaia` theme (inverted) at `16:9`.
- Multi-column layouts use the `.columns2` / `.columns3` CSS grid utilities
  defined in the front-matter `style:` block of `src/slides.md`.
- Marp v4 renders each slide as a `display: block` container and supports
  CSS nesting, so inline styles can be written in a cleaner, nested form.

## License

MIT — see [LICENSE](LICENSE).

