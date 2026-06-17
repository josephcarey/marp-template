# marp-template

A boilerplate for building presentations with [Marp](https://marp.app/), powered by
[Marp CLI **v4**](https://github.com/marp-team/marp-cli). Write slides in Markdown
(`src/slides.md`) and export to HTML, PDF, and PowerPoint in `dist/`.

Themes and layouts come from the shared
[`marp-theme-kit`](https://github.com/josephcarey/marp-theme-kit) library, consumed as a
devDependency — so decks across the studio share one design system.

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

| Script               | Description                                              |
| -------------------- | -------------------------------------------------------- |
| `npm run build:html` | Build `dist/slides.html` and copy images into `dist/`.   |
| `npm run build:pdf`  | Build `dist/slides.pdf`.                                  |
| `npm run build:pptx` | Build `dist/slides.pptx` (PowerPoint).                   |
| `npm run build:all`  | Clean, then build HTML, PDF, and PPTX.                    |
| `npm run watch:html` | Rebuild HTML on changes.                                 |
| `npm run preview`    | Open the Marp preview window.                            |
| `npm run clean`      | Remove the `dist/` directory.                            |

Marp configuration (local-file access, HTML support) lives in the `marp` block of
`package.json`.

## The theme kit

This template gets its look from the published
[`marp-theme-kit`](https://github.com/josephcarey/marp-theme-kit) library, pinned in
`package.json`:

```json
"devDependencies": {
  "marp-theme-kit": "github:josephcarey/marp-theme-kit#v0.3.0"
}
```

The kit ships a committed `dist/` of compiled themes, so a plain `npm install` populates
`node_modules/marp-theme-kit/dist/*.css` with no build step. The build scripts register
that whole directory as a theme set via Marp's `--theme-set` flag:

```sh
marp src/slides.md --theme-set node_modules/marp-theme-kit/dist -o dist/slides.html
```

> **Note:** `--theme-set` is a greedy CLI option, so the input path (`src/slides.md`)
> must come **first** or it gets swallowed by the flag.

### VS Code preview

The `--theme-set` flag only wires up the **CLI** builds — it does nothing for the
**VS Code Marp preview** (the [Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)
extension). The preview is configured separately via `.vscode/settings.json`, which is
committed so the preview works out of the box:

```jsonc
{
  // Register each kit theme explicitly — the extension accepts files, not directories,
  // so the whole dist/ can't be pointed at like --theme-set does.
  "markdown.marp.themes": [
    "./node_modules/marp-theme-kit/dist/base.css",
    "./node_modules/marp-theme-kit/dist/brand.css",
    "./node_modules/marp-theme-kit/dist/academic.css",
    "./node_modules/marp-theme-kit/dist/graph-paper.css",
    "./node_modules/marp-theme-kit/dist/palladium.css",
    "./node_modules/marp-theme-kit/dist/rose-pine.css",
    "./node_modules/marp-theme-kit/dist/rose-pine-dawn.css"
  ],
  // Enable raw HTML (e.g. the col-break <div>) in the preview. "all" is required:
  // "default" only allows Marp's built-in HTML allowlist, which excludes <div>.
  "markdown.marp.html": "all"
}
```

Two details matter:

- **`base.css` must be listed.** The custom themes do `@import 'base'`, which Marp
  resolves by the registered theme _name_ (`base`), not by a relative file path. If
  `base.css` isn't registered, `theme: brand` renders without the layout contract in the
  preview.
- **`markdown.marp.html: "all"` is required** for the `col-break` utility's raw
  `<div class="col-break"></div>` to render in the preview. This setting is an enum
  (`"off"` | `"default"` | `"all"`); `"default"` only permits Marp's built-in HTML
  allowlist, which does **not** include `<div>`, so only `"all"` enables `col-break`. The
  CLI already gets HTML from the `marp.html: true` block in `package.json`; this brings the
  preview to parity. Note that `"all"` only takes effect in a **trusted** workspace —
  [VS Code Workspace Trust](https://code.visualstudio.com/docs/editor/workspace-trust)
  forces it to `"off"` in untrusted workspaces — and it renders any raw HTML in a deck, so
  leave decks from untrusted sources untrusted.

### Switching themes

Pick a theme by name in the front-matter of `src/slides.md`:

```yaml
---
marp: true
theme: brand
---
```

Available themes from the kit:

| Theme            | Kind   | Layout classes |
| ---------------- | ------ | -------------- |
| `brand`          | Custom | Full support   |
| `base`           | Foundation (imported by custom themes; not usually selected directly) | — |
| `rose-pine`      | Skin   | Skin only      |
| `rose-pine-dawn` | Skin   | Skin only      |
| `graph-paper`    | Skin   | Skin only      |
| `academic`       | Skin   | Skin only      |
| `palladium`      | Skin   | Skin only      |

The vendored skins restyle colors and type but do **not** implement the layout contract
below — those classes are a feature of the custom `brand` theme.

## Layout contract

The `brand` theme exposes a small set of reusable slide layouts. Apply one per slide with
a `_class` directive (`<!-- _class: name -->`). A plain title + bullets slide needs no
class — that is the unstyled baseline.

| `_class`    | Use for                       | Expected markup                                              |
| ----------- | ----------------------------- | ----------------------------------------------------------- |
| `lead`      | Centered title slide          | `# Title` + a paragraph subtitle                            |
| `statement` | One big centered punchline     | `# A bold **statement**`                                    |
| `cols-2`    | Two-column flow               | `# Heading` then a single bullet list (flows into 2 cols)   |
| `cols-3`    | Three-column flow             | `# Heading` then a single bullet list (flows into 3 cols)   |
| `compare`   | Two labeled columns           | `# Title`, then exactly two `## Label` headings each with a list |
| `closing`   | Quiet thank-you / "Questions?" | `# Questions?`                                              |
| `tight`     | Utility: shrink type for dense slides | Combine with a layout, e.g. `<!-- _class: cols-2 tight -->` |
| `invert`    | Utility: dark-mode token flip  | `class: invert` (front-matter) or `<!-- _class: invert -->`, combinable e.g. `_class: cols-2 invert` |
| `top`       | Utility: pin content to top    | `<!-- _class: top -->`, combinable e.g. `_class: top center-x` |
| `center-x`  | Utility: center text horizontally | `<!-- _class: center-x -->`, combinable e.g. `_class: top center-x` |

### Utilities

The kit adds a set of small, combinable utilities on top of the layouts above:

- **`tight`** — shrinks type so dense slides fit. Stack it on any layout, e.g.
  `<!-- _class: cols-2 tight -->`.
- **`invert`** — dark mode. Flips the brand tokens (the accent stays). Apply per slide
  with `<!-- _class: invert -->`, globally via front-matter `class: invert`, or combine
  with a layout, e.g. `<!-- _class: cols-2 invert -->`.
- **`col-break`** — forces a manual column break inside `cols-2` / `cols-3`. Split the
  content into separate lists and drop an empty `<div class="col-break"></div>` where the
  next column should start. This is the one place a `<div>` is needed, and it requires
  HTML to be enabled (`marp.html: true` for builds, `markdown.marp.html: "all"` for
  the VS Code preview — both already set up here).
- **`top`** — pins content to the top of the slide instead of vertically centered. Works
  even on full-bleed `![bg]` image slides. Combine with any layout, e.g.
  `<!-- _class: top -->`.
- **`center-x`** — centers text horizontally (the baseline is left-aligned). Combine with
  `top` for a top-center slide: `<!-- _class: top center-x -->`. Note that `-` list bullet
  markers stay left-pinned, so prefer headings/paragraphs over bullet lists when centering.

Notes:

- `cols-2` / `cols-3` use real CSS columns — write one normal list and headings span all
  columns automatically. No `<div>` wrappers needed.
- `compare` expects **exactly two** `##` headings (the second one breaks into the right
  column). Don't nest extra `##` subheads inside a compare column.
- Image-background slides use Marp's built-in directives, e.g.
  `![bg right:40%](images/foo.jpg)` or `![bg fit](images/bar.jpg)`, and work with any
  theme.
- Suppress pagination on individual slides (e.g. title / closing) with
  `<!-- _paginate: false -->`.

`src/slides.md` ships as a worked example of every layout above.

## License

MIT — see [LICENSE](LICENSE).
