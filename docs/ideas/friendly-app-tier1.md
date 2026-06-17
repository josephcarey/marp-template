# Friendly Marp App — Tier 1 Scope (idea, not scheduled)

> Status: captured idea, NOT in progress. A separate future effort from the theme kit.
> Goal: make Marp approachable for non-programmers without abandoning Marp's markdown core.

## Problem
Marp is excellent via CLI + VS Code preview, but that workflow intimidates non-programmers
(installing Node/CLI, the terminal, YAML frontmatter, `_class` directives, typing image file
paths, the build/export step). We want a friendlier on-ramp.

## Research finding (validated 2026-06)
No maintained, friendly, standalone Marp app exists — and the Marp team deliberately doesn't
ship one:
- `web.marp.app` (their own web editor) is now INACTIVE and redirects users to vscode.dev + the
  Marp extension.
- The "classic" Marp desktop app (Electron, ~2016) is archived/dead since ~2018 (their word: a
  "gravesite").
- The official ecosystem is intentionally libraries + integrations only (Marpit, Marp Core,
  Marp CLI, VS Code extension). `marp-react`/`marp-vue` wrappers exist but are inactive.
- Alternatives people get pointed to don't fit: vscode.dev+extension (still "VS Code"), HackMD
  (reveal.js, not Marp), Deckset (Mac/paid/closed, not Marp), DeckDeckGo (sunset).

Conclusion: the niche is genuinely open AND the Marp team won't compete in it. The one caution
is WHY they stepped back — desktop GUI maintenance burden (old Electron, dependency churn).
Mitigations: use a lighter stack (Tauri) and stay strictly Tier 1.

## Key technical unlock
`@marp-team/marp-core` is the actively-maintained, embeddable render library that powers the CLI
and the VS Code preview. A custom app embeds it directly for live preview (no need for the dead
react/vue wrappers, no CLI for preview). The CLI is only needed for PDF/PPTX export (headless
Chromium) — the single heaviest dependency.

## Scope — Tier 1 "guided markdown" (the sweet spot)
A split-view editor with a toolbar that hides the scary parts. Explicitly NOT a WYSIWYG engine.

MVP features:
- Split view: friendly text editor (left) + live marp-core preview (right), re-rendering on edit.
- Toolbar that manages the intimidating bits:
  - "New slide" (inserts `---`)
  - Layout dropdown -> inserts `<!-- _class: lead|statement|cols-2|cols-3|compare|closing -->`
    (driven by the marp-theme-kit LAYOUT CONTRACT)
  - Theme picker -> sets frontmatter `theme:` (driven by the marp-theme-kit theme set)
  - Drag-drop image -> copies the file into the project and inserts the path automatically
  - One "Export" button -> HTML / PDF / PPTX
- Frontmatter is managed by the UI, never hand-typed.

## Synergy with marp-theme-kit (why this compounds)
This app is essentially a friendly UI over THIS kit:
- The layout contract (lead/statement/cols-2/cols-3/compare/closing/tight) becomes the layout dropdown.
- The vendored + custom themes become the theme picker.
- The kit's `dist/` theme set is what the app registers for preview/export.

## Recommended stack
Tauri desktop app with a Svelte front end (matches the studio stack; double-click install, no
terminal/Node for the user, native file access for images, can bundle marp-cli for export).
A pure web app stumbles on local files + PDF/PPTX export.

## Hard parts / risks
1. PDF/PPTX export needs Chromium — either bundle marp-cli (fat installer) or use the system
   webview's print-to-PDF (lighter, but PPTX still needs marp-cli).
2. Image path management (the drag-drop-copies-file flow) is fiddly but high value.
3. Scope discipline — resist creeping toward Tier 2/3.
4. Long-term maintenance burden (the reason the Marp team backed out). Keep the surface thin:
   marp-core does the rendering; we maintain a UI shell, not an engine.

## Explicitly out of scope (for Tier 1)
- Tier 2: slide-thumbnail navigator, per-slide property panel.
- Tier 3: true block/WYSIWYG editing with markdown round-tripping (a research problem; wrong
  fight on top of Marp — decided against).
- Collaboration / cloud / accounts.

## Rough estimate
Usable prototype in days; polished Tier 1 in ~2-3 weeks. (Separate effort; not scheduled.)

## Open questions (decide if/when we start)
- Export: bundle marp-cli vs. system-webview print-to-PDF (PPTX likely forces marp-cli).
- How the app consumes the theme kit: vendored copy of `dist/` vs. the github dependency.
- Target OS priority (macOS first?).
- App name.
