---
marp: true
theme: brand
size: 16:9
paginate: true
---

<!-- _class: lead -->
<!-- _paginate: false -->

# Theme Kit Showcase

A kitchen-sink tour of every `brand` layout and utility — one slide each, with a
copy-paste snippet.

---

<!-- _class: lead -->

# `lead`

Centered title slide: a big `# Title` plus a paragraph subtitle.

```md
<!-- _class: lead -->

# Title

Subtitle paragraph
```

---

<!-- _class: statement -->

# One big **statement**

```md
<!-- _class: statement -->

# One big **statement**
```

---

<!-- _class: cols-2 -->

# `cols-2`

One list flows into two real CSS columns — headings span all columns.

```md
<!-- _class: cols-2 -->

# Heading

- item …
```

- Item 1
- Item 2
- Item 3
- Item 4
- Item 5
- Item 6

---

<!-- _class: cols-3 -->

# `cols-3`

Same idea as `cols-2`, but three columns.

```md
<!-- _class: cols-3 -->

# Heading

- item …
```

- Item 1
- Item 2
- Item 3
- Item 4
- Item 5
- Item 6
- Item 7
- Item 8
- Item 9

---

<!-- _class: cols-2 -->

# `col-break`

Force the next column with an empty `<div class="col-break"></div>` (needs HTML
enabled). Split the content into separate lists around the break.

- Stays in column one
- Also column one

<div class="col-break"></div>

```md
- left column
<div class="col-break"></div>
- right column
```

- Pushed to column two
- Also column two

---

<!-- _class: compare -->

# `compare`

## Left label

- exactly two `##` headings
- each with its own list

## Right label

```md
<!-- _class: compare -->
# Title
## Left
- …
## Right
- …
```

- the second `##` breaks to the right

---

<!-- _class: tight -->

# `tight`

A utility that shrinks type for dense slides. Stack it on any layout, e.g.
`<!-- _class: cols-2 tight -->`.

```md
<!-- _class: tight -->
```

- Point 1
- Point 2
- Point 3
- Point 4
- Point 5
- Point 6
- Point 7
- Point 8

---

<!-- _class: top -->

# `top`

Utility: pins content to the top instead of vertically centering. Combine with
any layout, e.g. `<!-- _class: top center-x -->`.

```md
<!-- _class: top -->
```

- Content hugs the top edge

---

<!-- _class: top center-x -->

# `center-x`

Utility: centers text horizontally (shown here combined with `top`). Prefer
headings/paragraphs — list bullets stay left-pinned.

```md
<!-- _class: top center-x -->
```

This paragraph is centered horizontally.

---

<!-- _class: invert -->

# `invert`

Dark-mode utility: flips the brand tokens (accent stays). Per slide here, or
deck-wide via front-matter `class: invert`.

```md
<!-- _class: invert -->        # one slide
class: invert                  # whole deck (front-matter)
```

- Combinable with any layout, e.g. `_class: cols-2 invert`

---

# `<!-- fit -->` auto-scaling

> **Requires kit ≥ the auto-scaling release** (expected v0.4.0). Won't render as
> intended on the pinned `v0.3.0` — bump the `marp-theme-kit` pin first.

Scales a heading (and code/math) to fit the slide:

```md
# <!-- fit --> Big Auto-Sized Title
```

---

<!-- _class: closing -->
<!-- _paginate: false -->

# Questions?

```md
<!-- _class: closing -->
```
